---
layout: single
title: "Complete Guide to Ubuntu Service Management: From Mechanisms to Commands"
date: 2026-04-06 19:00:00 +0800
categories: Linux
tags: [Ubuntu, systemd, service management, Linux administration]
read_time: true
---

In Ubuntu systems, service management is a fundamental skill for operations and development. Whether it's starting Nginx, restarting MySQL, or troubleshooting SSH connection issues, understanding service management tools is indispensable. This article will start from the underlying mechanisms and guide you through mastering every aspect of Ubuntu service management.

---

## 1. Introduction

Ubuntu's service management has evolved from **SysVinit** to **Upstart**, and then to **systemd**. Since Ubuntu 15.04, systemd has become the default init system. It not only manages services but also unifies the management of mount points, devices, scheduled tasks, and other system resources. Understanding systemd's design philosophy will help you use service management commands more efficiently and quickly locate the root cause when encountering issues.

---

## 2. Evolution of Service Management Mechanisms

### 2.1 SysVinit (Traditional System V init)
- Uses shell scripts under `/etc/init.d/` to control services.
- Switches states through runlevels, with service scripts relying on `# Required-Start:` comments to define startup order.
- **Disadvantages**: Serial startup, slow speed; dependency relationships maintained manually, prone to errors.

### 2.2 Upstart (Ubuntu 6.10–14.10)
- Event-driven, supports on-demand startup and parallelization.
- Configuration files located in `/etc/init/` (`.conf` files).
- Although efficiency improved, it didn't become the unified standard for Linux distributions.

### 2.3 systemd (Ubuntu 15.04 and later)
- Fully parallel startup, uses **units** to abstract all resources.
- Defines dependencies and startup behavior through declarative unit files, dependency tree automatically resolved.
- Provides unified management tools like `systemctl`, `journalctl`, and maintains backward compatibility with `service` commands.

---

## 3. Core Concepts of systemd

### 3.1 Units
systemd abstracts managed objects as units, with each unit type distinguished by a suffix:

| Suffix | Purpose | Example |
|--------|---------|---------|
| `.service` | Regular services | `nginx.service` |
| `.target` | Collection of units (similar to runlevels) | `multi-user.target` |
| `.socket` | Network or local socket, for on-demand activation | `sshd.socket` |
| `.mount` | File system mount point | `home.mount` |
| `.timer` | Timer (cron alternative) | `backup.timer` |

Unit file storage locations:
- `/usr/lib/systemd/system/` – Shipped with system software packages
- `/etc/systemd/system/` – User-defined or overrides (higher priority)

### 3.2 Dependencies
Relationships between units are defined through explicit directives, and systemd builds a dependency tree based on these for efficient parallel startup:

- `Requires=`: Strong dependency, dependency must start before this unit.
- `Wants=`: Weak dependency, only attempts to start, failure doesn't affect this unit.
- `After=` / `Before=`: Only defines startup order, doesn't enforce dependency.
- `Conflicts=`: Mutual exclusion, cannot run simultaneously.

### 3.3 Targets
Targets are collections of units used to represent system states (like traditional runlevels). Common targets:

| Target | Corresponds to old runlevel | Description |
|--------|---------------------------|-------------|
| `poweroff.target` | 0 | Shutdown |
| `rescue.target` | 1 | Single-user mode |
| `multi-user.target` | 3 | Multi-user text mode |
| `graphical.target` | 5 | Multi-user graphical mode |
| `reboot.target` | 6 | Reboot |

Switch to target:
```bash
sudo systemctl isolate multi-user.target   # Enter text mode
```

### 3.4 Service States
Detailed service status can be seen through `systemctl status`:

- `active (running)` – Running normally
- `active (exited)` – One-time task, exited after execution
- `active (waiting)` – Waiting for an event (e.g., socket activation)
- `inactive` – Not running
- `failed` – Startup failed

---

## 4. Detailed Explanation of systemctl Commands

`systemctl` is the primary tool for interacting with systemd. Mastering these commands allows you to complete daily service management tasks.

### 4.1 Basic Operations
```bash
# Start service
sudo systemctl start <service-name>

# Stop service
sudo systemctl stop <service-name>

# Restart service (stop then start)
sudo systemctl restart <service-name>

# Reload configuration (without interrupting service)
sudo systemctl reload <service-name>

# View service status
systemctl status <service-name>
```

### 4.2 Boot Auto-Start Management
```bash
# Enable boot auto-start (create symbolic link to corresponding target.wants/)
sudo systemctl enable <service-name>

# Disable boot auto-start
sudo systemctl disable <service-name>

# Check if enabled
systemctl is-enabled <service-name>
```

### 4.3 Viewing Service List
```bash
# List all loaded service units
systemctl list-units --type=service

# List all installed services (including non-running ones)
systemctl list-unit-files --type=service

# View only running services
systemctl list-units --type=service --state=running
```

### 4.4 Viewing Service Logs
```bash
# View full logs
sudo journalctl -u <service-name>

# Real-time tracking
sudo journalctl -u <service-name> -f

# Show last 50 lines
sudo journalctl -u <service-name> -n 50
```

### 4.5 Advanced Operations
```bash
# Reload systemd configuration (must execute after modifying unit files)
sudo systemctl daemon-reload

# Mask service (prevent manual or automatic startup)
sudo systemctl mask <service-name>

# Unmask service
sudo systemctl unmask <service-name>
```

---

## 5. service Command and Traditional Compatibility

The `service` command is a simple wrapper around `systemctl`, suitable for users accustomed to SysVinit usage.

```bash
sudo service <service-name> <action>
```

Supported actions: `start`, `stop`, `restart`, `reload`, `status`.

Examples:
```bash
sudo service nginx start
sudo service nginx status
```

> **Note**: `service` doesn't support `enable`/`disable`; these functions still require `systemctl`.

---

## 6. Best Practices and Notes

1. **Always use `sudo`**: Service management requires root privileges.
2. **Omit .service suffix**: `systemctl start nginx` is equivalent to `systemctl start nginx.service`.
3. **After modifying unit files**: Must execute `sudo systemctl daemon-reload` to make changes take effect.
4. **Use logs to locate issues**: When a service cannot start, first check `journalctl -u <service-name>` for detailed error information.
5. **Avoid directly editing /usr/lib/systemd/system/**: If you need to override configuration, place the modified unit file in `/etc/systemd/system/`.
6. **Use mask appropriately**: For services you don't want to be accidentally started by other dependencies, `mask` can completely disable them.

---

## 7. Summary

Ubuntu's service management is centered on **systemd**, defines services through **unit files**, and achieves efficient startup and management through **dependency relationships** and **on-demand activation**. `systemctl` is the standard tool for interacting with it, with comprehensive functionality and easy script writing. Although `service` commands are still available, it's recommended to master `systemctl` to fully leverage systemd's powerful capabilities.

From understanding mechanisms to熟练 using commands, I hope this article helps you manage Ubuntu services with more confidence. If you have deeper needs (such as writing your own service units, customizing targets, or debugging startup issues), feel free to discuss and explore in the comments.

---

*This article is written based on Ubuntu 22.04 LTS and applies to all modern Ubuntu versions using systemd.*
