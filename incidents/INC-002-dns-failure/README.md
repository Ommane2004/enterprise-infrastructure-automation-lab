# INC-002 — DNS Failure

**Incident ID:** INC-002  
**Category:** DNS / Windows Server / Active Directory  
**Severity:** Medium  
**Status:** Resolved  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected Service:** Internal DNS  
**Primary System:** DC01  
**Detection Method:** Controlled lab failure simulation

---

## 1. Incident Summary

A controlled DNS service failure was introduced on the Active Directory Domain Controller (`DC01`) to validate the ability to identify, troubleshoot, remediate, and verify recovery from an internal DNS outage.

The Windows 10 client remained able to reach DC01 over IP while DNS queries failed.

The DNS Server service on DC01 was intentionally stopped:

```powershell
Stop-Service -Name DNS
```
After the failure was injected:

IP connectivity to DC01 remained available.
DNS queries to 10.10.20.10 timed out.
dc01.corp.lab could not be resolved.
corp.lab could not be resolved.
The DNS Server service was confirmed to be stopped.

The DNS service was then restored:

Start-Service -Name DNS

Recovery was validated by confirming:

DNS Server service returned to Running.
dc01.corp.lab resolved to 10.10.20.10.
corp.lab resolved to 10.10.20.10.
DNS queries sent explicitly to 10.10.20.10 succeeded.
AD-integrated DNS zones remained present.
2. Impact

During the controlled failure window:

Internal DNS resolution was unavailable from the Windows 10 client.
Queries against the configured DNS server 10.10.20.10 timed out.
IP connectivity to DC01 remained operational.

The failure was intentionally introduced as part of the lab's incident-response and troubleshooting validation process.

No production infrastructure or external customer systems were involved.

3. Environment
Affected Client
Hostname: DESKTOP-JMEKHSC
IP Address: 10.10.10.10/24
DNS Server: 10.10.20.10
Domain: corp.lab
DNS Server
Hostname: DC01
IP Address: 10.10.20.10/24
Role: Active Directory Domain Services + DNS Server
DNS Service: Windows DNS Server
Startup Type: Automatic
DNS Configuration

AD-integrated zones:

corp.lab
_msdcs.corp.lab

Additional reverse lookup zones:

0.in-addr.arpa
127.in-addr.arpa
255.in-addr.arpa

Configured forwarder:

8.8.8.8
4. Incident Workflow

The incident followed the lab's standard operational workflow:

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
Service Recovery
   ↓
Functional Validation
   ↓
Documentation
   ↓
Lessons Learned
5. Root Cause

The immediate root cause was the intentional stoppage of the Windows DNS Server service on DC01.

The service state during the failure was:

DNS Server
Status: Stopped
StartType: Automatic

Because the DNS server was unavailable, DNS queries sent to 10.10.20.10 timed out even though the underlying IP connectivity to DC01 remained healthy.

See:

Root Cause
Investigation
6. Detection and Investigation

The failure was isolated by comparing network-layer connectivity with application/service-layer DNS resolution.

IP connectivity

The Windows 10 client successfully pinged:

10.10.20.10

with:

4 packets transmitted
4 packets received
0% packet loss

This established that the client could still reach DC01.

DNS resolution

DNS queries then produced timeouts:

nslookup dc01.corp.lab

and:

nslookup corp.lab

Both failed while the DNS service was stopped.

An explicit query against:

10.10.20.10

also timed out.

This separated the DNS failure from a general network-connectivity failure.

7. Remediation

The DNS Server service was restarted on DC01:

Start-Service -Name DNS

The service was subsequently verified as:

DNS Server
Status: Running
StartType: Automatic

See:

Remediation
Timeline

**8. Recovery Validation**

Recovery was validated from the Windows 10 client.

DC01 resolution
dc01.corp.lab → 10.10.20.10
Domain resolution
corp.lab → 10.10.20.10
Explicit DNS server query

Queries sent directly to:

10.10.20.10

successfully returned DNS records.

The DNS zones were also rechecked after recovery and remained present.

**9. Evidence**
Investigation
Investigation Evidence
Root Cause Evidence
Remediation Evidence
Lessons Learned
Command Output

**Timeline**
[Incident Timeline](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/23672345fefad168629a5c7bf36d670cc5babc51/incidents/INC-001-vyos-interface-failure/timeline.md)

**10. Key Troubleshooting Lesson**

The incident demonstrates the importance of separating:

Network Reachability
        ≠
Service Availability
        ≠
Application Functionality

A successful ping to a server does not prove that DNS is operational.

The investigation therefore tested both:

IP connectivity to the DNS server.
DNS functionality through actual name-resolution queries.

This allowed the failure domain to be narrowed to the DNS service rather than incorrectly treating it as a network outage.

**11. Security and Safety**

This was a controlled failure simulation inside an isolated lab environment.

No production systems, customer data, credentials, API tokens, private keys, or other sensitive information are included in this incident documentation.

**12. Related Documentation**
[Enterprise Architecture](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/23672345fefad168629a5c7bf36d670cc5babc51/architecture/enterprise-architecture.md)
[Network Architecture](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/23672345fefad168629a5c7bf36d670cc5babc51/architecture/network-architecture.md)
[Monitoring Architecture](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/23672345fefad168629a5c7bf36d670cc5babc51/architecture/monitoring-architecture.md)
[Evidence Index](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/23672345fefad168629a5c7bf36d670cc5babc51/evidence/INDEX.md)
[Incidents](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/23672345fefad168629a5c7bf36d670cc5babc51/incidents)

**13. Status
RESOLVED**
