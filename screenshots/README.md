### Ansible

| File                                      | Description                                                                                                        |
|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| `01-linux-server-audit.png`               | Ansible execution of a Linux server audit, demonstrating automated information collection and validation across Linux hosts.                                                                                                                                                     |
| `02-windows-server-audit.png`             | Ansible automation against the Windows Server environment using WinRM, demonstrating remote Windows administration.                                                                                                                                                                  |
| `03-active-directory-user-automation.png` | Automated Active Directory test-user workflow demonstrating Windows domain administration through Ansible.         |
| `04-vyos-network-audit.png`               | Automated VyOS network audit showing interface addressing, interface state, routing information, and successful Ansible execution.                                                                                                                                               |
| `05-vyos-health-check.png`                | Automated health validation of critical VyOS interfaces with explicit PASS/FAIL verification.                      |
| `06-vyos-configuration-backup.png`        | Automated collection and timestamped storage of a sanitized VyOS configuration backup.                             |
| `07-ansible-inventory.png`                | Ansible inventory hierarchy separating Linux, Windows, and network infrastructure targets.                         |
| `08-ansible-environment.png`              | Ansible controller environment showing Ansible Core, Python, executable path, and active configuration file.       |

### Zabbix

| File | Description |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| `09-zabbix-host-inventory.png`     | Zabbix infrastructure inventory showing the monitored servers, clients, network devices, and virtualization platform. |
| `10-zabbix-pfsense-monitoring.png` | Zabbix monitoring view for pfSense showing monitored network and availability metrics collected through SNMP.         |
| `11-zabbix-vyos-monitoring.png`    | Zabbix monitoring view for VyOS showing infrastructure availability and network-related telemetry.                    |
| `12-zabbix-proxmox-monitoring.png` | Zabbix monitoring of the Proxmox virtualization platform using its configured monitoring integration.                 |
| `13-zabbix-problems.png`           | Zabbix Problems view demonstrating centralized detection and presentation of infrastructure events.                   |
| `14-zabbix-dashboard.png`          | Infrastructure monitoring dashboard providing a centralized NOC-style operational view of the lab.                    |

---

### `01-physical-virtual-architecture.png`

> **Physical / Virtual Architecture** — Overview of the VMware-hosted lab and the virtual machines providing firewalling, routing, monitoring, servers, clients, virtualization, and security-testing capabilities.

### `02-network-architecture.png`

> **Network Architecture** — Logical network topology showing the WAN, transit, internal client, server, and isolated security networks and the routing relationships between them.

### `03-automation-architecture.png`

> **Automation Architecture** — Ansible-based management architecture showing Zabbix01 as the automation controller and the Linux, Windows, Active Directory, and VyOS systems managed from the centralized automation environment.

### `04-noc-soc-operational-workflow.png`

> **NOC/SOC Operational Workflow** — Operational lifecycle connecting monitoring, detection, investigation, validation, remediation, verification, and documentation.

---

```markdown
# Enterprise Infrastructure Automation, Monitoring & Security Lab

> A hands-on enterprise-style virtual lab combining networking, system administration, automation, monitoring, virtualization, and cybersecurity.

**Core technologies:** VMware • pfSense • VyOS • Zabbix • Ansible • Linux • Windows Server • Active Directory • Proxmox • SNMP • Python • Kali Linux

---

## What This Project Demonstrates

**Networking**
Routing • NAT • DNS • DHCP • Segmentation • SNMP • VMware Networking

**Automation**
Ansible • Linux • Windows • Active Directory • VyOS • Configuration Backup • Health Validation

**Monitoring**
Zabbix • SNMP • Agent Monitoring • Network Telemetry • Problems • Dashboards

**Systems**
Ubuntu • RHEL • Windows Server • Active Directory • DNS • SSH • WinRM

**Security**
Kali Linux • Metasploitable2 • Isolated Security Network • Attack Simulation • Detection Engineering

[Architecture](architecture/diagrams/02-network-architecture.png) •
[Automation](architecture/diagrams/03-automation-architecture.png) •
[NOC/SOC Workflow](architecture/diagrams/04-noc-soc-operational-workflow.png)

