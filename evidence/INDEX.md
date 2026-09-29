# Evidence Index

This index maps the laboratory architecture and engineering
capabilities to the evidence maintained in this repository.

The objective is to make the relationship between:

```text
Architecture
    ↓
Implementation
    ↓
Validation
    ↓
Evidence

```

# 2. Validation Evidence

Validation evidence demonstrates that implemented infrastructure
behaves according to the documented design.

Current validation artifacts:

| Artifact | Validation |
|---|---|
| [`vyos-health-validation.txt`](validation/vyos-health-validation.txt) | Verifies critical VyOS interfaces are present and configured |
| [`vyos-connectivity-validation.txt`](validation/vyos-connectivity-validation.txt) | Verifies successful automated connectivity validation |
| [`vyos-audit-validation.txt`](validation/vyos-audit-validation.txt) | Verifies successful automated VyOS configuration audit |
| [`vyos-backup-validation.txt`](validation/vyos-backup-validation.txt) | Verifies successful automated configuration backup |

The detailed validation evidence is maintained in:

[`validation/`](validation/)

These artifacts represent actual laboratory validation results,
not placeholder examples.

# 3. Network Evidence

Network validation evidence demonstrates that the implemented
network architecture is configured and operating as expected.

The current evidence set covers interface state, routing,
pfSense routing, SNMP configuration, and end-to-end connectivity.

| Artifact | Evidence |
|---|---|
| [`15-vyos-interfaces.png`](validation/15-vyos-interfaces.png) | VyOS interface state and addressing |
| [`16-vyos-routing-table.png`](validation/16-vyos-routing-table.png) | VyOS routing table and upstream/default route |
| [`17-pfsense-routing.png`](validation/17-pfsense-routing.png) | pfSense routes toward internal networks |
| [`18-pfsense-snmp-configuration.png`](validation/18-pfsense-snmp-configuration.png) | pfSense SNMP configuration used for monitoring |
| [`19-end-to-end-connectivity.png`](validation/19-end-to-end-connectivity.png) | End-to-end network connectivity validation |

These artifacts support the network architecture documented in:

[`../architecture/network-architecture.md`](../architecture/network-architecture.md)

---

## Network Validation Relationship

```text
Client Network
10.10.10.0/24
       │
       ▼
     VyOS
10.10.10.1
       │
       ▼
 Transit Network
192.168.0.0/24
       │
       ▼
   pfSense
192.168.0.1
       │
       ▼
 VMware NAT
       │
       ▼
   Internet
