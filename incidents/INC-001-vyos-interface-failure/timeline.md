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

### 1. Baseline Validation

The VyOS baseline was captured before failure injection.

The `eth1` interface was operational:

```text
eth1  10.10.10.1/24  u/u
```

The 10.10.10.0/24 network was present as a directly connected route:

Routing entry for 10.10.10.0/24
  Known via "connected", distance 0, metric 0, best
  Status: Installed
  * directly connected, eth1, weight 1

This established the expected healthy state before the controlled failure.

2. Controlled Failure Injection

A controlled administrative shutdown was applied to eth1:

set interfaces ethernet eth1 disable

The configuration was committed.

This intentionally simulated an interface failure affecting the Internal LAN.

3. Interface Failure Observed

After the failure was committed, eth1 changed to:

eth1  10.10.10.1/24  A/D

The interface was administratively down and operationally down.

Detailed interface output reported:

eth1: <BROADCAST,MULTICAST> ... state DOWN
inet 10.10.10.1/24

The IP address remained configured, but the interface was unavailable.

4. Routing Impact Observed

The connected route for the Internal LAN disappeared from the routing table:

show ip route 10.10.10.0/24

% Network not in table

The previously present connected route:

10.10.10.0/24 → directly connected via eth1

was no longer installed.

The remaining routing table retained the other active networks, including the server network and the VyOS transit network.

5. Connectivity Impact Observed

A connectivity test toward 10.10.10.10 was performed while eth1 was disabled.

The test produced:

9 packets transmitted, 0 received, 100% packet loss

The output also showed ICMP redirects from 192.168.0.1.

The connectivity failure was consistent with the loss of the connected 10.10.10.0/24 route.

6. Investigation

The operational state of eth1 was inspected:

eth1: ... state DOWN
inet 10.10.10.1/24
Description: INTERNA_LAN

The interface statistics showed the interface was down while the IP configuration remained present.

The configuration was inspected and confirmed the failure condition:

set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'
set interfaces ethernet eth1 disable

The VyOS configuration also showed that the 10.10.10.0/24 network was configured as an internal DHCP subnet with:

Default gateway: 10.10.10.1
DNS server: 10.10.20.10
DHCP range: 10.10.10.10 through 10.10.10.100

This confirmed that eth1 provides the Layer 3 gateway interface for the Internal LAN.

7. Root Cause Identified

The direct cause of the controlled incident was the administrative disablement of the VyOS eth1 interface.

The failure condition was:

set interfaces ethernet eth1 disable

Because 10.10.10.0/24 was directly connected through eth1, disabling the interface caused the connected route to disappear from the routing table.

Therefore:

Administrative interface shutdown
        ↓
eth1 becomes A/D
        ↓
Connected route 10.10.10.0/24 disappears
        ↓
Internal LAN loses routing through VyOS
8. Remediation

The temporary failure configuration was removed:

delete interfaces ethernet eth1 disable

The configuration was then committed.

9. Interface Recovery

After remediation, eth1 returned to an operational state:

eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> ... state UP

The interface again carried:

10.10.10.1/24

This confirmed recovery of the VyOS interface.

10. Routing Recovery

The connected route was restored:

Routing entry for 10.10.10.0/24
  Known via "connected", distance 0, metric 0, best
  Status: Installed
  * directly connected, eth1, weight 1

This confirmed recovery of the VyOS routing state for the Internal LAN.

11. Endpoint Connectivity Validation

A subsequent ping to:

10.10.10.10

returned:

Destination Host Unreachable
100% packet loss

The Windows 10 endpoint was confirmed to be powered off at the time of this test.

Therefore, this result is not treated as evidence that the VyOS interface remediation failed.

The VyOS interface and connected route had already been independently verified as recovered.

Endpoint availability was treated as a separate condition.

12. Final Configuration Verification

The final VyOS configuration was inspected.

The temporary failure-injection statement:

set interfaces ethernet eth1 disable

was no longer present.

The final configuration contained:

set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'

along with the existing interface configuration.

This confirmed that the controlled failure condition had been removed.

Final Incident State
Component	Final State
VyOS eth1	Recovered
10.10.10.1/24	Configured
10.10.10.0/24 connected route	Restored
Failure-injection disable statement	Removed
VyOS configuration	Remediated
Windows 10.10.10.10	Powered off
Endpoint ping	Not used as final network-recovery proof
Incident Conclusion

The controlled failure successfully demonstrated that administratively disabling the VyOS eth1 interface removes the directly connected 10.10.10.0/24 route and causes loss of reachability to the Internal LAN.

The failure was investigated through interface-state, routing-table, connectivity, and configuration evidence.

Removing the administrative disable and committing the configuration restored the VyOS interface and the connected Internal LAN route.

The subsequent inability to reach 10.10.10.10 was separately explained by the Windows 10 endpoint being powered off and was therefore not attributed to the remediated VyOS interface failure.

The incident demonstrates the relationship between:

Interface state → connected route → network reachability → troubleshooting evidence → remediation → recovery validation.
