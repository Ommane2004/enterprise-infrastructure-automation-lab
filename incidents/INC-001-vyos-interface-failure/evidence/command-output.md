# Command Output Evidence — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected Interface:** `eth1`  
**Affected Network:** `10.10.10.0/24` — Internal LAN

---

## Evidence Handling

This document preserves the relevant command-output evidence collected during the controlled failure exercise.

Hardware-specific identifiers such as MAC addresses have been redacted because they are not required to demonstrate the troubleshooting process.

The outputs below are presented in chronological order.

---

# 1. Baseline — Interface State

### Command

```text
show interfaces
```
**Output**
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface    IP Address      MAC                VRF        MTU  S/L    Description
-----------  --------------  -----------------  -------  -----  -----  ------------------
eth0         192.168.0.2/24  [REDACTED]         default   1500  u/u    TRANSIT_TO_PFSENSE
eth1         10.10.10.1/24   [REDACTED]         default   1500  u/u    INTERNA_LAN
eth2         10.10.20.1/24   [REDACTED]         default   1500  u/u    SERVER_NETWORK
lo           127.0.0.1/8     [REDACTED]         default  65536  u/u
             ::1/128
Observation

eth1 was operational with:

10.10.10.1/24
u/u

This established the baseline interface state.

2. Baseline — Connected Route
Command
show ip route 10.10.10.0/24
Output
Routing entry for 10.10.10.0/24
  Known via "connected", distance 0, metric 0, best
  Last update [baseline]
  Flags: Selected
  Status: Installed
  * directly connected, eth1, weight 1
Observation

The Internal LAN was directly connected through eth1.

3. Failure Injection
Configuration Change

The controlled failure was introduced by disabling eth1:

set interfaces ethernet eth1 disable

The configuration was committed.

4. Failure State — Interface
Command
show interfaces
Output
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface    IP Address      MAC                VRF        MTU  S/L    Description
-----------  --------------  -----------------  -------  -----  -----  ------------------
eth0         192.168.0.2/24  [REDACTED]         default   1500  u/u    TRANSIT_TO_PFSENSE
eth1         10.10.10.1/24   [REDACTED]         default   1500  A/D    INTERNA_LAN
eth2         10.10.20.1/24   [REDACTED]         default   1500  u/u    SERVER_NETWORK
lo           127.0.0.1/8     [REDACTED]         default  65536  u/u
             ::1/128
Observation

eth1 changed from u/u to:

A/D

The interface was administratively down and operationally down.

5. Failure State — Routing
Command
show ip route 10.10.10.0/24
Output
% Network not in table
Observation

The connected route for 10.10.10.0/24 was no longer present.

6. Failure State — Connectivity
Command
ping 10.10.10.10
Output
PING 10.10.10.10 (10.10.10.10) 56(84) bytes of data.
From 192.168.0.1: icmp_seq=1 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=2 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=3 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=4 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=5 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=6 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=7 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=8 Redirect Host(New nexthop: 192.168.0.2)
From 192.168.0.1: icmp_seq=9 Redirect Host(New nexthop: 192.168.0.2)

9 packets transmitted, 0 received, 100% packet loss
Observation

The Internal LAN endpoint was unreachable while the connected route was absent.

7. Failure State — Detailed Interface Inspection
Command
show interfaces ethernet eth1
Output
eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether [REDACTED] brd ff:ff:ff:ff:ff:ff
    altname enp2s4
    altname ens36
    inet 10.10.10.1/24 brd 10.10.10.255 scope global eth1
       valid_lft forever preferred_lft forever
    Description: INTERNA_LAN

    RX:  bytes  packets  errors  dropped  overrun       mcast
             0        0       0        0        0           0
    TX:  bytes  packets  errors  dropped  carrier  collisions
          8926      142       0        0        0           0
Observation

The IP address remained configured, while the interface itself was down.

8. Failure State — Configuration
Command
show configuration commands | match "eth1"
Output
set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'
set interfaces ethernet eth1 disable
set interfaces ethernet eth1 hw-id '[REDACTED]'
set interfaces ethernet eth1 offload gro
set interfaces ethernet eth1 offload gso
set interfaces ethernet eth1 offload sg
set interfaces ethernet eth1 offload tso
Observation

The administrative disable statement was present in the active configuration.

9. Remediation
Configuration Change

The controlled failure condition was removed:

delete interfaces ethernet eth1 disable

The configuration was committed:

commit
10. Recovery — Interface
Command
show interfaces ethernet eth1
Output
eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether [REDACTED] brd ff:ff:ff:ff:ff:ff
    altname enp2s4
    altname ens36
    inet 10.10.10.1/24 brd 10.10.10.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe03:8116/64 scope link
       valid_lft forever preferred_lft forever
    Description: INTERNA_LAN
Observation

eth1 returned to:

UP, LOWER_UP

The configured address remained:

10.10.10.1/24
11. Recovery — Routing
Command
show ip route 10.10.10.0/24
Output
Routing entry for 10.10.10.0/24
  Known via "connected", distance 0, metric 0, best
  Last update 00:00:26 ago
  Flags: Selected
  Status: Installed
  * directly connected, eth1, weight 1
Observation

The connected route for the Internal LAN was restored through eth1.

12. Recovery — Endpoint Test
Command
ping 10.10.10.10
Output
PING 10.10.10.10 (10.10.10.10) 56(84) bytes of data.
From 10.10.10.1 icmp_seq=1 Destination Host Unreachable
From 10.10.10.1 icmp_seq=2 Destination Host Unreachable
From 10.10.10.1 icmp_seq=3 Destination Host Unreachable
From 10.10.10.1 icmp_seq=4 Destination Host Unreachable
From 10.10.10.1 icmp_seq=5 Destination Host Unreachable
From 10.10.10.1 icmp_seq=6 Destination Host Unreachable
From 10.10.10.1 icmp_seq=7 Destination Host Unreachable
From 10.10.10.1 icmp_seq=8 Destination Host Unreachable
From 10.10.10.1 icmp_seq=9 Destination Host Unreachable

10 packets transmitted, 0 received, +9 errors, 100% packet loss
Interpretation

The VyOS interface and connected route had already been independently verified as recovered.

The Windows 10 endpoint was confirmed to be powered off during this test.

Therefore, this result is recorded as an endpoint availability condition, not as evidence that the VyOS interface remediation failed.

13. Final Configuration Verification
Command
show configuration commands | match "eth1"
Output
set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'
set interfaces ethernet eth1 hw-id '[REDACTED]'
set interfaces ethernet eth1 offload gro
set interfaces ethernet eth1 offload gso
set interfaces ethernet eth1 offload sg
set interfaces ethernet eth1 offload tso
Observation

The failure-injection statement:

set interfaces ethernet eth1 disable

was no longer present.

Evidence Summary
Evidence	Failure State	Recovery State
eth1	A/D	UP, LOWER_UP
10.10.10.1/24	Configured	Configured
10.10.10.0/24 route	Absent	Connected via eth1
eth1 disable	Present	Absent
Endpoint ping	100% loss	Not testable because endpoint powered off
Evidence Conclusion

The command outputs establish the following sequence:

eth1 disabled
    ↓
eth1 A/D
    ↓
10.10.10.0/24 route absent
    ↓
Internal LAN connectivity impacted
    ↓
eth1 disable removed
    ↓
eth1 UP/LOWER_UP
    ↓
10.10.10.0/24 route restored
    ↓
Failure-injection configuration absent

The evidence demonstrates the controlled interface failure, its routing consequence, the configuration-level root cause, and the subsequent recovery of the VyOS interface and connected route.
