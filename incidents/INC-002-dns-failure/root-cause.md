# INC-002 — Root Cause Analysis

## 1. Root Cause Summary

The immediate root cause of the DNS outage was the **Windows DNS Server service on DC01 being stopped**.

The service was intentionally stopped as part of a controlled incident simulation:

```powershell
Stop-Service -Name DNS
```
While the service was stopped:

DC01 remained reachable over IP.
DNS queries to 10.10.20.10 timed out.
Internal DNS names could not be resolved.

The DNS service was subsequently restored:

Start-Service -Name DNS

After restoration, DNS resolution returned to normal.

2. Failure Condition

The affected service was:

DNS Server

The affected system was:

DC01
10.10.20.10

During the incident, the service state was:

Status: Stopped
StartType: Automatic

This prevented DC01 from servicing DNS queries.

3. Evidence Chain

The root cause is supported by the following sequence of observations.

Step 1 — Network connectivity remained healthy

The Windows 10 client successfully reached DC01:

ping 10.10.20.10

4 packets transmitted
4 packets received
0% packet loss

Therefore, the failure was not caused by complete loss of IP connectivity to DC01.

Step 2 — DNS resolution failed

During the failure condition:

nslookup dc01.corp.lab

returned DNS request timeouts.

The following query also failed:

nslookup corp.lab

A direct query against the configured DNS server also failed:

nslookup dc01.corp.lab 10.10.20.10

This established that DNS functionality was unavailable even though the server itself remained reachable.

Step 3 — DNS service was found stopped

The DNS Server service on DC01 was checked:

Get-Service -Name DNS | Select-Object Name, DisplayName, Status, StartType

Observed failure state:

Name         : DNS
DisplayName  : DNS Server
Status       : Stopped
StartType    : Automatic
Step 4 — Service restoration recovered DNS

The DNS service was restarted:

Start-Service -Name DNS

The resulting service state was:

Status: Running
StartType: Automatic

DNS resolution then succeeded:

dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10

This provides direct before/after evidence connecting the service state to the DNS behavior.

4. Root Cause
Immediate Cause

DNS Server service on DC01 was stopped.

Contributing Condition

The incident was intentionally created by stopping the DNS Server service as part of a controlled infrastructure failure simulation.

Not the Root Cause

The following conditions were investigated and were not responsible for the observed failure:

Client IP configuration
Client-to-DC01 IP connectivity
DC01 host availability
DNS zone deletion or corruption
Loss of the AD-integrated corp.lab zone
Loss of the AD-integrated _msdcs.corp.lab zone
5. Why the Failure Produced the Observed Symptoms

The Windows 10 client was configured to use:

10.10.20.10

as its DNS server.

When the DNS Server service on that address was stopped:

Windows 10
    │
    │ DNS query
    ▼
10.10.20.10
    │
    X
DNS Server service stopped

The client could still communicate with the server at the IP layer, but the DNS service was unavailable to answer name-resolution requests.

Therefore:

IP connectivity = Available
DNS functionality = Unavailable
6. Root Cause Classification
Category	Finding
Incident type	Service availability failure
Affected service	Windows DNS Server
Affected host	DC01
Failure state	DNS service stopped
Network connectivity	Operational
DNS zones	Intact
Client configuration	Operational
Resolution	DNS service restored
7. Recovery Verification

Recovery was considered successful only after both service state and application functionality were verified.

Service-level verification
DNS Server = Running
DNS functionality verification
dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10
Configuration verification

The following DNS zones remained present:

_msdcs.corp.lab
0.in-addr.arpa
127.in-addr.arpa
255.in-addr.arpa
corp.lab

The AD-integrated zones remained intact:

_msdcs.corp.lab
corp.lab
8. RCA Conclusion

The controlled incident was successfully isolated to the Windows DNS Server service availability layer.

The evidence supports the following causal chain:

DNS Server service stopped
        ↓
DNS service unavailable
        ↓
DNS queries timed out
        ↓
Internal name resolution failed
        ↓
DNS service restarted
        ↓
DNS service returned to Running
        ↓
DNS resolution restored

No evidence from this controlled incident indicates that the underlying IP network or DNS zone configuration was responsible for the outage.
