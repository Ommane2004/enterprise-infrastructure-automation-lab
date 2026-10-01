INC-004 — Investigation Evidence
Objective

Determine why Zabbix lost SNMP monitoring visibility for VyOS and pfSense, and establish whether the loss of monitoring was caused by a general network connectivity failure or an SNMP service failure.

Monitoring Architecture
Zabbix01: 10.10.20.20
VyOS transit address: 192.168.0.2
pfSense transit address: 192.168.0.1
Monitoring protocol: SNMP over UDP/161

The investigation used Zabbix monitoring state, ICMP reachability tests, and service-state checks.

Investigation A — VyOS
Baseline observations
VyOS SNMP metrics were visible in Zabbix.
A ping test from Zabbix01 to 192.168.0.2 returned 4 replies with 0% packet loss.
The VyOS SNMP daemon was confirmed active before the controlled failure.
Failure injection

The SNMP daemon was stopped on VyOS:

sudo systemctl stop snmpd

The service-state check returned inactive.

A subsequent ping test from Zabbix01 to 192.168.0.2 still returned 4 replies with 0% packet loss.

Interpretation

IP connectivity remained available while the SNMP daemon was stopped. This indicates that the observed monitoring interruption was consistent with an SNMP service availability problem rather than a complete loss of IP reachability.

Recovery observation

The daemon was restarted:

sudo systemctl start snmpd

The service was subsequently confirmed active. Check the captured Zabbix recovery evidence to establish whether SNMP metric collection also resumed.

Investigation B — pfSense
Baseline observations
pfSense monitoring data was visible in Zabbix.
A ping test from Zabbix01 to 192.168.0.1 returned 4 replies with 0% packet loss.
The SNMP service was enabled in the pfSense web interface before the test.
Failure injection

The SNMP daemon was disabled through Services → SNMP, and the change was saved/applied.

Zabbix then reported the pfSense SNMP agent availability item as:

not available (0)

A subsequent ping test from Zabbix01 to 192.168.0.1 still returned 4 replies with 0% packet loss.

Interpretation

The device remained reachable over IP while its SNMP monitoring was unavailable. The Zabbix item state provided direct evidence of the monitoring failure.

Recovery observation

SNMP was re-enabled through Services → SNMP, and the configuration was saved/applied.

Zabbix subsequently reported the SNMP agent availability item as:

available (1)

This confirms recovery of the pfSense SNMP availability check.

Comparative Findings

|Check                                       |                    	VyOS	                           |                      pfSense                 |
|--------------------------------------------|-------------------------------------------------------|----------------------------------------------|
|Baseline monitoring	                       |                SNMP metrics visible            	     |               Monitoring data visible        |
|Controlled failure	                         |               Stopped snmpd	                         |         Disabled SNMP through web UI         | 
|Service/monitoring state during failure     |              	inactive on device	                   |              not available (0) in Zabbix     |
|IP connectivity during failure              |               	4/4 replies; 0% loss	                 |               4/4 replies; 0% loss           |
|Recovery action	                           |                  Started snmpd	                       |                    Re-enabled SNMP           |
|Recovery confirmed	                         |   Service active; verify Zabbix metric recovery	     |           Zabbix returned available (1)      |
|Evidence to Review

screenshots

[evidence/monitoring/](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/236d0606d762dea092b27556401ba78c5fba8657/evidence/monitoring).
[evidence/validation/](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/236d0606d762dea092b27556401ba78c5fba8657/evidence/validation).

See the evidence index for the incident-specific evidence links.

Conclusion

Both controlled tests showed that successful ICMP reachability did not guarantee SNMP monitoring availability.

The investigation isolated the immediate failure to the SNMP service being stopped or disabled. Recovery must be checked at both levels: service state on the device and monitoring state in Zabbix.

Security Note

Do not include SNMP community strings, passwords, API tokens, or unredacted configuration screenshots in public evidence. Review screenshots before publishing them.
