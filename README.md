# Enterprise Infrastructure Automation, Monitoring & Security Lab

A production-inspired enterprise infrastructure lab designed to demonstrate
practical skills in network engineering, system administration, automation,
monitoring, cybersecurity, troubleshooting, and incident response.

A hands-on enterprise-style infrastructure lab built with VMware virtualization to practice and demonstrate:

- Network engineering
- Linux and Windows administration
- Network device automation
- Infrastructure monitoring
- Configuration backup and validation
- Active Directory administration
- SNMP monitoring
- API-based infrastructure management
- NOC/SOC operational workflows
- Cybersecurity and incident-response fundamentals

This project is designed as a practical engineering environment rather than a collection of isolated tutorials. The lab connects networking, systems administration, automation, monitoring, virtualization, and security into one operational environment.

---

## Objectives

The primary objectives of this lab are to:

1. Build an enterprise-style virtual network.
2. Practice routing, switching concepts, addressing, DNS, DHCP, and segmentation.
3. Automate infrastructure tasks using Ansible.
4. Monitor infrastructure using Zabbix.
5. Manage Windows Server and Active Directory.
6. Monitor network devices using SNMP.
7. Integrate Proxmox virtualization into infrastructure monitoring.
8. Practice configuration backup and health validation.
9. Simulate NOC/SOC troubleshooting workflows.
10. Document failures, fixes, incidents, and lessons learned.

---

# Architecture

## Physical / Virtual Architecture

The environment is hosted on VMware and consists of multiple isolated virtual networks.

![Physical and Virtual Architecture](architecture/diagrams/01-physical-virtual-architecture.png)

## Network Architecture

The lab contains separate transit, client, server, and security networks.

![Network Architecture](architecture/diagrams/02-network-architecture.png)

## Automation Architecture

Ansible automation is executed from the Zabbix01 management server and controls Linux, Windows, and VyOS infrastructure.

![Automation Architecture](architecture/diagrams/03-automation-architecture.png)

## NOC / SOC Operational Workflow

Monitoring, alerting, investigation, automation, remediation, and documentation are treated as connected operational activities.

![NOC/SOC Operational Workflow](architecture/diagrams/04-noc-soc-operational-workflow.png)

---

# Network Topology

| Network           | Purpose                           |
|-------------------|-----------------------------------|
| `192.168.61.0/24` | WAN / VMware NAT network          | 
| `192.168.0.0/24`  | pfSense ↔ VyOS transit network    | 
| `10.10.10.0/24`   | Internal client network           |
| `10.10.20.0/24`   | Server / management network       |
| `172.16.50.0/24`  | Isolated security/testing network |

### Routing Flow

```text
Client / Server
      |
      v
    VyOS
      |
      v
   pfSense
      |
      v
 VMware NAT
      |
      v
   Internet
