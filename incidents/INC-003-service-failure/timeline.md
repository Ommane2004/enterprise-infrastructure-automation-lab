INC-003 — SSH Service Failure Timeline
Incident Summary
Incident ID: INC-003
Affected host: RHEL01 (rhel01.corp.lab)
Host IP: 10.10.20.30
Affected service: SSH (sshd)
Service port: TCP/22
Status: Resolved
Timeline
1. Baseline Verification

The SSH service was initially running on RHEL01.

systemctl status sshd --no-pager showed the service as active (running).
SSH was listening on TCP port 22.
Ansible connectivity was verified from Zabbix01 using the ansible.builtin.ping module.
The Ansible test returned SUCCESS with ping: pong.
2. Controlled Failure Injection

The SSH service was deliberately stopped on RHEL01 to simulate a service-availability incident.

sudo systemctl stop sshd

The service state was subsequently verified as inactive.

3. Initial Investigation

From the Ansible controller, Zabbix01:

ICMP connectivity to 10.10.20.30 remained successful: 4 packets transmitted, 4 received, 0% packet loss.
The Ansible connectivity test failed with Connection refused when attempting to connect to TCP/22.
Ansible reported the host as UNREACHABLE.

These results indicated that the host remained reachable at the IP layer, while SSH-based remote management was unavailable.

4. Service Recovery

On the RHEL01 console, the SSH service was started again.

sudo systemctl start sshd

The service was then verified:

systemctl is-active sshd
systemctl is-enabled sshd

The outputs were:

active
enabled
5. Post-Recovery Validation

From Zabbix01, the Ansible connectivity test was repeated.

Result: SUCCESS
Ansible ping: pong
Changed: false

This confirmed that SSH-based Ansible management had been restored.

Final Status

Resolved. The SSH service was restarted, its active and enabled states were verified, and Ansible connectivity was successfully retested.

Evidence and Limitations

The incident was a controlled lab exercise. The observed failure and recovery establish that stopping sshd caused the SSH connection failure in this test. They do not establish the cause of an unrelated or naturally occurring SSH outage.
