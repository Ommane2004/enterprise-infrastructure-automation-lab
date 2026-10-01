INC-004 — SNMP Monitoring Failure: VyOS and pfSense
Incident Summary

|Field	                      |                      Details                       |
|-----------------------------|----------------------------------------------------|
|Incident ID	                |                      INC-004                       |
|Title                        |      SNMP Monitoring Failure — VyOS and pfSense    |
|Category	                    |    Infrastructure Monitoring / Network Operations  |
|Severity	L                   |     ab incident — monitoring visibility degraded   |
|Environment	                |           Isolated enterprise infrastructure lab   |
|Affected devices	            |            VyOS router and pfSense firewall        |
|Monitoring platform          |                     	Zabbix                       |
|Status	                      |   Recovered; evidence and documentation in progress|

Overview

This incident investigated the loss of SNMP monitoring visibility for two network devices: VyOS and pfSense.

Controlled failure injection was performed separately on each device. During both tests, IP connectivity from the Zabbix monitoring server to the affected device remained available, while SNMP monitoring was interrupted.

This distinction helped isolate the issue to the monitoring service rather than general IP reachability.

Environment
Monitoring server: Zabbix01 — 10.10.20.20
VyOS transit interface: 192.168.0.2
pfSense transit interface: 192.168.0.1
Monitoring protocol: SNMP over UDP/161
Monitoring platform: Zabbix
Impact

During each controlled failure:

Zabbix lost SNMP monitoring availability for the affected device.
The device remained reachable through ICMP ping from Zabbix01.
SNMP-based monitoring data could not be collected normally while the respective SNMP service was disabled.

The test was performed in a lab environment and does not represent a production outage.

Investigation and Root Cause
VyOS

The snmpd service was stopped on VyOS as a controlled failure injection. Its service state became inactive. Zabbix could still reach 192.168.0.2 by ping, but SNMP monitoring was interrupted.

The immediate cause was the stopped SNMP daemon.

pfSense

The SNMP daemon was disabled through the pfSense web interface under Services → SNMP. Zabbix subsequently reported the SNMP agent availability item as not available (0).

Ping to 192.168.0.1 continued to succeed during the test.

The immediate cause was the disabled SNMP service.

Recovery
VyOS

The SNMP daemon was restarted with:

sudo systemctl start snmpd

The service was subsequently confirmed to be active. The available evidence should be used to verify the return of SNMP metrics in Zabbix.

pfSense

SNMP was re-enabled through Services → SNMP, and the configuration was saved and applied.

Zabbix subsequently reported SNMP agent availability as available (1).

Validation

The investigation used the following checks:

Confirmed baseline SNMP monitoring in Zabbix.
Checked IP reachability before failure injection.
Interrupted SNMP monitoring on one device at a time.
Rechecked IP reachability during each failure.
Observed the corresponding SNMP monitoring state in Zabbix.
Restored the SNMP service and checked recovery.

See the evidence index for screenshots and supporting validation outputs.

Lessons Learned
IP reachability does not guarantee that an application or monitoring service is available.
Monitoring availability and network connectivity should be tested independently.
Zabbix should alert on monitoring-agent availability so that collection failures are visible.
Failure injection should be controlled, performed on one device at a time, and followed by explicit recovery checks.
Screenshots and command output should demonstrate baseline, failure, connectivity, and recovery without exposing credentials.

Evidence
[Evidence index](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/7532f930a10723dd0a3e6fc63b61ff9c555b7503/evidence/INDEX.md)
[Monitoring evidence](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/7532f930a10723dd0a3e6fc63b61ff9c555b7503/evidence/monitoring)
[Connectivity validation evidence](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/7532f930a10723dd0a3e6fc63b61ff9c555b7503/evidence/validation)

This was a controlled test in an isolated lab. No production systems were involved.

Do not publish SNMP community strings, API tokens, passwords, private keys, or unredacted configuration screenshots. Review all evidence before making the repository public.

Final Status

Recovered in the lab. The pfSense SNMP availability item returned to available (1). The VyOS SNMP daemon was confirmed active after restart; confirm the resumed Zabbix metrics before describing its monitoring recovery as fully validated.
