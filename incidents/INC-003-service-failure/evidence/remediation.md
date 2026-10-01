INC-003 — Remediation and Recovery
1. Objective

Restore SSH-based remote administration on RHEL01 after a controlled test stopped the SSH daemon.

2. Affected System
Hostname: rhel01.corp.lab
IP address: 10.10.20.30
Service: sshd
Protocol and port: SSH over TCP/22
Ansible controller: Zabbix01
3. Recovery Procedure
Step 1: Access the RHEL01 console

Because SSH was unavailable, use the existing RHEL01 VM console rather than relying on remote SSH access.

Step 2: Start the SSH service

Run the following command on RHEL01:

sudo systemctl start sshd
Step 3: Verify service state

Run:

systemctl is-active sshd
systemctl is-enabled sshd

Observed results:

active
enabled

The service was running and configured to start automatically at boot.

Step 4: Validate Ansible connectivity

From the Zabbix01 controller, run:

cd /home/admin1/automation && ./.venv/bin/ansible rhel01 -m ansible.builtin.ping -k

Enter the SSH password when prompted.

The test returned SUCCESS, with ping: pong and changed: false. This confirmed that Ansible could manage RHEL01 over SSH again.

4. Recovery Validation

|Validation                 	| Result|
|-----------------------------|-------|
|SSH service active	          | Pass  |
|SSH service enabled	        | Pass  |
|Ansible connection restored	| Pass  |
|Ansible ping module        	| pong  |
|Ansible reported changes    	|  No   |

5. Corrective Action

The immediate corrective action was to restart the stopped SSH service from the RHEL01 console.

No SSH configuration changes were required for this controlled exercise.

6. Preventive Recommendations

For a production environment, consider:

Monitoring SSH service availability independently from ICMP reachability.
Alerting when sshd stops unexpectedly.
Reviewing systemd and authentication logs when an unplanned outage occurs.
Restricting who can stop critical remote-access services.
Keeping console or out-of-band recovery access available.
Revalidating service health and remote management after maintenance.

These are recommendations, not claims that the controls have already been implemented.

7. Final Status

Resolved and validated. The SSH service is active and enabled, and the Ansible controller successfully re-established remote connectivity.

The remediation applies to the controlled lab incident documented in INC-003.
