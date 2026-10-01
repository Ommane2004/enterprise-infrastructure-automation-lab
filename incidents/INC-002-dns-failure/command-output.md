# INC-002 — Command Output Evidence

## Purpose

This document records sanitized command output collected during the controlled DNS failure simulation.

The evidence demonstrates:

```text
Baseline
    ↓
Failure Injection
    ↓
Failure Validation
    ↓
Root Cause Confirmation
    ↓
Remediation
    ↓
Recovery Validation
```
No credentials, tokens, private keys, passwords, or other secrets are included.

1. Baseline — Windows 10 DNS Configuration

The Windows 10 client was configured to use DC01 as its DNS server.

Relevant configuration:

Host Name: DESKTOP-JMEKHSC
Primary DNS Suffix: corp.lab

IPv4 Address: 10.10.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 10.10.10.1
DHCP Server: 10.10.10.1
DNS Servers: 10.10.20.10
2. Baseline — DC01 DNS Service

Command:

Get-Service -Name DNS | Select-Object Name, DisplayName, Status, StartType

Output:

Name DisplayName  Status  StartType
---- -----------  ------  ---------
DNS  DNS Server   Running Automatic

This established the healthy DNS service baseline.

3. Baseline — DNS Zones

Command:

Get-DnsServerZone | Select-Object ZoneName, ZoneType, IsDsIntegrated

Output:

ZoneName         ZoneType IsDsIntegrated
--------         -------- --------------
_msdcs.corp.lab  Primary  True
0.in-addr.arpa   Primary  False
127.in-addr.arpa Primary  False
255.in-addr.arpa Primary  False
corp.lab         Primary  True

The important AD-integrated zones were:

_msdcs.corp.lab
corp.lab
4. Baseline — DNS Forwarder

Command:

Get-DnsServerForwarder | Select-Object IPAddress, UseRootHint

Output:

IPAddress UseRootHint
--------- -----------
8.8.8.8   True
5. Baseline — DC01 Connectivity

Command from Windows 10:

ping 10.10.20.10

Result:

Reply from 10.10.20.10: bytes=32 time=1ms TTL=127
Reply from 10.10.20.10: bytes=32 time=1ms TTL=127
Reply from 10.10.20.10: bytes=32 time<1ms TTL=127
Reply from 10.10.20.10: bytes=32 time=1ms TTL=127

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
6. Baseline — DNS Resolution

Command:

nslookup dc01.corp.lab

Result:

Server:  UnKnown
Address: 10.10.20.10

Name:    dc01.corp.lab
Address: 10.10.20.10

Command:

nslookup corp.lab

Result:

Server:  UnKnown
Address: 10.10.20.10

Name:    corp.lab
Address: 10.10.20.10

Server: UnKnown was not treated as an outage because the DNS queries successfully returned the expected records.

7. Failure Injection

The DNS Server service was intentionally stopped on DC01.

Command:

Stop-Service -Name DNS

The command completed without an error.

8. Failure-State Service Verification

Command:

Get-Service -Name DNS | Select-Object Name, DisplayName, Status, StartType

Output:

Name DisplayName  Status  StartType
---- -----------  ------  ---------
DNS  DNS Server   Stopped Automatic

This established the server-side failure state.

9. Failure Validation — IP Connectivity

Command:

ping 10.10.20.10

Result:

Reply from 10.10.20.10: bytes=32 time=1ms TTL=127
Reply from 10.10.20.10: bytes=32 time=1ms TTL=127
Reply from 10.10.20.10: bytes=32 time<1ms TTL=127
Reply from 10.10.20.10: bytes=32 time=1ms TTL=127

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
Interpretation

DC01 remained reachable despite the DNS service failure.

10. Failure Validation — DNS Query

Command:

nslookup dc01.corp.lab

Result:

DNS request timed out.
timeout was 2 seconds.

Server:  UnKnown
Address: 10.10.20.10

DNS request timed out.
DNS request timed out.
DNS request timed out.
DNS request timed out.

*** Request to UnKnown timed-out
11. Failure Validation — Domain Query

Command:

nslookup corp.lab

Result:

DNS request timed out.
timeout was 2 seconds.

Server:  UnKnown
Address: 10.10.20.10

DNS request timed out.
DNS request timed out.
DNS request timed out.
DNS request timed out.

*** Request to UnKnown timed-out
12. Failure Validation — Explicit DNS Server

Command:

nslookup dc01.corp.lab 10.10.20.10

Result:

DNS request timed out.
timeout was 2 seconds.

Server:  UnKnown
Address: 10.10.20.10

DNS request timed out.
DNS request timed out.
DNS request timed out.
DNS request timed out.

*** Request to UnKnown timed-out
Interpretation

The explicit DNS-server query also failed.

This strengthened the conclusion that the issue was with DNS service availability rather than DNS client server selection.

13. Remediation

The DNS Server service was restored.

Command:

Start-Service -Name DNS

The command completed without an error.

14. Post-Remediation Service Verification

Command:

Get-Service -Name DNS | Select-Object Name, DisplayName, Status, StartType

Output:

Name DisplayName  Status  StartType
---- -----------  ------  ---------
DNS  DNS Server   Running Automatic
15. Recovery — DC01 Resolution

Command:

nslookup dc01.corp.lab

Result:

Server:  UnKnown
Address: 10.10.20.10

Name:    dc01.corp.lab
Address: 10.10.20.10
16. Recovery — Domain Resolution

Command:

nslookup corp.lab

Result:

Server:  UnKnown
Address: 10.10.20.10

Name:    corp.lab
Address: 10.10.20.10
17. Recovery — Explicit DNS Server

Command:

nslookup dc01.corp.lab 10.10.20.10

Result:

Server:  UnKnown
Address: 10.10.20.10

Name:    dc01.corp.lab
Address: 10.10.20.10
18. Final DNS Zone Validation

Command:

Get-DnsServerZone | Select-Object ZoneName, ZoneType, IsDsIntegrated

Output:

ZoneName         ZoneType IsDsIntegrated
--------         -------- --------------
_msdcs.corp.lab  Primary  True
0.in-addr.arpa   Primary  False
127.in-addr.arpa Primary  False
255.in-addr.arpa Primary  False
corp.lab         Primary  True

The DNS zones remained intact after recovery.

19. Evidence Summary
Test	Baseline	Failure	Recovery
DC01 IP connectivity	PASS	PASS	PASS
DNS service	Running	Stopped	Running
dc01.corp.lab	PASS	TIMEOUT	PASS
corp.lab	PASS	TIMEOUT	PASS
Explicit DNS query	PASS	TIMEOUT	PASS
DNS zones	Present	Present	Present
20. Incident Conclusion

The evidence establishes the following sequence:

DNS service Running
        ↓
DNS resolution Working
        ↓
DNS service intentionally stopped
        ↓
DNS resolution Failed
        ↓
IP connectivity remained Working
        ↓
DNS service restarted
        ↓
DNS resolution Restored
        ↓
DNS zones verified intact

The controlled failure was successfully reproduced, diagnosed, remediated, and validated.

INC-002 — RESOLVED
