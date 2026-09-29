# Future Roadmap

## Overview

The current lab establishes a foundation across networking, system administration, monitoring, automation, virtualization, and cybersecurity.

Future development will focus on increasing reliability, automation depth, observability, security visibility, and operational realism.

The goal is to increase engineering depth rather than simply adding more technologies.

---

# Phase 1 — Strengthen the Existing Foundation

## Networking

- Expand routing scenarios.
- Add additional subnetting and routing exercises.
- Introduce more failure scenarios.
- Improve network troubleshooting runbooks.
- Expand SNMP monitoring coverage.

## Systems

- Add more Linux administration scenarios.
- Expand Windows Server administration.
- Improve Active Directory administration workflows.
- Add service-failure exercises.
- Expand DNS troubleshooting.

## Monitoring

- Improve Zabbix dashboards.
- Add more service checks.
- Improve alert classification.
- Expand network telemetry.
- Improve monitoring documentation.

---

# Phase 2 — Automation Maturity

## Ansible

Develop reusable automation for:

- Linux administration
- Windows administration
- Active Directory
- VyOS
- Configuration validation
- Backups
- Compliance checks

Future playbooks should increasingly use:

```text
Pre-Check
   ↓
Change
   ↓
Post-Check
   ↓
Validation
