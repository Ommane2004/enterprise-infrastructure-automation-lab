# Incident Management

This directory documents controlled infrastructure failures, troubleshooting investigations, root-cause analysis, remediation, recovery validation, and lessons learned from the enterprise infrastructure lab.

The objective is to demonstrate an evidence-driven incident response workflow rather than simply documenting successful configurations.

---

## Incident Workflow

Each incident follows this lifecycle:

```text
Detect
  ↓
Investigate
  ↓
Collect Evidence
  ↓
Identify Root Cause
  ↓
Remediate
  ↓
Validate Recovery
  ↓
Document
  ↓
Improve
```

The incident documentation is designed around:

Evidence before assumptions
Clear separation of symptoms and root cause
Configuration and operational-state analysis
Recovery validation
Lessons learned
Repeatable troubleshooting procedures

**Incident Index**

|Incident	       |      Title	Domain	               |       Status                     |
|----------------|-----------------------------------|----------------------------------|
|INC-001	       |   VyOS eth1 Interface Failur      |  Networking / Routing	Resolved  |

**1.**INC-001 — VyOS eth1 Interface Failure****

Type: Controlled infrastructure failure
Affected network: 10.10.10.0/24 — Internal LAN
Affected interface: eth1 — 10.10.10.1/24
Status: Resolved

**Scenario**

A controlled administrative shutdown of the VyOS eth1 interface was introduced to simulate an infrastructure interface failure.

The incident demonstrated the relationship between:

Interface State
      ↓
Connected Route
      ↓
Network Reachability
      ↓
Troubleshooting
      ↓
Remediation
      ↓
Recovery Validation

**Incident Documentation**

Incident Overview
Timeline
Investigation Evidence
Root Cause Analysis
Remediation & Recovery
Lessons Learned

**Evidence Philosophy**

Incident documentation should establish a clear relationship between:

Observation
   ↓
Evidence
   ↓
Interpretation
   ↓
Hypothesis
   ↓
Root Cause
   ↓
Remediation
   ↓
Validation

Evidence should be collected before remediation whenever operationally safe.

Public documentation must not expose:

Passwords
API tokens
Private keys
Credentials
SNMP community strings
Session tokens
Sensitive configuration
Unnecessary hardware identifiers
Real customer or organizational data

**Incident Documentation Standard**

Each incident should document, where applicable:

1. Overview

What happened and what component was affected.

2. Timeline

What happened and in what sequence.

3. Investigation

What was checked and what the evidence showed.

4. Root Cause

What directly caused the incident and how the evidence supports the conclusion.

5. Remediation

What corrective action was performed.

6. Recovery Validation

How recovery was independently verified.

7. Lessons Learned

What the incident demonstrated and what can be improved.

**Related Project Areas**

Architecture
Automation
Playbooks
Evidence
Monitoring
Validation
Network Architecture
Monitoring Architecture
Automation Architecture

**Operational Principle**

The purpose of incident documentation is not to demonstrate that failures never occur.

It is to demonstrate the ability to:

Detect → Investigate → Explain → Remediate → Validate → Improve

**### INC-002 — DNS Failure

**Status:** Resolved  
**Category:** DNS / Windows Server / Active Directory  
**System:** DC01  
**Failure:** Windows DNS Server service stopped  
**Validation:** IP connectivity remained healthy while DNS resolution failed; service restoration returned DNS resolution to normal.

- [Incident Overview](INC-002-dns-failure/README.md)
- [Timeline](INC-002-dns-failure/timeline.md)
- [Investigation](INC-002-dns-failure/evidence/investigation.md)
- [Root Cause](INC-002-dns-failure/evidence/root-cause.md)
- [Remediation](INC-002-dns-failure/evidence/remediation.md)
- [Lessons Learned](INC-002-dns-failure/evidence/lessons-learned.md)
- [Command Output](INC-002-dns-failure/evidence/command-output.md)**

INC-003 — SSH Service Failure

Status: Resolved

Summary: A controlled failure-injection exercise stopped the SSH daemon on RHEL01 (10.10.20.30). ICMP connectivity remained available, but SSH connections and Ansible remote management failed. Restarting sshd restored connectivity, confirmed by service-state checks and a successful Ansible ping.

Documentation: INC-003 — [SSH Service Failure](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/35bd1221581c66d6adcac5449a9f9912af814e8c/incidents/INC-003-service-failure)
