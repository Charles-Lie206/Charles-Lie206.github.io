---
layout: single
title: "Deep Understanding of Windows Network Routing: From ipconfig to VPN Traffic Forwarding"
date: 2026-04-06 20:30:00 +0800
categories: Windows
tags: [Windows, networking, VPN, routing, network administration]
read_time: true
---

In daily computer use, we often encounter network configuration-related issues. Whether troubleshooting network faults or wanting to understand how VPN works, mastering the output of `ipconfig` and `route print` is an essential skill. This article will start with a practical case, gradually analyze network adapter configuration and routing table structure, deeply explore the working principle of VPN virtual gateways, and finally organize some commonly used network commands.

---

## 1. Viewing Network Adapters from ipconfig

Running `ipconfig` allows you to view the basic configuration of all network adapters. Here's a typical output:

```
Unknown adapter LetsTAP:
   IPv4 Address. . . . . . . . . . . : 26.26.26.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.248
   Default Gateway . . . . . . . . . :

Ethernet adapter 以太网:
   Media State . . . . . . . . . . . : Media disconnected

Ethernet adapter 以太网 2:
   IPv4 Address. . . . . . . . . . . : 7.128.67.43
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   IPv4 Address. . . . . . . . . . . : 7.128.116.155
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   ...

Wireless LAN adapter WLAN:
   IPv4 Address. . . . . . . . . . . : 192.168.0.108
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.0.1
```

### Adapter Interpretation

- **LetsTAP (Virtual Network Card)**: This is a virtual network adapter created by VPN software. It's assigned IP `26.26.26.1/29` but has no default gateway. This indicates the VPN client is installed but hasn't taken over all traffic yet (or is in standby mode).
- **Ethernet (Wired Network Card)**: Shows "Media disconnected", indicating the network cable is not connected or the physical link is down.
- **Ethernet 2 (Second Wired Network Card)**: Binds multiple IP addresses, belonging to different subnets. Usually used to connect to specific internal networks, but there's no default gateway here, so it won't be used for internet access.
- **WLAN (Wireless Network Card)**: This is the physical network card currently used for internet. It has IP `192.168.0.108` and default gateway `192.168.0.1` (home router).

Through `ipconfig`, we can quickly understand the status and basic IP information of each network card, but to know exactly how traffic flows, we must look at the **routing table**.

---

## 2. Deep Analysis of Routing Table (route print)

Running `route print` displays the system's current routing rules. Here are key excerpts:

```
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0      192.168.0.1    192.168.0.108     45
          0.0.0.0        128.0.0.0       26.26.26.3       26.26.26.1      1
        128.0.0.0        128.0.0.0       26.26.26.3       26.26.26.1      1
       26.26.26.0  255.255.255.248         On-link        26.26.26.1    257
       26.26.26.1  255.255.255.255         On-link        26.26.26.1    257
      192.168.0.0    255.255.255.0         On-link     192.168.0.108    301
    192.168.0.108  255.255.255.255         On-link     192.168.0.108    301
...
```

### 1. Default Route (0.0.0.0/0)
- First: `0.0.0.0` `0.0.0.0` gateway `192.168.0.1`, interface `192.168.0.108`, metric 45. This is a standard default route, used to send all traffic with unknown destination IPs to the router.
- Second, third: `0.0.0.0` `128.0.0.0` and `128.0.0.0` `128.0.0.0`, gateway `26.26.26.3`, interface `26.26.26.1`, metric 1. These two routes together cover the entire IPv4 address space (`0.0.0.0/1` + `128.0.0.0/1` = `0.0.0.0/0`). Due to the very low metric value, they will be matched first.

**Conclusion**: All internet traffic (e.g., accessing 8.8.8.8) will first match these two routes added by VPN, thereby handing packets to the virtual network card `26.26.26.1` with gateway `26.26.26.3`.

### 2. Local Subnet Routes (On-link)
- Gateways for `26.26.26.0/29` and `192.168.0.0/24` both show as `On-link`. This indicates these networks are directly connected to the corresponding interfaces and don't need to pass through a router.
- For example, when accessing `192.168.0.50`, the system will send data frames directly within the local LAN through ARP, without passing through `192.168.0.1`.

### 3. Host Routes (/32)
- Entries like `26.26.26.1/32` and `192.168.0.108/32` specify the local machine's own IP addresses, and packets will be sent to the local loopback interface.

---

## 3. Role of VPN Virtual Gateway (26.26.26.3)

`26.26.26.3` is not a physical device but a **logical gateway** set by the VPN client in the virtual subnet `26.26.26.0/29`. When the operating system sends packets to `26.26.26.3` according to the routing table, the virtual network card driver will intercept these packets and pass them to the VPN client process (user-space program).

At this point, the VPN client performs the following on the original packets:
- **Encryption** (e.g., AES-256)
- **Encapsulation**: Wraps the encrypted data as the payload of a new UDP packet, with the new IP header's destination address being the VPN server's real public IP, and the source address being the physical network card's IP (e.g., `192.168.0.108`).

Then, the VPN client sends this new packet back into the network stack through the operating system's network API. The operating system queries the routing table again. Since the new packet's destination is the VPN server IP, and that IP has been excluded from the VPN tunnel by a **more specific route** (e.g., a host route), the new packet will go out through the physical network card.

### How to Avoid Loops?
If packets destined for the VPN server IP again hit the VPN route, it would form a dead loop. To prevent this, VPN software adds an **exclusion route** to the routing table, typically a `/32` host route for the VPN server IP, with the gateway pointing to the physical gateway (`192.168.0.1`) and a metric lower than the VPN route. This way, when the system sends the encapsulated packet, it will choose the physical network card, breaking the loop.

Although no explicit exclusion route is visible in your routing table, the VPN server's IP likely belongs to an existing directly connected network segment (e.g., `7.x.x.x`), and routes for these network segments (like `7.128.64.0/20`, etc.) are already directly connected through the physical network card, naturally serving the exclusion purpose.

---

## 4. What Does On-link Actually Mean?

In the routing table, when the "Gateway" column shows `On-link`, it indicates that the network destination doesn't need to be forwarded through any router but is directly connected to a certain interface. Packets will be sent directly from that interface and find the target on the local link (through ARP or direct sending).

Common scenarios:
- **Local subnet routes**: e.g., `192.168.0.0/24`, indicating hosts within the same LAN can communicate directly.
- **Host routes**: `/32` routes for the local machine's IP, indicating packets destined for the local machine go directly to the loopback.
- **Broadcast addresses**: Subnet broadcast addresses are also On-link.

Understanding On-link helps distinguish which traffic will pass through routers and which only interacts on the local link.

---

## 5. Complete Flow of VPN Traffic Forwarding

1. **Application initiates request**: Browser accesses `http://example.com`, system generates original packet with destination IP `93.184.216.34`.
2. **Routing decision**: System queries routing table, matches `0.0.0.0/1` or `128.0.0.0/1` added by VPN, hands packet to virtual network card (`26.26.26.1`), gateway `26.26.26.3`.
3. **Virtual network card interception**: Virtual network card driver copies packet from kernel to user-space VPN client.
4. **Encryption and encapsulation**: VPN client encrypts original packet, builds new UDP packet with destination as VPN server IP (e.g., `203.0.113.10`) and source as physical network card IP (`192.168.0.108`).
5. **Re-injection**: VPN client sends new packet back to operating system through standard sockets.
6. **Routing again**: Operating system queries routing table, destination `203.0.113.10` hits exclusion route (or existing directly connected route), hands packet to physical network card (WLAN), gateway `192.168.0.1`.
7. **Physical sending**: Physical network card sends encrypted packet to router, eventually reaching VPN server.
8. **VPN server forwarding**: VPN server decrypts packet, restores original request, accesses `example.com` on behalf, and returns encrypted results.

The entire process implements a "full tunnel" VPN, where all traffic is encrypted and forwarded through the VPN server.

---

## 6. Common Network Commands Summary

In daily troubleshooting, the following commands are very useful:

### 1. `ipconfig`
Displays IP addresses, subnet masks, and default gateways (if static gateway is configured) for all network adapters.
- `ipconfig /all`: Display more detailed information, including MAC address, DNS servers, DHCP status, etc.
- `ipconfig /release` and `ipconfig /renew`: Release and renew DHCP-assigned IP addresses (for physical network cards).

### 2. `route print`
Prints the current routing table.
- `route print -4`: Display only IPv4 routes.
- `route print -6`: Display only IPv6 routes.
- `route add` / `route delete`: Manually add or delete routes (requires administrator privileges). For example:  
  `route add 10.0.0.0 mask 255.0.0.0 192.168.1.1 metric 1`  
  Note: Are manually added routes persistent by default? No, need to add `-p` parameter.

### 3. `tracert`
Tracks routing nodes packets pass through to reach the target.
- For example `tracert 8.8.8.8`, if the first hop is `26.26.26.3`, it indicates traffic is passing through VPN virtual gateway; if it's `192.168.0.1`, it means VPN hasn't taken over.

### 4. `ping`
Tests connectivity with the target.
- `ping -n 10 8.8.8.8`: Send 10 ICMP requests.
- `ping -l 1400 8.8.8.8`: Set packet size (used to detect MTU issues).

### 5. `netsh`
Network configuration tool with powerful functionality.
- `netsh interface ip show config`: View interface IP configuration.
- `netsh winsock reset`: Reset Winsock catalog (fix network stack issues).
- `netsh int ip reset`: Reset TCP/IP stack.

### 6. `arp -a`
View ARP cache, understand the correspondence between IP and MAC addresses on the local link.

---

## 7. Conclusion

Through the output of `ipconfig` and `route print`, we can clearly see the configuration of network adapters and the system's routing strategy. VPN cleverly implements traffic encryption and forwarding by using virtual network cards and dynamic routing tables. Understanding these principles not only helps in troubleshooting network issues but also allows for a deeper grasp of the operating system's network stack working mechanism.
