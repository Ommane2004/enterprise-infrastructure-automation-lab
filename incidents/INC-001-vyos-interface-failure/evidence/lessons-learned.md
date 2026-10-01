# Lessons Learned — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected Interface:** `eth1`  
**Affected Network:** `10.10.10.0/24` — Internal LAN  
**Incident Type:** Controlled infrastructure failure

---

## Purpose

This incident was intentionally created to validate infrastructure troubleshooting and recovery procedures.

The exercise demonstrated how an interface-level configuration change can propagate through the network stack and affect routing and connectivity.

---

## 1. Interface State Is an Important First-Level Indicator

The failure changed `eth1` from an operational state to:

```text
A/D
```

Detailed inspection also showed:

state DOWN

This provided an immediate indication that the affected interface was not operational.

Lesson

When investigating network connectivity problems, interface state should be checked early.

A useful diagnostic sequence is:

Interface state
    ↓
IP configuration
    ↓
Routing table
    ↓
Connectivity

**2. Routing Evidence Helped Establish Causality**

The 10.10.10.0/24 connected route disappeared while eth1 was disabled.

During the failure:

show ip route 10.10.10.0/24

returned:

% Network not in table

After remediation, the route returned as:

10.10.10.0/24
directly connected, eth1
Lesson

A connectivity failure should not be investigated only from the endpoint perspective.

Comparing the routing table before and after a configuration change can establish whether the network path itself has changed.

**3. Configuration Evidence Is Stronger Than Assumption**

The active configuration explicitly contained:

set interfaces ethernet eth1 disable

This provided direct configuration evidence for the failure condition.

Lesson

Troubleshooting should correlate:

Observed symptoms
Operational state
Routing state
Active configuration

rather than relying on assumptions about what may have happened.

**4. Separate Symptoms From Root Cause**

The initial symptom was loss of reachability to the Internal LAN.

However, the investigation did not stop at the failed ping.

The evidence chain was:

Connectivity failure
        ↓
Route investigation
        ↓
Connected route absent
        ↓
Interface investigation
        ↓
eth1 A/D
        ↓
Configuration investigation
        ↓
eth1 disable present
Lesson

A failed ping is a symptom, not necessarily a root cause.

Effective troubleshooting should identify the underlying condition that explains multiple observed symptoms.

**5. Validate Recovery at Multiple Layers**

After remediation, recovery was not declared based solely on the interface returning to an UP state.

Recovery was validated through:

Interface state
IP configuration
Connected routing entry
Final configuration state

The resulting state was:

eth1
  ↓
UP / LOWER_UP
  ↓
10.10.10.1/24
  ↓
10.10.10.0/24
  ↓
directly connected via eth1
Lesson

Infrastructure recovery should be validated across multiple layers.

A service or interface being "up" does not automatically prove that the complete network path is functioning.
**
6. Endpoint Availability Must Be Distinguished From Network Availability**

The final ping to 10.10.10.10 failed.

However, the Windows 10 endpoint was confirmed to be powered off.

Therefore, the ping result was not used as evidence that the VyOS remediation had failed.

Lesson

When a validation test fails, verify the state of the test target before concluding that the infrastructure under investigation remains faulty.

This prevents incorrect root-cause attribution.

**7. Controlled Failure Injection Improves Troubleshooting Skills**

The failure was deliberately introduced using:

set interfaces ethernet eth1 disable

This created a reproducible failure with a known cause and observable consequences.

Lesson

Controlled failure injection can be used to practice:

Incident detection
Evidence collection
Hypothesis testing
Root-cause analysis
Configuration remediation
Recovery validation
Incident documentation

The value comes from observing the complete cause-and-effect chain rather than simply restoring the configuration.

**8. Evidence Should Be Collected Before Remediation**

During this exercise, evidence was captured while the failure condition was still present.

Examples included:

eth1 → A/D
10.10.10.0/24 → absent
eth1 disable → present

This allowed the failure state to be documented before it was changed.

Lesson

Whenever operationally safe, capture relevant evidence before modifying the affected system.

Otherwise, remediation can remove the very evidence needed to establish the root cause.

**9. Public Portfolio Evidence Requires Sanitization**

The live interface output contained hardware-specific information such as the interface MAC address.

Such information is not required to demonstrate the troubleshooting process.

Lesson

Before publishing infrastructure evidence:

Remove credentials.
Remove API tokens.
Remove private keys.
Remove passwords.
Remove SNMP community strings.
Review IP addresses for sensitivity.
Review MAC addresses and hardware identifiers.
Remove unnecessary host-specific information.
Review screenshots before committing them.

The objective is to demonstrate technical capability without unnecessarily exposing infrastructure details.

**10. Recommended Future Improvements**

This controlled incident identified several areas that could be strengthened in a production-style environment.

Monitoring

Add or validate monitoring for:

Interface administrative state
Interface operational state
Interface availability
Route availability
Packet errors and discards

The existing Zabbix environment can be used to provide infrastructure visibility.

Automation

Develop an automated health check that validates:

Interface state
    ↓
Expected IP address
    ↓
Expected connected route
    ↓
Expected reachability
Configuration Control

Use configuration management and change control to reduce accidental administrative changes to critical interfaces.

Recovery Validation

Standardize a recovery checklist rather than relying on a single ping test.

Operational Troubleshooting Workflow

The incident supports the following reusable workflow:

1. Identify the affected service/network
                ↓
2. Check interface state
                ↓
3. Check IP configuration
                ↓
4. Check routing table
                ↓
5. Test connectivity
                ↓
6. Inspect active configuration
                ↓
7. Form a hypothesis
                ↓
8. Compare hypothesis against evidence
                ↓
9. Apply controlled remediation
                ↓
10. Validate interface recovery
                ↓
11. Validate routing recovery
                ↓
12. Validate endpoint/service availability
                ↓
13. Document the incident

**Key Takeaways**
Technical
Interface state directly affects connected routing.
Removing an interface from service can remove its connected route.
Routing-table inspection can explain connectivity symptoms.
Configuration inspection can identify the direct trigger.
Recovery should be validated at multiple layers.
Operational
Collect evidence before remediation when safe.
Separate symptoms from root cause.
Separate infrastructure availability from endpoint availability.
Avoid declaring recovery based on a single test.
Document the complete incident lifecycle.
Portfolio

This incident demonstrates practical capability in:

VyOS administration
Layer 3 troubleshooting
Routing-table analysis
Configuration analysis
Controlled failure injection
Root-cause analysis
Recovery validation
Incident documentation
Evidence-driven troubleshooting
Infrastructure operations

**Final Lesson**

The most important lesson from INC-001 is that infrastructure troubleshooting should follow evidence rather than assumption.

The failure was traced through:

Configuration
    ↓
Interface state
    ↓
Routing state
    ↓
Connectivity impact
    ↓
Root cause
    ↓
Remediation
    ↓
Recovery validation

This provides a repeatable troubleshooting methodology that can be applied to more complex network, infrastructure, monitoring, and security incidents.
