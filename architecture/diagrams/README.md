# Architecture Diagrams

This directory contains the visual architecture documentation for
the Enterprise Infrastructure & Automation Lab.

The diagrams correspond to the implemented and documented
infrastructure architecture.

---

## Diagram Index

| #  | Diagram                         | Purpose                                                      |
|----|---------------------------------|--------------------------------------------------------------|
| 01 | Physical / Virtual Architecture | VMware virtualization and infrastructure layout              |
| 02 | Network Architecture            | VMware networks, pfSense, VyOS, routing and network segments |
| 03 | Automation Architecture         | Ansible, Python, APIs and infrastructure automation          |
| 04 | NOC / SOC Operational Workflow  | Monitoring, investigation, response and operational workflow |
| 05 | Monitoring Architecture         | Zabbix, agents, SNMP and monitoring relationships            |
| 06 | Security Architecture           | Network segmentation, security testing and security controls |

---

## 01 — Physical / Virtual Architecture

Shows the relationship between the physical host,
VMware Workstation Pro, virtual machines and virtual networks.

File:

`01-physical-virtual-architecture.png`

---

## 02 — Network Architecture

Shows:

- VMware NAT
- pfSense
- VyOS
- Transit network
- Internal client network
- Server / management network
- Isolated security network

File:

`02-network-architecture.png`

---

## 03 — Automation Architecture

Shows the relationship between:

- Ansible
- Python
- Linux
- Windows
- Active Directory
- VyOS
- Zabbix API
- Proxmox API

File:

`03-automation-architecture.png`

---

## 04 — NOC / SOC Operational Workflow

Shows the operational lifecycle from:

```text
Detection
    ↓
Validation
    ↓
Investigation
    ↓
Evidence Collection
    ↓
Root Cause
    ↓
Remediation
    ↓
Validation
    ↓
Documentation
