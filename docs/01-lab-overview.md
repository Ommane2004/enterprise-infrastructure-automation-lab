# Lab Overview

## Purpose

This project is a personal enterprise-style infrastructure lab designed to develop practical skills across networking, system administration, infrastructure automation, monitoring, virtualization, and cybersecurity.

The environment is built using VMware virtualization and contains separate network segments for client systems, servers, infrastructure transit, and security testing.

---

## Primary Goals

- Practice enterprise networking concepts.
- Build and troubleshoot routed network segments.
- Manage Linux and Windows systems.
- Configure and administer Active Directory.
- Automate infrastructure operations using Ansible.
- Monitor infrastructure using Zabbix.
- Monitor network devices through SNMP.
- Integrate virtualization infrastructure with monitoring.
- Perform configuration backups and automated health validation.
- Practice structured troubleshooting and incident handling.
- Build a realistic environment for future SOC and security engineering work.

---

## Lab Architecture

The environment contains the following major components:

| Component       | Role                                         |
|-----------------|----------------------------------------------|
| pfSense         | Firewall, gateway, NAT and upstream routing  |
| VyOS            | Core routing                                 |
| Zabbix01        | Monitoring and Ansible automation controller |
| DC01            | Windows Server, Active Directory and DNS     |
| RHEL01          | Linux server                                 |
| Windows 10      | Client workstation                           |
| Kali Linux      | Security testing workstation                 |
| Metasploitable2 | Isolated security-testing target             |
| Proxmox         | Virtualization infrastructure                |

---

## Network Segmentation

The lab uses separate virtual networks for different operational purposes.

| Network           | Purpose                           |
|-------------------|-----------------------------------|
| `192.168.61.0/24` | VMware NAT / WAN                  |
| `192.168.0.0/24`  | pfSense ↔ VyOS transit            |
| `10.10.10.0/24`   | Internal client network           |
| `10.10.20.0/24`   | Server and management network     |
| `172.16.50.0/24`  | Isolated security-testing network |

---

## Routing Design

The primary routing path is:

```text
Internal / Server Networks
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
