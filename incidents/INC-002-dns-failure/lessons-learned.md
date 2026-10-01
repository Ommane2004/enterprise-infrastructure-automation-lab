# INC-002 — Lessons Learned

## 1. Incident Overview

This controlled incident simulated an internal DNS service outage on the Active Directory Domain Controller (`DC01`).

The failure was created by stopping the Windows DNS Server service.

The incident demonstrated the complete operational cycle:

```text
Baseline
    ↓
Failure Injection
    ↓
Detection
    ↓
Investigation
    ↓
Root Cause Identification
    ↓
Remediation
    ↓
Recovery Validation
    ↓
Documentation
```
2. Key Lesson — Separate Network Connectivity From Service Availability

The most important troubleshooting lesson was that successful IP connectivity does not prove that a specific network service is operational.

During the incident:

Ping DC01
    ↓
Successful

while:

DNS query
    ↓
Timeout

Therefore:

Host reachable
        ≠
DNS service available

This distinction is critical when troubleshooting infrastructure incidents.

3. Use Layered Troubleshooting

The investigation followed a layered approach rather than immediately changing configuration.

Layer 1 — Client Configuration

Verified:

Client IP address
Default gateway
DNS server
DNS suffix
Layer 2 — IP Connectivity

Tested connectivity to:

10.10.20.10
Layer 3 — DNS Functionality

Tested:

nslookup dc01.corp.lab
nslookup corp.lab
Layer 4 — Server Service State

Checked:

DNS Server
Layer 5 — DNS Configuration

Verified:

AD-integrated zones
Reverse lookup zones
DNS forwarder

This approach reduced the possibility of making unnecessary configuration changes.

4. Service-Specific Validation Is Essential

A generic connectivity test such as ping is insufficient when troubleshooting application or infrastructure services.

The investigation used:

ping

to validate IP connectivity and:

nslookup

to validate DNS functionality.

This provided two different forms of evidence.

Ping
  ↓
Network reachability

nslookup
  ↓
DNS functionality

This distinction should be applied broadly to infrastructure troubleshooting.

5. Validate Recovery at Multiple Levels

The incident was not considered resolved merely because the DNS service returned to Running.

Recovery was validated at three levels.

Service Level
DNS Server = Running
Functional Level
dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10
Configuration Level

Required DNS zones remained present.

This provides stronger evidence than relying on a single service-status check.

6. Avoid Unnecessary Changes

The investigation established that:

The client configuration was valid.
DC01 was reachable.
DNS zones were present.
The DNS Server service was the failed component.

Therefore, remediation was limited to restoring the failed service.

No changes were required to:

Routing
Client IP configuration
DNS zone configuration
DNS records
Firewall configuration
Network interfaces

This follows a useful operational principle:

Correct the identified failure rather than changing unrelated components.

7. Controlled Failure Injection Improves Troubleshooting Skill

The incident was intentionally created in a controlled lab environment.

This provided an opportunity to observe:

Healthy State
     ↓
Known Failure
     ↓
Observable Symptoms
     ↓
Diagnostic Evidence
     ↓
Remediation
     ↓
Known Recovery State

Controlled failure injection is useful because the actual root cause is known while the diagnostic process still requires evidence-based reasoning.

8. Evidence Should Establish Causality

The strongest evidence in this incident was the relationship between service state and DNS behavior.

Before failure
DNS service = Running
DNS resolution = Successful
During failure
DNS service = Stopped
DNS resolution = Failed
IP connectivity = Successful
After remediation
DNS service = Running
DNS resolution = Successful

This creates a clear:

Before → Failure → Recovery

evidence chain.

9. Troubleshooting Should Follow Evidence

The investigation avoided immediately assuming that the DNS failure was caused by:

Network routing
Firewall rules
DNS zone corruption
Client misconfiguration
Domain Controller failure

Instead, each layer was tested.

The resulting troubleshooting pattern was:

Observe
  ↓
Form hypothesis
  ↓
Test hypothesis
  ↓
Collect evidence
  ↓
Isolate failure
  ↓
Remediate
  ↓
Validate

This approach is applicable to NOC, Network Engineering, System Administration, Infrastructure, and SOC environments.

10. Operational Improvement Opportunities

Although this incident was intentionally generated, the exercise identifies several areas that could be developed further in the lab.

Monitoring

Configure monitoring to detect:

DNS service unavailable

before users report DNS resolution problems.

Alerting

Generate an alert when the DNS service changes from:

Running → Stopped
Service Recovery

Evaluate whether appropriate infrastructure services should have:

Service recovery actions
Automated restart policies
Monitoring-based remediation
Dependency Awareness

Document the dependency relationship between:

Active Directory
      ↓
DNS
      ↓
Domain services
      ↓
Windows clients
Runbook Development

Create a reusable DNS troubleshooting runbook containing:

Verify client configuration.
Test IP connectivity.
Test DNS resolution.
Check DNS service state.
Check DNS zones.
Check DNS forwarders.
Restore service if appropriate.
Validate functional recovery.
Document evidence.
11. Broader Infrastructure Lesson

Infrastructure troubleshooting should distinguish between:

Infrastructure Reachability
        ↓
Service Availability
        ↓
Application Functionality

A system can be reachable while one of its critical services is unavailable.

Therefore, effective infrastructure engineers validate the specific service involved in an incident rather than relying only on generic connectivity tests.

12. Skills Demonstrated

This incident demonstrates practical experience with:

Windows Server administration
Active Directory infrastructure
DNS administration
PowerShell
DNS troubleshooting
Layered troubleshooting
Service-state analysis
Incident response
Root-cause analysis
Controlled failure injection
Recovery validation
Evidence-based troubleshooting
Technical documentation
13. Final Lesson

The primary lesson from INC-002 is:

Do not confuse network reachability with service availability.

A successful ping established that DC01 was reachable.

A successful nslookup established that DNS was functional.

Using both tests allowed the failure domain to be isolated accurately and the remediation to be validated objectively.
