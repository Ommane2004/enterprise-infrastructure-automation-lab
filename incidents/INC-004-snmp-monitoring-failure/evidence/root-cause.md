INC-004 — Root Cause Analysis
Summary

The controlled SNMP monitoring failures on VyOS and pfSense were caused by their respective SNMP services being stopped or disabled during testing.

In both cases, ICMP connectivity from Zabbix01 to the device remained available. The evidence therefore distinguishes SNMP service availability from general IP reachability.

Root Cause A — VyOS
Failure mechanism

The VyOS SNMP daemon was deliberately stopped using:

sudo systemctl stop snmpd

The subsequent service-state check returned inactive.

Evidence
Before the test, VyOS SNMP metrics were visible in Zabbix.
During the test, the SNMP daemon was inactive.
Ping from Zabbix01 to 192.168.0.2 succeeded with 4 replies and 0% packet loss.
The daemon was restarted and subsequently confirmed active.
Root cause

Immediate cause: The SNMP daemon was stopped, preventing normal SNMP monitoring while the service was unavailable.

Scope and limitation

This was a deliberate failure injection, not an unexpected service crash. The test does not establish why an SNMP daemon might stop unexpectedly in a production environment.

The daemon's return to active confirms service-level recovery. Confirm the corresponding Zabbix metric recovery before claiming end-to-end monitoring recovery.

Root Cause B — pfSense
Failure mechanism

The SNMP daemon was deliberately disabled through the pfSense web interface under Services → SNMP, and the change was saved/applied.

Zabbix reported the SNMP agent availability item as not available (0).

Evidence
Before the test, pfSense monitoring data was visible in Zabbix.
During the test, the SNMP availability item reported not available (0).
Ping from Zabbix01 to 192.168.0.1 succeeded with 4 replies and 0% packet loss.
After SNMP was re-enabled and applied, Zabbix reported available (1).
Root cause

Immediate cause: SNMP was disabled on pfSense, interrupting SNMP-based monitoring.

Scope and limitation

This was a controlled failure injection, not evidence of an unexpected production failure. The test does not establish an underlying defect in pfSense or Zabbix.

Why IP Connectivity Was Not the Root Cause

ICMP ping and SNMP test different aspects of device availability:

ICMP ping checks whether the device responds to network reachability probes.
SNMP monitoring depends on the SNMP service being available and reachable, with the required configuration and access permissions.

A successful ping does not prove that SNMP is working. Conversely, an SNMP monitoring failure does not, by itself, prove that the device or its network path is down.

Contributing Factors

No additional contributing fault was established by these controlled tests. The service state was deliberately changed to reproduce the monitoring failure.

Preventive Recommendations
Configure Zabbix alerts for SNMP agent unavailability.
Monitor both device reachability and SNMP availability.
Investigate service-state changes and monitoring gaps separately from network outages.
Document the failure-injection and recovery procedures.
Verify recovery from both the device and Zabbix perspectives.
Protect SNMP community strings and configuration details from public exposure.
Final Assessment

The evidence supports a service-availability root cause for both devices: SNMP was deliberately stopped or disabled. IP reachability remained intact during both tests. pfSense's SNMP availability recovery was confirmed in Zabbix; VyOS service recovery was confirmed, with its Zabbix metric recovery requiring verification if not already demonstrated by the evidence.
