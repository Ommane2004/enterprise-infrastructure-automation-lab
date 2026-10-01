# Evidence & Validation

This directory contains selected evidence demonstrating that the
infrastructure, automation, networking, monitoring and operational
workflows documented in this repository have been implemented and
validated.

The evidence is organized around engineering outcomes rather than
screenshot quantity.

---

## Evidence Philosophy

The laboratory follows this workflow:

```text
Design
  ↓
Build
  ↓
Configure
  ↓
Validate
  ↓
Monitor
  ↓
Automate
  ↓
Break
  ↓
Troubleshoot
  ↓
Recover
  ↓
Document
  ↓
Improve
```
INC-004 — SNMP Failure Connectivity Validation

These screenshots record IP reachability before and during the controlled SNMP monitoring failures.

VyOS

[24-inc-004-vyos-connectivity-before.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/178dd65f94da2bb0d42cf9e2ff69b0dfbd8e6a5d/evidence/validation/24-inc-004-vyos-snmp-failure.png) — Baseline connectivity to VyOS (192.168.0.2).

[26-inc-004-vyos-connectivity-during-failure.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/178dd65f94da2bb0d42cf9e2ff69b0dfbd8e6a5d/evidence/validation/26-inc-004-vyos-connectivity-during-failure.png) — Connectivity to VyOS while its SNMP daemon was stopped.

pfSense

[30-inc-004-pfsense-connectivity-before.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/178dd65f94da2bb0d42cf9e2ff69b0dfbd8e6a5d/evidence/validation/30-inc-004-pfsense-connectivity-before.png) — Baseline connectivity to pfSense (192.168.0.1).

[32-inc-004-pfsense-connectivity-during-failure.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/178dd65f94da2bb0d42cf9e2ff69b0dfbd8e6a5d/evidence/validation/32-inc-004-pfsense-connectivity-during-failure.png) — Connectivity to pfSense while SNMP was disabled.

**Findings**

The recorded ping tests returned four replies with 0% packet loss for each device, including during its respective SNMP failure.

This evidence supports the conclusion that IP reachability remained available while SNMP monitoring was interrupted. It does not, by itself, establish that all other device services were operational.

Related evidence

[Monitoring evidence README](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/200358f9c9a250d019591b62aa9fb28c35597e31/evidence/monitoring/README.md)

[INC-004 investigation](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/200358f9c9a250d019591b62aa9fb28c35597e31/incidents/INC-004-snmp-monitoring-failure/evidence/investigation.md)

[Evidence index](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/200358f9c9a250d019591b62aa9fb28c35597e31/evidence/INDEX.md)
