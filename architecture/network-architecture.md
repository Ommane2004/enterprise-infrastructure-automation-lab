# Network Architecture

## Purpose

This document describes the implemented network architecture of
the Enterprise Infrastructure & Automation Lab.

The network is built primarily using VMware Workstation Pro and
uses pfSense and VyOS to provide firewalling, routing, NAT,
segmentation, DHCP, and monitoring integration.

The design separates:

- WAN connectivity
- Router transit
- Internal client systems
- Server and management systems
- Security-testing systems

---

## High-Level Network Path

```text
                         INTERNET
                            |
                     VMware NAT
                  192.168.61.0/24
                            |
                         pfSense
                            |
                    192.168.0.0/24
                     Transit Network
                            |
                          VyOS
                     /             \
                    /               \
                   /                 \
        10.10.10.0/24           10.10.20.0/24
        Internal Client         Server / Management
             Network                 Network

---
##A separate isolated security-testing network exists:
                    SECURITY TESTING
                     172.16.50.0/24
                            |
                    +-------+-------+
                    |               |
                   Kali       Metasploitable2

```
****---

## Related Incident

### INC-001 — VyOS `eth1` Interface Failure

The Internal LAN interface `eth1` (`10.10.10.1/24`) was used in a controlled failure exercise to validate the relationship between interface state, connected routing, and network reachability.

During the failure:

```text
eth1
  ↓
Administratively disabled
  ↓
10.10.10.0/24 connected route removed
  ↓
Internal LAN reachability impacted
```
eth1
  ↓
UP / LOWER_UP
  ↓
10.10.10.0/24 directly connected via eth1
  ↓
VyOS routing state restored

Incident documentation: INC-001 — VyOS eth1 Interface Failure

Evidence index: [(enterprise-infrastructure-automation-lab/main/evidence/INDEX.md)](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/42530ac1757da24250ab7a77616a1d9e5ab5a013/evidence/INDEX.md)

## Incident Validation

The network architecture has been validated through controlled failure and recovery exercises.

### INC-001 — VyOS Interface Failure

A controlled shutdown of the VyOS `eth1` internal interface was used to validate:

- Interface-state troubleshooting
- Routing-table changes
- Connectivity failure detection
- Service restoration
- Post-remediation validation

See:

[INC-001 — VyOS Interface Failure](../incidents/INC-001-vyos-interface-failure/README.md)

### INC-002 — DNS Failure

A controlled DNS Server service outage on `DC01` was used to validate:

- DNS service availability troubleshooting
- Separation of IP connectivity from DNS functionality
- DNS resolution testing
- Windows Server service-state analysis
- Service restoration
- Functional DNS recovery validation

The Windows 10 client remained able to reach `DC01` over IP while DNS queries failed, demonstrating that host reachability and DNS service availability must be tested separately.

See:

[INC-002 — DNS Failure](../incidents/INC-002-dns-failure/README.md)
