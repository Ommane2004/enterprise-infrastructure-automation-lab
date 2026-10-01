INC-004 — Incident Timeline
Objective

Record the sequence of checks, controlled SNMP failures, connectivity tests, and recovery actions performed on VyOS and pfSense.

Exact timestamps are omitted because they were not recorded in the available incident notes.

Phase 1 — Baseline
VyOS
Confirmed that VyOS SNMP metrics were visible in Zabbix.
From Zabbix01, pinged 192.168.0.2.
Result: 4 packets received, 0% packet loss.
Confirmed that the VyOS snmpd service was active before failure injection.
pfSense
Confirmed that pfSense monitoring data was visible in Zabbix.
From Zabbix01, pinged 192.168.0.1.
Result: 4 packets received, 0% packet loss.
Confirmed the SNMP configuration through the pfSense web interface.
Phase 2 — VyOS SNMP Failure
Stopped the SNMP daemon on VyOS using sudo systemctl stop snmpd.
Checked the service state; it returned inactive.
From Zabbix01, pinged 192.168.0.2 again.
Result: 4 packets received, 0% packet loss.
This demonstrated that IP reachability remained available while the SNMP service was stopped.
Phase 3 — VyOS Recovery
Restarted the SNMP daemon using sudo systemctl start snmpd.
Checked the service state; it returned active.
The service-level recovery was confirmed.
Verify the corresponding Zabbix metrics against the captured recovery evidence before claiming that monitoring recovery was fully validated.
Phase 4 — pfSense SNMP Failure
Opened Services → SNMP in the pfSense web interface.
Disabled the SNMP daemon and saved/applied the change.
In Zabbix, the SNMP agent availability item for pfSense reported not available (0).
From Zabbix01, pinged 192.168.0.1.
Result: 4 packets received, 0% packet loss.
This demonstrated that IP reachability remained available while SNMP monitoring was interrupted.
Phase 5 — pfSense Recovery
Re-enabled SNMP in Services → SNMP.
Saved and applied the configuration.
Confirmed that the Zabbix SNMP agent availability item returned to available (1).
Outcome

The controlled tests demonstrated that stopping or disabling SNMP can interrupt monitoring without interrupting ICMP reachability to the device.

The pfSense monitoring recovery was confirmed in Zabbix. The VyOS daemon was confirmed active after restart; its Zabbix metric recovery should be confirmed separately if it is not already shown clearly in the captured evidence.

Related Evidence

See the [evidence index](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/c87d3cc55a69a4b207f475add8f0dabaf229bb2c/evidence/INDEX.md) for the screenshots documenting baseline state, failure state, connectivity checks, and recovery.
