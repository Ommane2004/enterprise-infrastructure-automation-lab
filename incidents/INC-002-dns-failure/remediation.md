# INC-002 — DNS Remediation

## 1. Remediation Objective

Restore the Windows DNS Server service on DC01 and verify that internal DNS resolution returns to normal.

The remediation was intentionally limited to the failed service.

No DNS zones, records, client network configuration, or routing configuration were modified.

---

## 2. Identified Failure

During the incident, the DNS Server service on DC01 was:

```text
Status: Stopped
StartType: Automatic
```

The Windows 10 client could still reach DC01 over IP, but DNS queries timed out.

This established that the primary remediation target was the DNS Server service.

**3. Remediation Action**

The DNS Server service was started on DC01 using PowerShell:

Start-Service -Name DNS

The command completed without an error.

**4. Service-Level Verification**

After remediation, the DNS Server service was checked:

Get-Service -Name DNS | Select-Object Name, DisplayName, Status, StartType

Observed state:

Name         : DNS
DisplayName  : DNS Server
Status       : Running
StartType    : Automatic

This confirmed that the service had successfully returned to an operational state.

**5. Functional DNS Validation**

Service status alone was not considered sufficient evidence of recovery.

The Windows 10 client was used to perform functional DNS validation.

Domain Controller resolution
nslookup dc01.corp.lab

Result:

dc01.corp.lab → 10.10.20.10
Domain resolution
nslookup corp.lab

Result:

corp.lab → 10.10.20.10
Explicit DNS server validation

The DNS server was explicitly specified:

nslookup dc01.corp.lab 10.10.20.10

The query successfully returned:

dc01.corp.lab → 10.10.20.10
**6. Configuration Validation**

After service recovery, the DNS zones were checked again.

Present zones:

_msdcs.corp.lab
0.in-addr.arpa
127.in-addr.arpa
255.in-addr.arpa
corp.lab

The AD-integrated zones remained present:

_msdcs.corp.lab
corp.lab

No zone deletion, recreation, or configuration reset was required.

**7. Remediation Effectiveness**

The remediation was considered successful because all three validation layers passed:

Layer 1 — Service
DNS Server = Running
Layer 2 — DNS functionality
dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10
Layer 3 — DNS configuration
Required DNS zones = Present
AD-integrated zones = Intact
**8. Recovery Chain**
DNS Server service stopped
        ↓
DNS queries timed out
        ↓
Start DNS Server service
        ↓
DNS service = Running
        ↓
Query dc01.corp.lab
        ↓
10.10.20.10 returned
        ↓
Query corp.lab
        ↓
10.10.20.10 returned
        ↓
DNS functionality restored
**9. Why the Remediation Was Minimal**

The investigation established that:

DC01 was reachable.
The client DNS configuration was valid.
DNS zones were present.
The DNS service was the failed component.

Therefore, changing unrelated infrastructure would have introduced unnecessary risk.

The remediation followed the principle:

Fix the identified failure, then validate the actual service functionality.

No routing, firewall, DNS zone, or client configuration changes were required.

**10. Final State**

The DNS Server service was restored to its normal configuration:

Service:     DNS Server
Status:      Running
Start Type:  Automatic

Internal DNS resolution was successfully restored.

Remediation Status: COMPLETE
