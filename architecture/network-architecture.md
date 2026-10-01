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

Evidence index: enterprise-infrastructure-automation-lab/main/evidence/INDEX.md
