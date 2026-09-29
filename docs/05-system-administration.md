# System Administration

## Overview

The lab includes Linux, Windows Server, Windows client, Active Directory, DNS, remote administration, service management, monitoring agents, and automated administrative workflows.

The objective is to practice system administration in an environment where servers and clients are connected through defined network segments and managed through centralized monitoring and automation.

---

## Infrastructure Systems

| System          | Address                          | Operating System      | Primary Role                         |
|-----------------|----------------------------------|-----------------------|--------------------------------------|
| Zabbix01        | `10.10.20.20`                    | Ubuntu 24.04 LTS      | Monitoring and automation controller |
| DC01            | `10.10.20.10`                    | Windows Server 2025   | Active Directory and DNS             |
| RHEL01          | `10.10.20.30`                    | RHEL 9.8              | Linux server                         |
| Windows 10      | `10.10.10.10`                    | Windows 10            | Client workstation                   |
| Kali Linux      | `10.10.10.11` / `172.16.50.20`   | Kali Linux            | Security-testing workstation         |
| Metasploitable2 | `172.16.50.10`                   | Metasploitable2       | Isolated security-testing target     |
| VyOS            | `10.10.20.1`                     | VyOS                  | Network router                       |
| pfSense         | `192.168.61.128` / `192.168.0.1` | pfSense               | Firewall and gateway                 |
| Proxmox         | `192.168.61.136`                 | Proxmox VE            | Virtualization platform              |

---

# 1. Zabbix01

## Role

Zabbix01 is the central management server for the lab.

It provides:

- Zabbix monitoring
- Ansible automation
- API-based infrastructure queries
- Local automation logs and backup storage

## Platform

```text
Operating System: Ubuntu 24.04 LTS
IP Address:       10.10.20.20/24
Gateway:          10.10.20.1
DNS:              10.10.20.10
