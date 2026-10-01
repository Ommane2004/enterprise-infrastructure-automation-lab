INC-004 — Command Output and Technical Observations
Purpose

Record the relevant commands and observed results from the controlled SNMP monitoring tests on VyOS and pfSense.

The outputs below are summarized from the recorded test results. They are not represented as verbatim terminal transcripts.

1. VyOS — Baseline Connectivity

Run on Zabbix01:

ping -c 4 192.168.0.2

Observed result:

4 packets received
0% packet loss

This established that the VyOS transit address was reachable before the SNMP failure test.

2. VyOS — SNMP Service Baseline

Run on VyOS:

sudo systemctl status snmpd --no-pager

Observed result before failure injection: The SNMP daemon was active and running.

3. VyOS — Controlled SNMP Failure

Run on VyOS:

sudo systemctl stop snmpd
sudo systemctl is-active snmpd

Observed result:

inactive

The daemon was stopped intentionally for the controlled test.

4. VyOS — Connectivity During Failure

Run on Zabbix01:

ping -c 4 192.168.0.2

Observed result:

4 packets received
0% packet loss

IP connectivity remained available while the SNMP daemon was stopped.

5. VyOS — Recovery

Run on VyOS:

sudo systemctl start snmpd
sudo systemctl is-active snmpd

Observed result:

active

The daemon returned to the active state. Check the corresponding Zabbix metrics separately to confirm end-to-end monitoring recovery.

6. pfSense — Baseline Connectivity

Run on Zabbix01:

ping -c 4 192.168.0.1

Observed result:

4 packets received
0% packet loss

This established that the pfSense transit address was reachable before failure injection.

7. pfSense — Controlled SNMP Failure

Action performed in the web interface:

Opened Services → SNMP.
Disabled the SNMP daemon.
Saved and applied the change.

Observed result in Zabbix:

SNMP agent availability: not available (0)

This provided direct evidence that the SNMP availability check was unavailable during the test.

8. pfSense — Connectivity During Failure

Run on Zabbix01:

ping -c 4 192.168.0.1

Observed result:

4 packets received
0% packet loss

IP connectivity remained available while SNMP monitoring was interrupted.

9. pfSense — Recovery

Action performed in the web interface:

Opened Services → SNMP.
Re-enabled the SNMP daemon.
Saved and applied the change.

Observed result in Zabbix:

SNMP agent availability: available (1)

This confirmed recovery of the pfSense SNMP availability check.

10. Interpretation

The observations support the following conclusions:

Both devices remained reachable by ICMP during their respective SNMP failures.
The VyOS SNMP daemon was confirmed inactive during failure and active after restart.
Zabbix reported pfSense SNMP availability as unavailable during failure and available after recovery.
Successful ping alone was insufficient to establish SNMP service availability.
Security Notes
Do not publish SNMP community strings.
Do not include API tokens, passwords, private keys, or other secrets.
Review command transcripts and screenshots for sensitive configuration details before committing them.
These are summarized observations; consult the corresponding screenshots for the captured visual evidence.

Related Evidence

See the[ evidence index](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/c87d3cc55a69a4b207f475add8f0dabaf229bb2c/evidence/INDEX.md) and the screenshots under [evidence/monitoring/](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/3e7814b378868f30645fcd47657c2ae20aeb6a88/evidence/monitoring) and [evidence/validation/](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/3e7814b378868f30645fcd47657c2ae20aeb6a88/evidence/validation).
Related Evidence

See the evidence index and the screenshots under evidence/monitoring/ and evidence/validation/.
