# Root Cause Analysis — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected Interface:** `eth1`  
**Affected Network:** `10.10.10.0/24` — Internal LAN  
**Incident Type:** Controlled infrastructure failure

---

## Root Cause

The direct cause of the incident was the administrative disablement of the VyOS `eth1` interface.

The failure condition was:

```text
set interfaces ethernet eth1 disable
```

This configuration change caused eth1 to enter an administrative-down state.

Because 10.10.10.0/24 was directly connected through eth1, disabling the interface also caused the connected route for that network to be removed from the routing table.

Causal Chain
Administrative disablement of eth1
                ↓
eth1 enters A/D state
                ↓
eth1 stops providing the Internal LAN interface
                ↓
Connected route 10.10.10.0/24 is removed
                ↓
VyOS no longer has a directly connected route
to the Internal LAN
                ↓
Connectivity to the Internal LAN is disrupted
Evidence Supporting the Root Cause
Interface State

During the failure:

eth1  10.10.10.1/24  A/D

Detailed inspection reported:

state DOWN

This established that the affected interface was unavailable.

Routing State

During the failure:

show ip route 10.10.10.0/24

% Network not in table

Before the failure, the network was:

10.10.10.0/24
directly connected, eth1

The disappearance of the connected route correlated directly with the interface being disabled.

Configuration State

The active configuration during the failure contained:

set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'
set interfaces ethernet eth1 disable

The explicit disable statement is the direct configuration-level cause of the interface failure.

Impact

The affected network was:

10.10.10.0/24

The VyOS gateway for this network was:

10.10.10.1

When eth1 was disabled:

The interface became administratively down.
The connected route for 10.10.10.0/24 disappeared.
Traffic toward the Internal LAN could no longer use the directly connected route.
A connectivity test to 10.10.10.10 resulted in 100% packet loss.
Contributing Configuration Dependency

The 10.10.10.0/24 network is also configured as a VyOS DHCP subnet.

Its configured gateway is:

10.10.10.1

Therefore, eth1 represents the Layer 3 gateway interface for the Internal LAN.

The interface failure consequently affected both the interface availability and the routing path associated with that subnet.

Five-Why Analysis
Why did the Internal LAN become unreachable?

Because the connected route for 10.10.10.0/24 disappeared from the VyOS routing table.

Why did the connected route disappear?

Because the interface providing the directly connected network, eth1, was disabled.

Why was eth1 disabled?

Because the controlled incident deliberately introduced:

set interfaces ethernet eth1 disable
Why was this configuration introduced?

It was intentionally introduced as a controlled failure injection to simulate an infrastructure interface failure.

Why was the failure injection useful?

It provided a reproducible way to validate the relationship between:

Interface state
Connected routing
Network reachability
Troubleshooting methodology
Configuration-based remediation

**Root Cause Classification**

|Category	               |        Finding                          |
|------------------------|-----------------------------------------|
|Failure domain	         | Network interface                       |
|Device	                 |         VyOS                            |
|Interface	             |           eth1                          |
|Network	               |       10.10.10.0/24                     |
|Direct cause	           |Administrative interface disable         |
|Configuration trigger	 |  set interfaces ethernet eth1 disable   |
|Routing consequence	   |  Connected route removed                |
|Network consequence	   |  Internal LAN reachability disrupted    |
|Incident type	         |  Controlled lab failure injection       |

**Remediation**

The failure condition was removed with:

delete interfaces ethernet eth1 disable

The configuration was committed.

After remediation:

eth1 → UP, LOWER_UP

and:

10.10.10.0/24 → directly connected via eth1

The final configuration no longer contained the disable statement.

**Recovery Validation**

The following recovery conditions were verified:

Interface
eth1: ... UP, LOWER_UP
IP Address
10.10.10.1/24
Connected Route
10.10.10.0/24
directly connected, eth1
Configuration

The temporary failure-injection statement was absent from the final configuration.

These checks demonstrate that the VyOS interface and routing configuration were restored.

**Endpoint Availability Note**

A subsequent connectivity test to 10.10.10.10 returned:

Destination Host Unreachable
100% packet loss

The Windows 10 endpoint was confirmed to be powered off.

Therefore, the endpoint ping result is not classified as a failure of the VyOS remediation.

The RCA is based on the independently verified recovery of the VyOS interface, connected route, and configuration.

**RCA Conclusion**

The controlled incident was caused by administratively disabling VyOS eth1.

The evidence demonstrates the following relationship:

eth1 disabled
    ↓
eth1 A/D
    ↓
10.10.10.0/24 connected route removed
    ↓
Internal LAN routing impact

Removing the administrative disable restored:

eth1 UP/LOWER_UP
    ↓
10.10.10.0/24 directly connected via eth1

The incident therefore provides a reproducible demonstration of how an interface-level configuration change can propagate into a routing and network-reachability failure.

