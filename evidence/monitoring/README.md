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
