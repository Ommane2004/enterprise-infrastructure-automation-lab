# INC-002 — DNS Failure Investigation

## 1. Investigation Objective

Determine why the Windows 10 client could reach the Domain Controller over IP but could not resolve internal DNS records.

The investigation followed a layered troubleshooting approach:

```text
Client Configuration
        ↓
IP Connectivity
        ↓
DNS Resolution
        ↓
DNS Server Service
        ↓
DNS Configuration
```
The objective was to identify the actual failure domain rather than assuming that a DNS resolution failure represented a general network outage.

2. Client DNS Configuration

The Windows 10 client was verified with the following relevant configuration:

Parameter	Value
Hostname	DESKTOP-JMEKHSC
IPv4	10.10.10.10/24
Default Gateway	10.10.10.1
DNS Server	10.10.20.10
DNS Suffix	corp.lab

The client was therefore correctly configured to use DC01 as its DNS server.

3. Initial Network Validation

The first diagnostic test was connectivity to the DNS server itself.

ping 10.10.20.10

Result:

4 packets transmitted
4 packets received
0% packet loss
Interpretation

The Windows 10 client had working IP connectivity to DC01.

This ruled out several broad network-layer possibilities, including:

Loss of connectivity to the server
Basic routing failure between the client and DC01
Complete interface failure on the destination
Complete host unavailability

The investigation therefore moved to the DNS service/application layer.

4. DNS Resolution Validation

The following DNS query was performed:

nslookup dc01.corp.lab

During the failure condition, the request timed out.

The domain itself was also tested:

nslookup corp.lab

This query also timed out.

A direct query against the configured DNS server was then performed:

nslookup dc01.corp.lab 10.10.20.10

This also timed out.

Interpretation

The failure was reproducible against the explicitly configured DNS server.

The evidence showed:

IP connectivity       = Working
DNS resolution        = Failed
Configured DNS server = 10.10.20.10

This significantly narrowed the failure domain.

5. DNS Server Investigation

The investigation then moved to DC01.

The DNS Server role was confirmed as installed.

The installed role included:

DNS
DNS Server

The DNS Server PowerShell management module was also present:

DnsServer
Version: 2.0.0.0

The DNS Server service was initially verified as:

Status: Running
StartType: Automatic

The DNS configuration included:

corp.lab
_msdcs.corp.lab

as AD-integrated primary zones.

The configured forwarder was:

8.8.8.8
6. Controlled Failure Correlation

A controlled failure was intentionally introduced by stopping the DNS Server service:

Stop-Service -Name DNS

The client was then retested.

The important observation was:

Ping DC01 = Successful
DNS query = Timeout

The DNS Server service was subsequently checked on DC01 and found to be:

Status: Stopped
StartType: Automatic
Correlation

The sequence established a direct relationship:

DNS service stopped
        ↓
DNS queries time out
        ↓
DNS name resolution fails

At the same time:

DNS service stopped
        ≠
DC01 unreachable

because IP connectivity continued to work.

7. Failure Domain Isolation

The investigation isolated the failure to the DNS service layer.

Layer-by-layer assessment
Layer / Component	Result
Windows client network configuration	Healthy
Client → DC01 IP connectivity	Healthy
DC01 host availability	Healthy
DNS Server role	Installed
DNS zones	Present
DNS Server service	Failed / stopped during incident
DNS resolution	Failed during incident

The evidence therefore supports the conclusion that the controlled incident was caused by the DNS Server service being stopped.

8. Remediation Validation

The DNS Server service was restored with:

Start-Service -Name DNS

The service was then verified:

Status: Running
StartType: Automatic

DNS functionality was tested again from Windows 10.

The following resolutions succeeded:

dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10

An explicit query against 10.10.20.10 also succeeded.

9. Final Assessment

The investigation demonstrated a complete troubleshooting chain:

Client Configuration
        ↓
IP Connectivity Test
        ↓
DNS Resolution Test
        ↓
DNS Server Investigation
        ↓
Controlled Failure Correlation
        ↓
Service Remediation
        ↓
Functional DNS Validation

The incident was successfully isolated to the DNS Server service and resolved by restoring that service.

10. Troubleshooting Lesson

A successful ping does not prove that a network service is operational.

In this incident:

10.10.20.10 reachable

did not mean:

DNS operational

The investigation therefore used a service-specific test (nslookup) in addition to an IP-level connectivity test (ping).

This prevented the DNS incident from being incorrectly classified as a general network outage.
