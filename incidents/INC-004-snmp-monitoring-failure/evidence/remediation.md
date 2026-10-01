INC-004 — Remediation and Recovery
Objective

Restore SNMP monitoring after the controlled failure injections on VyOS and pfSense, then validate recovery without confusing service availability with IP reachability.

Remediation A — VyOS
Action

The SNMP daemon had been deliberately stopped during the test. On the VyOS device, it was restarted using:

sudo systemctl start snmpd
Service validation

Check the service state:

sudo systemctl is-active snmpd

Observed result: active

Monitoring validation

In Zabbix:

1.Open Monitoring → Latest data.
2.Select the VyOS host.
3.Locate the relevant SNMP items.
4.Check that the items are receiving fresh values and their last-check timestamps are advancing.

Evidence status: The daemon's return to active was confirmed. Record end-to-end monitoring recovery as confirmed only if the captured Zabbix evidence demonstrates that SNMP metrics resumed.

Remediation B — pfSense
Action
1.Open the pfSense web interface.
2.Navigate to Services → SNMP.
3.Enable the SNMP daemon.
4.Save and apply the configuration.
5.Monitoring validation

In Zabbix, inspect the pfSense SNMP agent availability item.

Observed result after recovery: available (1)

This confirms that the Zabbix availability check returned to the available state.

Connectivity Validation

During both failure injections, ping from Zabbix01 to the affected device succeeded:

|Device	     |         Target	         |        Result during SNMP failure       |
|------------|-------------------------|-----------------------------------------|
|VyOS	       |      192.168.0.2	       |       4 replies; 0% packet loss         |
|pfSense     |    	192.168.0.1	4      |           replies; 0% packet loss       |

These results support the conclusion that IP reachability remained intact during the controlled SNMP failures.

Recovery Acceptance Criteria

Use the following checklist when validating future SNMP incidents:

SNMP service is enabled and running on the affected device.

Zabbix reports the SNMP availability item as available.

SNMP items are receiving fresh data.

IP reachability is confirmed independently.

No credentials or SNMP community strings appear in published evidence.

Recovery observations are documented with screenshots or command output.

Preventive Improvements
1.Configure alerts for SNMP availability failures.
2.Monitor device reachability separately from SNMP availability.
3.Investigate service-state changes when monitoring disappears.
4.Keep a documented recovery procedure for each monitored device.
5.Review SNMP access restrictions and credentials.
6.Rotate any community string that may have been exposed and update the corresponding Zabbix configuration.

Final Status
VyOS: SNMP daemon restarted and confirmed active. Confirm resumed Zabbix metric collection if not already shown by the evidence.
pfSense: SNMP re-enabled; Zabbix availability returned to available (1).

Both devices were recovered from their respective controlled failure states.
