# Automation Architecture

## Purpose

This document describes the automation architecture implemented
within the Enterprise Infrastructure & Automation Lab.

The automation layer combines:

- Ansible
- Python
- Ansible Collections
- Network automation
- Windows/WinRM automation
- Linux administration
- Active Directory automation
- Proxmox API integration
- Zabbix API integration

The objective is to make infrastructure operations repeatable,
observable, validated, and easier to maintain.

---

# Automation Architecture

```text
                         ADMINISTRATOR
                              |
                              v
                    Automation Environment
                              |
                +-------------+-------------+
                |                           |
                v                           v
              Ansible                     Python
                |                           |
       +--------+---------+          +------+------+
       |        |         |          |             |
       v        v         v          v             v
     Linux   Windows    VyOS      Proxmox       Zabbix
       |        |         |         API           API
       |        |         |
       v        v         v
    Servers    AD      Network
    / Linux   /WinRM   Devices
       |        |         |
       +--------+---------+
                |
                v
        Infrastructure State
                |
                v
            Validation
                |
                v
            Monitoring
                |
                v
          Evidence / RCA
