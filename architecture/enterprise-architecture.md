# Enterprise Architecture

## Purpose

This document describes the actual architecture of the
Enterprise Infrastructure & Automation Lab.

The environment is built primarily using VMware Workstation Pro
and provides an isolated infrastructure environment for practicing:

- Network engineering
- Linux administration
- Windows Server administration
- Active Directory
- DNS
- NOC operations
- Infrastructure monitoring
- Ansible automation
- Network automation
- Virtualization
- API automation
- Troubleshooting
- Incident response
- Cybersecurity

The architecture is implemented incrementally and documented
against the actual lab environment.

---

## High-Level Architecture

```text
                              INTERNET
                                  |
                           VMware NAT Network
                           192.168.61.0/24
                                  |
                               pfSense
                         Firewall / Gateway / NAT
                                  |
                             192.168.0.0/24
                             pfSense ↔ VyOS
                                  |
                                VyOS
                         Internal Router
                         /             \
                        /               \
                       /                 \
              10.10.10.0/24          10.10.20.0/24
              Internal Client        Server / Management
                    Network                Network
                       |                     |
                 +-----+-----+        +------+-------+
                 |           |        |      |       |
              Windows 10   Kali      DC01  Zabbix  RHEL01
                .10         .11       .10    .20     .30


                    ISOLATED SECURITY NETWORK
                         172.16.50.0/24
                              |
                       +------+------+
                       |             |
                      Kali      Metasploitable2
                      .20              .10
