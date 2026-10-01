# Remediation & Recovery — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected Interface:** `eth1`  
**Affected Network:** `10.10.10.0/24` — Internal LAN  
**Remediation Type:** Configuration rollback / service recovery

---

## Remediation Objective

Restore the VyOS `eth1` interface to its intended operational state and confirm that the connected route for the Internal LAN is restored.

The remediation must:

1. Remove the temporary failure-injection configuration.
2. Commit the configuration.
3. Confirm interface recovery.
4. Confirm route recovery.
5. Confirm the failure-injection configuration is absent.

---

## 1. Identify the Failure Condition

The investigation identified the following configuration statement as the controlled failure condition:

```text
set interfaces ethernet eth1 disable
```

This statement caused eth1 to enter an administrative-down state.

**2. Remove the Failure Condition**

The temporary administrative disable was removed using:

delete interfaces ethernet eth1 disable

The configuration change was then committed:

commit

This restored the intended configuration state of the interface.

**3. Interface Recovery Validation**

The interface was inspected after the configuration was committed.

The resulting state was:

eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> ... state UP

The configured address remained:

10.10.10.1/24
Recovery finding

eth1 successfully returned to an operational state.

This confirmed that the interface-level failure condition had been removed.

**4. Routing Recovery Validation**

The routing table was checked for the affected Internal LAN:

show ip route 10.10.10.0/24

The resulting route was:

Routing entry for 10.10.10.0/24
  Known via "connected", distance 0, metric 0, best
  Status: Installed
  * directly connected, eth1, weight 1
Recovery finding

The connected route for 10.10.10.0/24 was restored through eth1.

This confirmed recovery of the Layer 3 routing state associated with the affected interface.

**5. Configuration Verification**

The final configuration was inspected with:

show configuration commands | match "eth1"

The final configuration contained:

set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INTERNA_LAN'

The temporary failure condition:

set interfaces ethernet eth1 disable

was absent.

Recovery finding

The failure-injection configuration was successfully removed.

**6. Endpoint Connectivity Consideration**

A connectivity test was performed against:

10.10.10.10

The result was:

Destination Host Unreachable
100% packet loss

The Windows 10 endpoint was confirmed to be powered off.

Therefore, the endpoint ping result was not interpreted as evidence of continued VyOS interface failure.

Recovery was instead validated through:

Interface operational state
IP configuration
Connected routing-table entry
Final configuration state

**7. Recovery Validation Matrix**
 
|Validation	                   | Expected State	                |    Observed State	    |   Result          |
|------------------------------|--------------------------------|-----------------------|-------------------|
|eth1 administrative state	   |       Up	                      |            Up	        |      PASS         |
|eth1 operational state	       |     Up	                        |      UP, LOWER_UP	    |    PASS           |
|eth1 IP address	             |    10.10.10.1/24	              |           Present	    |      PASS         |
|10.10.10.0/24 route	         |  Connected via eth1	          |             Present	  |        PASS       |
|Failure-injection disable	   |      Absent	                  |             Absent    |         PASS      |
|Windows 10.10.10.10	         |  Available for ping validation	|         Powered off	  |    NOT TESTABLE   |

**8. Remediation Result**

The remediation successfully restored the affected VyOS interface and its connected route.

The final state was:

eth1
  ↓
UP / LOWER_UP
  ↓
10.10.10.1/24
  ↓
10.10.10.0/24
  ↓
directly connected via eth1

The temporary failure-injection configuration was removed.

**9. Operational Lesson**

The recovery procedure demonstrates that interface failures should be validated at multiple layers rather than using a single connectivity test.

The recovery sequence used in this incident was:

Configuration
    ↓
Interface state
    ↓
Routing state
    ↓
Endpoint availability

This provides stronger evidence than simply confirming that an interface changed from down to up.

Remediation Conclusion

The controlled VyOS interface failure was successfully remediated by removing the administrative disable configuration and committing the change.

Recovery was independently verified through interface state, IP configuration, routing-table state, and final configuration inspection.

The endpoint connectivity test was excluded from the final recovery determination because the Windows 10 endpoint was powered off.

The remediation therefore restored the VyOS network path while keeping endpoint availability as a separate operational condition.
