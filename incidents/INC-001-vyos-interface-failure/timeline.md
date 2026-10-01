# Incident Timeline — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected network:** `10.10.10.0/24` — Internal LAN  
**Affected interface:** `eth1` — `10.10.10.1/24`  
**Incident type:** Controlled infrastructure failure / routing impact  
**Severity:** Lab-controlled simulation

---

## Timeline

### 1. Baseline validation

The VyOS baseline was captured before failure injection.

`eth1` was operational:

```text
eth1  10.10.10.1/24  u/u
```
This established the expected healthy state.

2. Controlled failure injection

A controlled administrative shutdown was applied to eth1:

set interfaces ethernet eth1 disable

The configuration was committed.

This intentionally simulated an interface failure affecting the Internal LAN.

3. Interface failure observed

After the failure was committed, eth1 changed to:

eth1  10.10.10.1/24  A/D

The interface was administratively down and operationally down.

Detailed interface output reported:

state DOWN

while the IP address 10.10.10.1/24 remained configured.

4. Routing impact observed

The connected route for the Internal LAN disappeared:

show ip route 10.10.10.0/24

% Network not in table

The routing table no longer contained:

10.10.10.0/24 → directly connected via eth1

This demonstrated the direct relationship between the disabled interface and loss of the connected route.

5. Connectivity impact observed

A connectivity test toward 10.10.10.10 was performed while the interface was disabled.

The test produced:

9 packets transmitted, 0 received, 100% packet loss

The output also showed ICMP redirects from 192.168.0.1.

The connectivity failure was therefore consistent with the loss of the Internal LAN route.

6. Investigation

The interface configuration and operational state were inspected.

The interface showed:

eth1: ... state DOWN
inet 10.10.10.1/24

The configuration confirmed that the address remained configured on eth1.

The relevant failure condition was:

set interfaces ethernet eth1 disable

The Internal LAN DHCP configuration also remained associated with:

10.10.10.0/24

with gateway:

10.10.10.1
7. Remediation

The temporary failure configuration was removed:

delete interfaces ethernet eth1 disable

The configuration was committed.

8. Interface recovery

After remediation, eth1 returned to:

UP, LOWER_UP

with:

10.10.10.1/24

configured.

This confirmed that the interface itself had recovered.

9. Routing recovery

The connected route was restored:

Routing entry for 10.10.10.0/24
  Known via "connected"
  Status: Installed
  * directly connected, eth1

This confirmed recovery of the VyOS routing state for the Internal LAN.

10. Endpoint connectivity test

A subsequent ping to:

10.10.10.10

returned:

Destination Host Unreachable
100% packet loss

The Windows 10 endpoint was confirmed to be powered off at the time of this test.

Therefore, this result is documented as an endpoint availability condition and not as evidence that the VyOS interface remediation failed.

11. Final configuration verification

The final VyOS configuration was inspected.

The temporary failure statement:

set interfaces ethernet eth1 disable

was no longer present.

The remaining configuration included:

set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'

This confirmed that the deliberate failure-injection configuration had been removed.
