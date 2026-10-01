# INC-002 — Incident Timeline

**Incident:** DNS Service Failure  
**System:** DC01  
**Client:** DESKTOP-JMEKHSC  
**DNS Server:** 10.10.20.10  
**Status:** Resolved

---

## 1. Baseline

The Windows 10 client was verified to have the expected network configuration:

- IPv4 address: `10.10.10.10/24`
- Default gateway: `10.10.10.1`
- DNS server: `10.10.20.10`
- DNS suffix: `corp.lab`

Initial DNS validation was successful.

### DC01 reachability

```text
ping 10.10.20.10
4 packets transmitted
4 packets received
0% packet loss
```
**DNS resolution**
dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10

The DNS Server service on DC01 was confirmed as:

Status: Running
StartType: Automatic

**2. DNS Configuration Baseline**

The DNS Server role was confirmed to be installed on DC01.

The DNS Server PowerShell module was loaded successfully.

Configured zones included:

_msdcs.corp.lab
0.in-addr.arpa
127.in-addr.arpa
255.in-addr.arpa
corp.lab

The AD-integrated zones were:

_msdcs.corp.lab
corp.lab

The configured DNS forwarder was:

8.8.8.8

**3. Failure Injection**

A controlled DNS service failure was introduced on DC01.

The DNS Server service was intentionally stopped:

Stop-Service -Name DNS

The purpose was to simulate an internal DNS service outage while keeping the underlying network path operational.

**4. Failure Validation**

The Windows 10 client was tested after the failure was introduced.

IP connectivity remained available
ping 10.10.20.10

4 packets transmitted
4 packets received
0% packet loss

This demonstrated that the client could still communicate with DC01 over IP.

DNS resolution failed

The following query timed out:

nslookup dc01.corp.lab

The domain lookup also timed out:

nslookup corp.lab

An explicit query against the configured DNS server also timed out:

nslookup dc01.corp.lab 10.10.20.10

This confirmed that the failure was specifically affecting DNS functionality rather than basic IP connectivity.

**5. Server-Side Failure Confirmation**

The DNS Server service was checked on DC01 after the client-side failure was observed.

Observed state:

DNS Server
Status: Stopped
StartType: Automatic

This correlated the client-side DNS timeouts with the stopped DNS service.

**6. Remediation**

The DNS Server service was restarted on DC01:

Start-Service -Name DNS

The service returned to:

Status: Running
StartType: Automatic

**7. Functional Recovery Validation**

After the service was restored, DNS resolution was tested again from Windows 10.

DC01 hostname
dc01.corp.lab → 10.10.20.10
Domain
corp.lab → 10.10.20.10
Explicit DNS server

A direct query against:

10.10.20.10

also succeeded.

**8. Final Configuration Validation**

The DNS zones were queried after recovery.

The following zones remained present:

_msdcs.corp.lab
0.in-addr.arpa
127.in-addr.arpa
255.in-addr.arpa
corp.lab

The AD-integrated zones remained intact:

_msdcs.corp.lab
corp.lab

**9. Incident Closure**

The incident was considered resolved after both service-state and functional validation succeeded.

Service validation
DNS Server = Running
Functional validation
dc01.corp.lab → 10.10.20.10
corp.lab      → 10.10.20.10
Final state
DNS service operational
DNS zones intact
Client DNS resolution restored

Incident Status: RESOLVED
