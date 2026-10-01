INC-003 — SSH Service Failure
Incident Overview

|Field	           |    Details                                |
|------------------|-------------------------------------------|
|Incident ID       |	INC-003                                  |
|Incident type	   |   Linux SSH service availability failure  |
|Severity	         | Lab exercise; no production impact        |
|Status	           | Resolved                                  |
|Affected host     | 	RHEL01 (rhel01.corp.lab)                 |
|IP address	       | 10.10.20.30                               |
|Affected service	 | OpenSSH daemon (sshd)                     |
|Service port	     | TCP/22                                    |
|Ansible controller|	Zabbix01                                 |

1. Objective

Simulate and investigate an SSH service outage on a Linux server. Determine why the host remained reachable over IP while Ansible remote management became unavailable, then restore and validate the service.

2. Scenario

The SSH daemon on RHEL01 was deliberately stopped during a controlled failure-injection exercise.

After the service was stopped:

The SSH service state became inactive.
ICMP connectivity to 10.10.20.30 remained successful.
SSH connections to TCP/22 returned Connection refused.
Ansible reported the host as UNREACHABLE.

This reproduced a remote-management availability failure without completely losing IP connectivity to the server.

3. Investigation and Root Cause

The investigation correlated the local service state, ICMP connectivity, the SSH connection error, and the Ansible result.

The direct cause of this simulated incident was the deliberate stopping of sshd on RHEL01. The findings apply to this controlled exercise and do not establish the cause of any unrelated SSH outage.

See Investigation Evidence and Root Cause Analysis.

4. Remediation

The SSH service was restarted from the RHEL01 VM console because remote SSH access was unavailable.

sudo systemctl start sshd

The service state was checked using:

systemctl is-active sshd
systemctl is-enabled sshd

Both checks returned the expected states: active and enabled.

See Remediation and Recovery.

5. Recovery Validation

From Zabbix01, Ansible connectivity was tested again:

cd /home/admin1/automation && ./.venv/bin/ansible rhel01 -m ansible.builtin.ping -k

The command returned SUCCESS, with ping: pong and changed: false.

The results confirmed that SSH-based Ansible management had been restored.

6. Evidence and Documentation
[Incident Timeline](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/76bf546602e4c4aeec0f88b9d957068505457c02/incidents/INC-003-service-failure/timeline.md)
[Investigation Evidence](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/76bf546602e4c4aeec0f88b9d957068505457c02/incidents/INC-003-service-failure/evidence/investigation.md)
[Root Cause Analysis](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/76bf546602e4c4aeec0f88b9d957068505457c02/incidents/INC-003-service-failure/evidence/root-cause.md)
[Remediation and Recovery](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/76bf546602e4c4aeec0f88b9d957068505457c02/incidents/INC-003-service-failure/evidence/remediation.md)
[Lessons Learned](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/76bf546602e4c4aeec0f88b9d957068505457c02/incidents/INC-003-service-failure/evidence/lessons-learned.md)
Command Output Evidence
7. Lessons Learned
Successful ICMP connectivity does not guarantee that a specific service is available.
Diagnose the affected service rather than relying solely on host reachability.
Correlate service state, connection errors, and automation results before determining root cause.
Maintain console or out-of-band access for recovery when remote administration fails.
Validate recovery from both the affected host and the remote management controller.

See Lessons Learned.

8. Final Status

Resolved and validated.

The SSH service was restarted, its active and enabled states were confirmed, and Ansible connectivity was successfully retested.

This was a controlled lab incident with no production impact.
