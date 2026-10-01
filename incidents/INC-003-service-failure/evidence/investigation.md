INC-003 — Investigation Evidence
1. Objective

Investigate a controlled SSH service outage on RHEL01 and determine why remote Ansible management failed while IP connectivity remained available.

2. Affected System

|Property	                |       Value         |
|-------------------------|---------------------|
|Hostname	                |  rhel01.corp.lab    |
|Ansible inventory alias	|      rhel01         |
|IP address	              |    10.10.20.30      |
|Operating system	        |     RHEL 9.8        |
|SSH service	            |       sshd          |
|SSH port	                |      TCP/22         |
|Ansible controller       |   	Zabbix01        |

4. Baseline Evidence

Before failure injection, the following checks succeeded:

systemctl status sshd --no-pager showed SSH as active (running).
SSH was listening on TCP port 22.
Ansible connected to rhel01 using ansible.builtin.ping.
The result was SUCCESS, with ping: pong.

This established that SSH-based Ansible connectivity worked before the test.

4. Failure Evidence

The SSH service was deliberately stopped on RHEL01:

sudo systemctl stop sshd

The service state was verified as inactive.

From Zabbix01, ICMP testing to 10.10.20.30 succeeded:

Packets transmitted: 4
Packets received: 4
Packet loss: 0%

However, the Ansible test failed with:

ssh: connect to host 10.10.20.30 port 22: Connection refused

Ansible reported the host as UNREACHABLE.

5. Evidence Analysis
|Test	                 |  Observation	        |Interpretation                         |
|----------------------|----------------------|---------------------------------------|
|SSH service state	   |   inactive	          |SSH service was stopped                |
|ICMP connectivity	   |   4/4 replies       	|Host remained reachable over IP        |
|TCP/22 connection	   |   Connection refused	|SSH connection could not be established|
|Ansible connectivity	 |  UNREACHABLE	        |Remote management failed over SSH      |

The combination of successful ICMP responses and a refused SSH connection was consistent with a service-availability failure rather than a complete loss of host connectivity.

6. Root-Cause Finding

The directly observed cause of the simulated outage was that the sshd service had been stopped on RHEL01. This made SSH-based Ansible management unavailable.

The test does not establish a cause for any separate, naturally occurring SSH incident.

7. Recovery Evidence

The SSH service was restarted on RHEL01:

sudo systemctl start sshd

The subsequent checks returned:

active
enabled

Ansible connectivity was then retested from Zabbix01. The result was SUCCESS, with ping: pong and changed: false.

8. Conclusion

The investigation demonstrated that a Linux host can remain reachable by ICMP while SSH-based administration is unavailable.

The service state, network test, SSH error, and Ansible result provided complementary evidence. After restarting SSH, service-state verification and the successful Ansible test confirmed recovery.
