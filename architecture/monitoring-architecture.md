# Monitoring Architecture

## Purpose

This document describes the monitoring and observability
architecture implemented within the Enterprise Infrastructure &
Automation Lab.

The monitoring environment is designed to provide operational
visibility into:

- Host availability
- System health
- Network infrastructure
- Network interfaces
- Interface traffic
- Interface errors
- Interface discards
- Service availability
- Infrastructure problems
- Virtualization infrastructure

The monitoring platform used by the lab is Zabbix.

---

# Monitoring Platform

The primary monitoring server is:

```text
Zabbix01
Ubuntu 24.04 LTS
10.10.20.20/24
```
## Related Incident

### INC-001 — VyOS `eth1` Interface Failure

The monitoring architecture is designed to provide visibility into infrastructure availability and network health.

INC-001 provides a controlled example of an interface-level failure affecting the Internal LAN:

```text
VyOS eth1
    ↓
Administrative failure
    ↓
Interface state changes
    ↓
Connected route disappears
    ↓
Internal LAN reachability is impacted

The incident demonstrates why infrastructure monitoring should correlate interface availability with network and routing state rather than relying on a single connectivity check.

After remediation:

eth1
    ↓
UP / LOWER_UP
    ↓
10.10.10.0/24 connected route restored
    ↓
VyOS network state recovered

Incident documentation: INC-001 — VyOS eth1 Interface Failure

Monitoring evidence: Monitoring Evidence

Evidence index: [Evidence Index](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/42530ac1757da24250ab7a77616a1d9e5ab5a013/evidence/INDEX.md)
