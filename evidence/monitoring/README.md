# Monitoring Evidence

This directory contains sanitized evidence demonstrating operational
monitoring of the laboratory infrastructure.

The monitoring platform used by the laboratory is Zabbix.

---

## Monitoring Scope

The monitoring environment provides visibility into:

- Host availability
- Zabbix Agent monitoring
- SNMP monitoring
- Network interface traffic
- Interface errors
- Interface discards
- System uptime
- Monitoring problems
- Infrastructure status
- NOC operational visibility

---

## Planned Evidence

The following evidence artifacts are intended to demonstrate the
implemented monitoring capabilities.

| Evidence | Purpose |
|---|---|
| `09-zabbix-host-inventory.png` | Demonstrates monitored infrastructure and host inventory |
| `10-zabbix-pfsense-snmp.png` | Demonstrates pfSense SNMP monitoring |
| `11-zabbix-vyos-snmp.png` | Demonstrates VyOS SNMP monitoring |
| `12-zabbix-proxmox-monitoring.png` | Demonstrates Proxmox monitoring |
| `13-zabbix-problems.png` | Demonstrates Zabbix problem detection |
| `14-zabbix-noc-dashboard.png` | Demonstrates operational monitoring visibility |

These filenames represent the planned evidence set. An artifact
should only be added after the corresponding monitoring state has
actually been captured and reviewed.

---

## Evidence Requirements

Each screenshot should demonstrate a specific monitoring capability.

### Host Inventory

Should demonstrate:

- Monitored hosts
- Availability
- Relevant host status

### pfSense SNMP

Should demonstrate:

- pfSense monitoring
- SNMP-based visibility
- Relevant monitored metrics

Sensitive SNMP community strings must be redacted.

### VyOS SNMP

Should demonstrate:

- VyOS monitoring
- SNMP availability
- Relevant network metrics

### Proxmox

Should demonstrate:

- Hypervisor monitoring
- Infrastructure visibility
- Relevant health or availability information

API tokens and authentication material must never be visible.

### Problems

Should demonstrate:

- Detected monitoring problems
- Problem state
- Relevant operational context

Intentionally powered-off laboratory systems must not be
misrepresented as unexpected production incidents.

### NOC Dashboard

Should demonstrate:

- Infrastructure visibility
- Monitoring status
- Operational awareness
- Useful aggregation of monitored systems

---

## Screenshot Quality Standard

Each screenshot should:

1. Demonstrate a specific engineering claim.
2. Contain enough context to identify what is being monitored.
3. Avoid unnecessary UI clutter.
4. Avoid exposing secrets.
5. Avoid exposing sensitive authentication material.
6. Be readable at normal repository viewing size.

---

## Security Review

Before committing a screenshot, inspect it for:

- Passwords
- API tokens
- Private keys
- SNMP community strings
- Session tokens
- Sensitive configuration
- Customer information
- Unnecessary MAC addresses
- Unnecessary UUIDs
- Internal secrets

Redact sensitive information before publication.

---

## Monitoring → Evidence Relationship

The monitoring architecture describes how infrastructure visibility
is designed.

The evidence in this directory should demonstrate that the
monitoring system is actually observing the implemented
infrastructure.

The relationship is:

```text
Infrastructure
      ↓
Zabbix Agent / SNMP
      ↓
Zabbix Server
      ↓
Metrics / Problems
      ↓
Operational Visibility
      ↓
Investigation / Response
```
INC-004 — SNMP Monitoring Failure Evidence

These screenshots document the baseline monitoring state, controlled SNMP service interruptions, and recovery observations for VyOS and pfSense.

General baseline

[21-inc-004-snmp-baseline.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/21-inc-004-snmp-baseline.png) — Baseline SNMP monitoring overview.

VyOS
[22-inc-004-vyos-snmp-baseline.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/22-inc-004-vyos-snmp-baseline.png) — VyOS SNMP baseline.

[25-inc-004-vyos-snmp-failure.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/25-inc-004-vyos-snmp-failure.png) — VyOS SNMP monitoring failure.

[27-inc-004-vyos-snmp-service-before.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/27-inc-004-vyos-snmp-service-before.png) — VyOS SNMP service before failure injection.

[28-inc-004-vyos-snmp-service-failure.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/28-inc-004-vyos-snmp-service-failure.png) — VyOS SNMP service during failure.

[29-inc-004-vyos-snmp-recovery.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/29-inc-004-vyos-snmp-recovery.png) — VyOS SNMP service recovery.

pfSense

[23-inc-004-pfsense-snmp-baseline.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/23-inc-004-pfsense-snmp-baseline.png) — pfSense SNMP baseline.

[31-inc-004-pfsense-snmp-failure.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/31-inc-004-pfsense-snmp-failure.png) — pfSense SNMP monitoring failure.

[33-inc-004-pfsense-snmp-recovery.png ](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/monitoring/33-inc-004-pfsense-snmp-recovery.png)— pfSense SNMP availability recovery.

Related connectivity evidence

Connectivity checks performed before and during each failure are documented in [../validation/README.md.](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/4213c73132253b29ae66a307df57c81ada084b12/evidence/validation/README.md)

Security review

Before publishing screenshots, verify that they do not expose SNMP community strings, passwords, API tokens, or other sensitive configuration details. Replace or remove any unredacted image containing such information.
