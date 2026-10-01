INC-004 — Lessons Learned
1. Monitoring Availability Is Different from Network Reachability

The most important finding was that both VyOS and pfSense remained reachable through ICMP ping while their SNMP monitoring was interrupted.

A device can respond to ping while a particular service is unavailable. Therefore, ping alone is not sufficient to establish that a device is fully operational.

2. Monitor Multiple Availability Signals

Infrastructure monitoring should distinguish between:

ICMP reachability: Can the monitoring server reach the device over IP?
SNMP availability: Can the monitoring system collect information through SNMP?
Metric freshness: Are the expected SNMP items receiving current data?
Service state: Is the relevant service running on the device?

These signals provide different information and should be interpreted together.

3. Establish a Baseline Before Troubleshooting

Before making changes, record the expected state:

Device reachability
SNMP availability in Zabbix
Relevant item values and last-check times
Service state, where accessible

A baseline makes it easier to identify what changed during an incident.

4. Change One Variable at a Time

The VyOS and pfSense tests were performed separately. This made it possible to associate each monitoring interruption with the service change on the corresponding device.

For future tests, record the exact action, its result, and the recovery step before proceeding to another component.

5. Validate Recovery at Both Ends

Restoring a service on a device is not the same as confirming end-to-end monitoring recovery.

For VyOS, the daemon was confirmed active after restart. The corresponding Zabbix metrics should also be checked for fresh values.

For pfSense, Zabbix reported available (1) after SNMP was re-enabled.

6. Protect Monitoring Credentials

SNMP community strings function as shared access credentials for SNMPv1/v2c. They must not be exposed in public screenshots, configuration files, or command output.

Use appropriate access restrictions, review screenshots before publishing, and rotate any credential that may have been exposed.

7. Improve Alerting and Incident Response

Useful monitoring improvements include:

Alerting when SNMP agent availability becomes unavailable.
Monitoring IP reachability independently.
Identifying stale or missing metric updates.
Recording the first observed failure and recovery times.
Documenting service changes and the validation results.
Avoiding unnecessary service restarts before collecting diagnostic evidence.
8. Evidence-Based Root Cause Analysis

The tests support a specific conclusion: the controlled monitoring interruptions occurred because SNMP was stopped or disabled.

They do not demonstrate an unexpected production failure or an underlying product defect.

Incident reports should distinguish directly observed facts from assumptions and should not claim more than the evidence establishes.

9. Reusable Troubleshooting Workflow

Use this workflow for similar monitoring incidents:

1.Confirm the alert and affected host.
2.Check whether IP reachability is available.
3.Check SNMP availability and metric freshness.
4.Inspect service state and configuration.
5.Identify the specific failure mechanism.
6.Apply the smallest appropriate recovery action.
7.Validate service state and monitoring recovery independently.
8.Capture sanitized evidence.
9.Record root cause, remediation, and preventive improvements.
10.Conclusion

INC-004 reinforced a core infrastructure operations principle: reachability, service availability, and monitoring-data freshness are separate conditions.

Reliable troubleshooting requires independent checks, controlled changes, explicit recovery validation, and evidence that supports each conclusion.
