INC-003 — Command Output Evidence
1. Purpose

This document records the commands and observed results from the controlled SSH service failure, investigation, and recovery on RHEL01.

The outputs below are transcribed from the lab session. They are not presented as a single captured terminal log.

2. Environment

|Property	             |             Value             |
|----------------------|-------------------------------|
|Ansible               |     controller	Zabbix01       |
|Ansible workspace	   |     /home/admin1/automation   |
|Inventory host	       |             rhel01            |
|Target hostname	     |         rhel01.corp.lab       | 
|Target IP	           |          10.10.20.30          |
|Service	             |               sshd            |
|Port	                 |             TCP/22            |

3. Baseline

Before the controlled failure, systemctl status sshd --no-pager showed the SSH service as active (running), with listeners on 0.0.0.0:22 and [::]:22.

The initial Ansible ping from Zabbix01 succeeded:

rhel01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

The output also reported the discovered Python interpreter as /usr/bin/python3.9.

4. Failure Injection

Run on the RHEL01 console:

sudo systemctl stop sshd

The command completed without an error being reported.

The subsequent service-state check returned:

inactive
5. Host Connectivity During the Outage

Run on Zabbix01:

ping -c 4 10.10.20.30

Observed result:

4 packets transmitted, 4 received, 0% packet loss

This confirmed that ICMP connectivity remained available during the SSH outage.

6. Ansible Failure During the Outage

Run on Zabbix01:

cd /home/admin1/automation && ./.venv/bin/ansible rhel01 -m ansible.builtin.ping -k

Observed error:

ssh: connect to host 10.10.20.30 port 22: Connection refused
rhel01 | UNREACHABLE!

The failure occurred while Ansible was establishing its SSH connection.

7. Recovery

Run on the RHEL01 console:

sudo systemctl start sshd

Then verify the service state:

systemctl is-active sshd
systemctl is-enabled sshd

Observed output:

active
enabled
8. Post-Recovery Ansible Validation

Run on Zabbix01:

cd /home/admin1/automation && ./.venv/bin/ansible rhel01 -m ansible.builtin.ping -k

Observed result:

rhel01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

The output also included the non-fatal Python interpreter discovery warning and identified /usr/bin/python3.9.

9. Evidence Interpretation

|Check	                     |   Observed result                  |         	Conclusion                    |
|----------------------------|------------------------------------|-----------------------------------------|
|Baseline SSH service	       | Active (running)	                  |   SSH initially available               |
|Baseline Ansible ping	     |     Success                       	|    Remote management worked             | 
|SSH service after stop	     |   Inactive	                        |   Failure injection succeeded           | 
|ICMP during outage	         |   4/4 replies	                    |    Host remained IP-reachable           |
|Ansible during outage	     |    Connection refused; unreachable	|    SSH-based management failed          |
|SSH service after recovery	 | Active and enabled	                |        Service restored                 |
|Ansible after recovery	     |     Success;                       |     pong	Remote management restored    |

10. Evidence Integrity

The observations in this document reflect the results reported during the lab session. They are transcribed evidence, not original timestamped terminal captures.

For stronger evidence, preserve sanitized terminal captures or complete command-output logs in the repository. Remove passwords, tokens, private keys, SNMP community strings, and other sensitive values before publishing.

11. Final Status

The controlled SSH service outage was resolved. Both local service-state checks and remote Ansible validation confirmed recovery.
