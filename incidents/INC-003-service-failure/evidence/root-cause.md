INC-003 — Root Cause Analysis

1. Incident Details

|Field	           |       Details                            |
|------------------|------------------------------------------|
|Incident ID	     |       INC-003                            | 
|Affected host     | 	RHEL01 (rhel01.corp.lab)                |
|IP address	       |    10.10.20.30                           |
|Affected service	 | OpenSSH daemon (sshd)                    |
|Service port      |     	TCP/22                              |
|Incident type	   |   SSH service availability failure       |
|Status	           |   Resolved                               |

3. Root Cause

The direct cause of the simulated incident was the intentional stopping of the sshd service on RHEL01 during a controlled failure-injection exercise.

With the SSH service stopped, the Ansible controller could no longer establish an SSH connection to the host.

3. Supporting Evidence

The following observations support this finding:

Before failure injection, sshd was active and Ansible connectivity succeeded.
After sudo systemctl stop sshd, the service state was inactive.
ICMP connectivity to 10.10.20.30 continued to succeed, with 4 packets received and 0% packet loss.
Ansible reported Connection refused when connecting to TCP/22 and marked the host UNREACHABLE.
After sudo systemctl start sshd, the service state returned to active.
The SSH service was also verified as enabled.
The subsequent Ansible test returned SUCCESS with ping: pong.
4. Failure Mechanism

The host's IP connectivity remained available, but its SSH service was unavailable.

This prevented Ansible from using SSH to execute the requested module on RHEL01. The failure was therefore at the remote-management service layer, rather than a complete host or IP-connectivity failure.

5. Contributing Factors and Scope

The exercise used a deliberate service stop to reproduce the failure. There is no evidence from this test that an unexpected service crash, firewall change, authentication failure, routing issue, or configuration error caused the outage.

Those possibilities should be investigated separately in a naturally occurring incident if the available evidence indicates they are relevant.

6. Corrective Action

The sshd service was restarted on RHEL01 using:

sudo systemctl start sshd

The service state and boot configuration were checked:

systemctl is-active sshd
systemctl is-enabled sshd

The outputs were active and enabled.

Ansible connectivity was then retested successfully from Zabbix01.

7. Preventive and Detective Improvements

Potential improvements for a production environment include:

Monitor SSH service availability and host reachability separately.
Alert when the SSH service becomes unavailable on managed Linux servers.
Investigate service-stop events and unexpected SSH daemon failures.
Use change control for planned service interruptions.
Validate both service state and remote-management connectivity after maintenance.
Maintain console or out-of-band access for recovery when remote administration is unavailable.

These are recommended improvements; they should not be represented as implemented unless separately configured and validated.

8. Final Assessment

Root cause confirmed for the controlled exercise: sshd was stopped on RHEL01, making SSH-based Ansible management unavailable.

Recovery confirmed: SSH returned to the active state, remained enabled, and Ansible connectivity succeeded.

The finding is limited to this lab exercise and does not establish the cause of any unrelated SSH outage.
