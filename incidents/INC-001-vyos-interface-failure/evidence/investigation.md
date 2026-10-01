# Investigation Evidence — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Affected Interface:** `eth1`  
**Affected Network:** `10.10.10.0/24` — Internal LAN  
**Investigation Type:** Controlled infrastructure failure analysis

---

## Investigation Objective

Determine why the `10.10.10.0/24` Internal LAN became unreachable after the controlled failure was introduced on VyOS.

The investigation focused on four areas:

1. Interface operational state
2. Routing-table state
3. Network connectivity
4. Active VyOS configuration

---

## 1. Interface State Investigation

### Command

```text
show interfaces
```
Failure-state observation

The affected interface reported:

eth1  10.10.10.1/24  A/D  INTERNA_LAN

Detailed interface inspection showed:

eth1: <BROADCAST,MULTICAST> ... state DOWN
inet 10.10.10.1/24
Interpretation

The interface was administratively disabled and operationally down.

The IP address remained configured on the interface, indicating that the problem was not caused by removal of the IP configuration.

The interface state was therefore identified as the first significant failure indicator.

2. Routing Investigation
Command
show ip route 10.10.10.0/24
Failure-state observation

VyOS returned:

% Network not in table

Before the failure, the same network had been present as:

Routing entry for 10.10.10.0/24
  Known via "connected"
  Status: Installed
  * directly connected, eth1, weight 1
Interpretation

The connected route for the Internal LAN disappeared when eth1 was disabled.

This established a direct relationship between the interface failure and the routing-table change.

3. Connectivity Investigation
Command
ping 10.10.10.10
Failure-state observation

The test produced:

9 packets transmitted, 0 received, 100% packet loss

The output also showed ICMP redirects from:

192.168.0.1
Interpretation

The Internal LAN endpoint was not reachable while the 10.10.10.0/24 connected route was absent.

The connectivity result was consistent with the routing failure.

The ICMP redirect messages were treated as supporting diagnostic output rather than the root cause.

4. Configuration Investigation
Command
show configuration commands | match "eth1"
Failure-state observation

The active configuration contained:

set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'
set interfaces ethernet eth1 disable
Interpretation

The explicit administrative disable statement was directly responsible for the observed interface state.

The IP address and interface configuration remained present, while the administrative disable prevented the interface from operating.

5. DHCP Configuration Dependency

The VyOS configuration also contained the Internal LAN DHCP configuration for:

10.10.10.0/24

The configured gateway was:

10.10.10.1

The configured DNS server was:

10.10.20.10

The configured address pool was:

10.10.10.10 - 10.10.10.100
Interpretation

eth1 provides the Layer 3 gateway interface associated with the Internal LAN DHCP scope.

Therefore, loss of eth1 affects more than interface availability: it removes the directly connected route required for normal Layer 3 communication with that subnet.

6. Evidence Correlation

The investigation produced the following evidence chain:
|--------------------------|------------------------------------|------------------------------------------|
|Investigation Area	       |          Evidence	                |       Finding                            |
|--------------------------|------------------------------------|------------------------------------------|
|Interface state	         |          eth1 = A/D	              |      Interface administratively disabled |
|Detailed interface state	 |         state DOWN	                |    Interface unavailable                 |
|Routing table         	   |      10.10.10.0/24 absent	        |    Connected route removed               |
|Connectivity	             |      100% packet loss       	      |    Internal endpoint unreachable         |
|Configuration	           |        eth1 disable present   	    |      Direct failure condition identified |
|DHCP configuration	       |   10.10.10.0/24 gateway 10.10.10.1	|    Interface is the LAN gateway          |
|--------------------------|------------------------------------|------------------------------------------|
7. Diagnostic Reasoning

The investigation followed this sequence:

Observed connectivity failure
        ↓
Checked routing table
        ↓
10.10.10.0/24 route absent
        ↓
Checked interface state
        ↓
eth1 = A/D
        ↓
Checked active configuration
        ↓
eth1 disable present
        ↓
Failure condition identified

This avoided treating the endpoint connectivity failure as an isolated symptom.

The routing-table change provided the intermediate evidence connecting the interface state to the connectivity impact.

8. Root Cause Determination

The controlled incident was caused by the administrative disablement of the VyOS eth1 interface:

set interfaces ethernet eth1 disable

This caused:

eth1 to enter an administrative-down state.
The connected 10.10.10.0/24 route to be removed.
Traffic destined for the Internal LAN to lose its directly connected route.
Connectivity to the Internal LAN to fail.
9. Post-Remediation Investigation

After removing the failure condition and committing the configuration:

delete interfaces ethernet eth1 disable
commit

the interface returned to:

UP, LOWER_UP

The 10.10.10.0/24 route returned as:

directly connected, eth1

The final configuration no longer contained:

set interfaces ethernet eth1 disable

These observations confirmed that the controlled failure condition had been successfully removed.

10. Endpoint Availability Consideration

A subsequent ping to 10.10.10.10 still failed.

The Windows 10 endpoint was confirmed to be powered off.

Therefore, the endpoint ping failure was not used as evidence against the VyOS remediation.

The recovery conclusion was based on:

eth1 returning to UP, LOWER_UP
10.10.10.0/24 returning as a connected route
Removal of the administrative disable configuration
Investigation Conclusion

The investigation established a clear evidence chain between the injected configuration change, interface state, routing-table change, and network impact.

The primary diagnostic indicators were:

eth1 → A/D
10.10.10.0/24 → absent
eth1 disable → present

After remediation:

eth1 → UP/LOWER_UP
10.10.10.0/24 → directly connected via eth1
eth1 disable → absent

The investigation therefore confirms that the controlled eth1 administrative shutdown caused the observed VyOS routing impact.
