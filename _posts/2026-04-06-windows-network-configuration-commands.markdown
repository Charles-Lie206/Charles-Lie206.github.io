---
layout: single
title: "Windows Network Configuration Commands: From Beginner to Master"
date: 2026-04-06 20:00:00 +0800
categories: Windows
tags: [Windows, networking, CMD, network administration]
read_time: true
---

In daily network management, troubleshooting, or system configuration, Windows provides a series of powerful command-line tools. Besides the well-known `ipconfig` and the comprehensive `netsh`, there are many other commands that can help you precisely locate issues, configure networks, or monitor traffic. This article will organize the most commonly used network commands in Windows in blog form, explaining their purposes, basic usage methods, and providing typical examples.

---

## 1. Network Connectivity Testing

### 1. `ping` – Basic Connectivity Test

**Purpose**  
Sends ICMP Echo requests to a target host to detect network reachability, latency, and packet loss rate.

**Usage**  
```cmd
ping [-t] [-a] [-n count] [-l size] [-w timeout] target
```

**Common Examples**  
- Continuous ping until manually stopped (Ctrl+C)  
  ```cmd
  ping -t 8.8.8.8
  ```
- Send 5 packets, each 1500 bytes  
  ```cmd
  ping -n 5 -l 1500 www.baidu.com
  ```
- Resolve hostname and display IP  
  ```cmd
  ping -a 192.168.1.1
  ```

---

### 2. `tracert` – Route Tracing

**Purpose**  
Displays each router hop that packets pass through from the local machine to the target host (achieved by incrementing TTL), used to locate network latency points or interruption locations.

**Usage**  
```cmd
tracert [-d] [-h maximum_hops] target
```

**Common Examples**  
- Trace route to Baidu without resolving hostnames (faster)  
  ```cmd
  tracert -d www.baidu.com
  ```
- Specify maximum hops as 20  
  ```cmd
  tracert -h 20 8.8.8.8
  ```

---

### 3. `pathping` – Combining ping and tracert

**Purpose**  
Combines the functionality of `ping` and `tracert`: first displays the route path, then performs continuous ICMP statistics on each hop, providing detailed reports on packet loss and latency. Suitable for analyzing network quality.

**Usage**  
```cmd
pathping [-n] [-h max_hops] target
```

**Common Example**  
```cmd
pathping -n 192.168.1.1
```

---

### 4. `telnet` – Port Connectivity Testing

**Purpose**  
Although traditionally used for remote login, it's often used to test whether a specific port on a target host is open (such as HTTP, SMTP, etc.).

> **Note**: Telnet client is not installed by default on Windows, needs to be added in "Turn Windows features on or off".

**Usage**  
```cmd
telnet host port
```

**Common Examples**  
- Test if port 80 is accessible  
  ```cmd
  telnet www.example.com 80
  ```
- Test SMTP port 25  
  ```cmd
  telnet mail.example.com 25
  ```
If the connection succeeds, the window will clear and enter interactive mode; if it fails, it will prompt "Could not open connection to the host".

---

## 2. Network Configuration and Viewing

### 5. `ipconfig` – IP Configuration Viewing (Previously introduced, supplementing)

**Purpose**  
Displays TCP/IP configuration, including IP address, subnet mask, default gateway, and can refresh DHCP leases and DNS cache.

**Supplementary Examples**  
- Display detailed configuration for all interfaces (including MAC address, DHCP server, etc.)  
  ```cmd
  ipconfig /all
  ```
- Renew DHCP lease  
  ```cmd
  ipconfig /renew
  ```
- Clear DNS cache  
  ```cmd
  ipconfig /flushdns
  ```

---

### 6. `getmac` – Get MAC Address

**Purpose**  
Quickly displays the physical address (MAC) of network interfaces, useful for network admission control or device identification.

**Usage**  
```cmd
getmac [/s computer [/u user [/p password]]] [/fo format] [/v]
```

**Common Examples**  
- Display local MAC address  
  ```cmd
  getmac
  ```
- Display detailed information in CSV format  
  ```cmd
  getmac /fo csv /v
  ```

---

### 7. `arp` – Address Resolution Protocol Table

**Purpose**  
View and modify local ARP cache (mapping of IP addresses to MAC addresses). Often used to detect ARP spoofing or manually bind static entries.

**Usage**  
```cmd
arp [-a [inet_addr]] [-d inet_addr] [-s inet_addr eth_addr]
```

**Common Examples**  
- Display current ARP table  
  ```cmd
  arp -a
  ```
- Add static ARP entry (bind gateway IP to MAC)  
  ```cmd
  arp -s 192.168.1.1 00-11-22-33-44-55
  ```
- Delete ARP entry for specified IP  
  ```cmd
  arp -d 192.168.1.1
  ```

---

### 8. `route` – Routing Table Management

**Purpose**  
View and modify local IP routing table, including adding, deleting, or modifying route entries.

**Usage**  
```cmd
route [-f] [-p] [command] [destination] [mask netmask] [gateway] [metric metric]
```

**Common Examples**  
- Display complete routing table  
  ```cmd
  route print
  ```
- Add permanent static route (`-p` makes it effective after restart)  
  ```cmd
  route -p add 192.168.2.0 mask 255.255.255.0 192.168.1.1
  ```
- Delete route  
  ```cmd
  route delete 192.168.2.0
  ```

---

### 9. `nslookup` – DNS Query Tool

**Purpose**  
Query the IP address corresponding to a domain name, or reverse resolve, and obtain DNS records (A, MX, NS, etc.). Used for diagnosing DNS resolution issues.

**Usage**  
```cmd
nslookup [-type=type] [domain] [dns-server]
```

**Common Examples**  
- Query A record  
  ```cmd
  nslookup www.google.com
  ```
- Query MX record (mail exchange)  
  ```cmd
  nslookup -type=mx example.com
  ```
- Query using specified DNS server  
  ```cmd
  nslookup www.baidu.com 8.8.8.8
  ```
- Enter interactive mode  
  ```cmd
  nslookup
  > set type=ptr
  > 8.8.8.8
  ```

---

### 10. `certutil` – Certificate and SSL/TLS Diagnostics (Not purely network, but commonly used)

**Purpose**  
Although primarily a certificate management tool, it can be used to check certificate information for HTTPS connections, test SSL connections, etc.

**Common Examples**  
- View website certificate chain  
  ```cmd
  certutil -urlfetch -verify https://www.microsoft.com
  ```
- Display system certificate store  
  ```cmd
  certutil -store my
  ```

---

## 3. Network Connections and Statistics

### 11. `netstat` – Network Statistics

**Purpose**  
Displays active TCP/UDP connections, port listening status, routing table, interface statistics, etc., is the preferred tool for troubleshooting port occupancy and network connections.

**Usage**  
```cmd
netstat [-a] [-b] [-n] [-o] [-p proto] [-r] [-s] [interval]
```

**Common Examples**  
- Display all connections and listening ports (including process PID)  
  ```cmd
  netstat -ano
  ```
- Display Ethernet statistics (bytes sent/received, etc.)  
  ```cmd
  netstat -e
  ```
- Refresh connections every 2 seconds  
  ```cmd
  netstat -n 2
  ```
- Display routing table (similar to `route print`)  
  ```cmd
  netstat -r
  ```

---

### 12. `nbtstat` – NetBIOS Statistics

**Purpose**  
View NetBIOS-based TCP/IP information, such as NetBIOS name tables of local or remote computers. Still useful in old networks or Windows sharing troubleshooting.

**Usage**  
```cmd
nbtstat [-a remotename] [-A IPaddress] [-c] [-n]
```

**Common Examples**  
- Display NetBIOS name table based on IP  
  ```cmd
  nbtstat -A 192.168.1.10
  ```
- Display local NetBIOS cache  
  ```cmd
  nbtstat -c
  ```

---

### 13. `net` – Network Services and Resource Sharing

**Purpose**  
The `net` command is a large command family for managing network resources, users, services, shares, etc. Here are only common network-related subcommands.

**Common Subcommands**  
- View shared resources  
  ```cmd
  net share
  ```
- Map network drive  
  ```cmd
  net use Z: \\server\share
  ```
- View network connection list  
  ```cmd
  net use
  ```
- Start/Stop service  
  ```cmd
  net start "DNS Client"
  net stop "DNS Client"
  ```
- View computer's local groups  
  ```cmd
  net localgroup
  ```

---

## 4. Advanced Diagnostics and Repair

### 14. `netsh` – Network Shell (Previously introduced in detail, supplementing)

**Supplementary Examples**  
- Reset Winsock (fix network connection issues)  
  ```cmd
  netsh winsock reset
  ```
- Reset TCP/IP stack  
  ```cmd
  netsh int ip reset
  ```
- Export firewall rules  
  ```cmd
  netsh advfirewall export "C:\backup\firewall.txt"
  ```

---

### 15. `tracert` and `pathping` Already Listed in Connectivity Testing

---

### 16. Network Commands in PowerShell

Although not traditional CMD commands, PowerShell provides rich network modules. For example, `Test-NetConnection` can replace `ping`, `tracert` and test ports. Briefly mentioned here:

```powershell
Test-NetConnection -ComputerName 8.8.8.8 -Port 80
```

---

## 5. Summary and Selection Recommendations

| Scenario | Recommended Command |
|----------|---------------------|
| View IP address | `ipconfig` |
| Check network connectivity | `ping` |
| Trace route | `tracert` |
| Analyze packet loss | `pathping` |
| View port usage | `netstat -ano` |
| Modify IP or DNS | `netsh interface ip` |
| Manage firewall rules | `netsh advfirewall` |
| DNS query | `nslookup` |
| Add static route | `route add` |
| View MAC address | `getmac` |
| Map network drive | `net use` |
| Reset network stack | `netsh winsock reset`, `netsh int ip reset` |
| Diagnose HTTPS certificate | `certutil` |

After mastering these commands, whether it's daily office network issues or server environment configuration, you'll be able to quickly locate and solve problems. I recommend noting commonly used commands on a sticky note and practicing more in actual scenarios to gradually form your own network troubleshooting routine.

---

*If you have deeper questions about a specific command, or want to see more practical cases, feel free to discuss in the comments!*
