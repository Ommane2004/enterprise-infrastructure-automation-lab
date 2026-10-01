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

# 3. Monitoring Evidence

Monitoring evidence demonstrates operational visibility into the
implemented infrastructure through Zabbix.

The current evidence set covers host inventory, SNMP monitoring,
Proxmox monitoring, problem detection, and NOC visibility.

| Artifact | Evidence |
|---|---|
| [`09-zabbix-host-inventory.png`](monitoring/09-zabbix-host-inventory.png) | Zabbix monitored-host inventory and availability |
| [`10-zabbix-pfsense-snmp.png`](monitoring/10-zabbix-pfsense-snmp.png) | pfSense SNMP monitoring |
| [`11-zabbix-vyos-snmp.png`](monitoring/11-zabbix-vyos-snmp.png) | VyOS SNMP monitoring |
| [`12-zabbix-proxmox-monitoring.png`](monitoring/12-zabbix-proxmox-monitoring.png) | Proxmox infrastructure monitoring |
| [`13-zabbix-problems.png`](monitoring/13-zabbix-problems.png) | Zabbix problem detection and operational state |
| [`14-zabbix-noc-dashboard.png`](monitoring/14-zabbix-noc-dashboard.png) | NOC-oriented infrastructure visibility |

Detailed monitoring evidence is maintained in:

[`monitoring/`](monitoring/)

---

## Monitoring Relationship

```text
Infrastructure
      │
      ├── Linux
      ├── Windows / AD
      ├── VyOS
      ├── pfSense
      └── Proxmox
              │
              ▼
        Zabbix Monitoring
              │
       ┌──────┴──────┐
       ▼             ▼
     Agent          SNMP
       │             │
       └──────┬──────┘
              ▼
       Metrics / Problems
              │
              ▼
       NOC Visibility
```
# 4. Network Evidence

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

```
5.## Incident Evidence

### INC-001 — VyOS `eth1` Interface Failure

Controlled infrastructure failure demonstrating the relationship between interface state, connected routing, network reachability, configuration analysis, remediation, and recovery validation.

**Incident:** [`INC-001-vyos-interface-failure`](../incidents/INC-001-vyos-interface-failure/)

**Evidence:**

- [Incident Overview](../incidents/INC-001-vyos-interface-failure/README.md)
- [Incident Timeline](../incidents/INC-001-vyos-interface-failure/timeline.md)
- [Investigation Evidence](../incidents/INC-001-vyos-interface-failure/evidence/investigation.md)
- [Root Cause Analysis](../incidents/INC-001-vyos-interface-failure/evidence/root-cause.md)
- [Remediation & Recovery](../incidents/INC-001-vyos-interface-failure/evidence/remediation.md)
- [Lessons Learned](../incidents/INC-001-vyos-interface-failure/evidence/lessons-learned.md)
- [Command Output Evidence](../incidents/INC-001-vyos-interface-failure/evidence/command-output.md)

### Evidence Chain

```text
INC-001
   ↓
Controlled Interface Failure
   ↓
Interface State Evidence
   ↓
Routing Evidence
   ↓
Connectivity Evidence
   ↓
Configuration Evidence
   ↓
Root Cause
   ↓
Remediation
   ↓
Recovery Validation
   ↓
Lessons Learned
```
### INC-002 — DNS Failure

Controlled DNS service outage on DC01.

**Evidence demonstrates:**

- Healthy baseline DNS state
- Controlled DNS service failure
- Successful IP connectivity during DNS outage
- DNS resolution timeouts
- Server-side service-state confirmation
- DNS service remediation
- Functional DNS recovery
- Final DNS zone validation

[View INC-002 Evidence](../incidents/INC-002-dns-failure/README.md)

INC-003 — SSH Service Failure

Status: Resolved

Scenario: The SSH daemon on RHEL01 (10.10.20.30) was deliberately stopped to simulate a remote-management outage.

Key findings:

ICMP connectivity remained available.
SSH connections to TCP/22 were refused.
Ansible reported the host as UNREACHABLE.
Restarting sshd restored SSH-based Ansible connectivity.

Evidence:

[Incident overview](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure)
[Timeline](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure/timeline.md)
[Investigation](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure/evidence/investigation.md)
[Root cause analysis](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure/evidence/root-cause.md)
[Remediation and recovery](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure/evidence/remediation.md)
[Lessons learned](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure/evidence/lessons-learned.md)
[Command output evidence](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/8293cd8aa2fce3a79294635fb3c7b5a4e191ac02/incidents/INC-003-service-failure/evidence/command-output.md)

INC-004 — SNMP Monitoring Failure

Controlled SNMP monitoring failures were tested independently on VyOS and pfSense. During the VyOS test, the SNMP daemon was stopped while ICMP connectivity remained available. During the pfSense test, Zabbix reported SNMP agent availability as not available (0) while pfSense remained reachable over IP. SNMP was restored on both devices, and pfSense availability returned to available (1).

Evidence

[pfSense SNMP failure](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/monitoring/31-inc-004-pfsense-snmp-failure.png)
[pfSense SNMP recovery](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/monitoring/33-inc-004-pfsense-snmp-recovery.png)
[pfSense connectivity before failure](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/validation/30-inc-004-pfsense-connectivity-before.png)
[pfSense connectivity during failure](https://github.com/Ommane2004/enterprise-infrastructure-automation-[lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/validation/32-inc-004-pfsense-connectivity-during-failure.png)
[VyOS SNMP service failure](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/monitoring/28-inc-004-vyos-snmp-service-failure.png)
[VyOS SNMP recovery](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/monitoring/29-inc-004-vyos-snmp-recovery.png)
[VyOS connectivity during failure](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/72e38b1c8bf0db20bf31aef1930f204c73cd39a4/evidence/validation/26-inc-004-vyos-connectivity-during-failure.png)

INC-004 — SNMP Monitoring Failure

Objective: Demonstrate how an SNMP monitoring failure affects observability while IP connectivity remains available, and validate recovery on VyOS and pfSense.

Incident workflow: Baseline → Failure injection → Connectivity validation → Recovery → Monitoring verification.

Monitoring evidence

|Evidence	                         |                            Description                           |
|------------------------------------|------------------------------------------------------------------|
|SNMP baseline	                     |          Both devices visible in Zabbix before testing           |
|VyOS SNMP baseline                  |            	VyOS SNMP metrics before failure                    |
|pfSense SNMP baseline	             |             pfSense SNMP metrics before failure                  |
|VyOS SNMP failure	                 |         Zabbix monitoring symptoms during the VyOS test          |
|VyOS service before failure	     |                SNMP daemon running before failure injection      |
|VyOS service failure	             |                       SNMP daemon stopped                        |
|VyOS SNMP recovery	                 |             Monitoring after VyOS SNMP recovery                  | 
|pfSense SNMP failure	             |            Zabbix reports SNMP agent unavailable                 |
|pfSense SNMP recovery	             |             Zabbix reports SNMP agent available again            |

Connectivity validation evidence

|Evidence	                                  |                 Description                                        |
|---------------------------------------------|--------------------------------------------------------------------|
|VyOS connectivity before failure	          |            Baseline ping to VyOS                                   |
|VyOS connectivity during failure	          |    Ping succeeds while VyOS SNMP is stopped                        |
|pfSense connectivity before failure	      |             Baseline ping to pfSense                               |
|pfSense connectivity during failure	      |     Ping succeeds while pfSense SNMP monitoring is unavailable     |

Key finding: SNMP monitoring availability and IP reachability are separate. During the VyOS test, the SNMP daemon was inactive while ping continued to succeed. During the pfSense test, Zabbix reported SNMP agent availability = 0; after SNMP was restored, it returned to 1.

Recovery: SNMP was restored on both devices. Verify the latest Zabbix data and recovery evidence when reviewing this incident.
