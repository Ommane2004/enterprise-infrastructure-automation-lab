# Lab 01 — System Administration

## Objective

Build and operate a Linux-based server environment while developing
practical system administration skills.

The lab focuses on system configuration, user management, services,
networking, security, monitoring, troubleshooting, and documentation.

---

## Skills Demonstrated

- Linux system administration
- User and group management
- File permissions
- SSH administration
- Service management
- Package management
- Process management
- Disk and filesystem management
- Network configuration
- Firewall configuration
- System logging
- Troubleshooting
- Basic system hardening
- Monitoring

---

## Environment

### Operating System

- Linux

### Virtualization

- Virtualized lab environment

### Management

- SSH
- Linux command line

### Monitoring

- Zabbix

---

## Lab Architecture

The system will operate as part of the larger enterprise infrastructure
laboratory.

```text
Administrator
      |
      | SSH
      v
+------------------+
|   Linux Server   |
+------------------+
      |
      +---- Services
      |
      +---- Logs
      |
      +---- Network
      |
      +---- Monitoring
