INC-003 — Lessons Learned
1. Incident Summary

A controlled failure-injection exercise stopped the SSH daemon on RHEL01 (10.10.20.30). The host remained reachable through ICMP, but SSH connections and Ansible remote execution failed.

The incident was resolved by restarting sshd from the VM console and validating remote access from Zabbix01.

2. Technical Lessons
Lesson 1: Host Reachability Does Not Guarantee Service Availability

Successful ICMP responses confirmed that RHEL01 remained reachable over IP. They did not establish that SSH was available.

Takeaway: Validate the specific service required by the application or management workflow, not just the host's IP address.

Lesson 2: Interpret Connection Errors in Context

The SSH client returned Connection refused when connecting to TCP/22. This was consistent with the SSH service being unavailable during the test.

Takeaway: Correlate the client-side error with the service state, listening sockets, firewall behavior, and other available evidence before drawing a conclusion.

Lesson 3: Ansible Depends on Working Remote Access

Ansible reported UNREACHABLE because it could not establish the SSH connection needed to execute the ping module.

Takeaway: When Ansible reports an unreachable Linux host, distinguish transport/connectivity failures from errors that occur after a successful connection.

Lesson 4: Console Access Provides a Recovery Path

Because SSH was unavailable, the service was restarted through the RHEL01 VM console.

Takeaway: Maintain an alternative administrative access method for recovering systems when remote management fails.

Lesson 5: Validate Recovery at Multiple Levels

After remediation, systemctl is-active sshd returned active, systemctl is-enabled sshd returned enabled, and the Ansible ping returned pong.

Takeaway: Confirm both local service health and the remote management workflow before declaring an incident resolved.

3. Troubleshooting Method

This exercise followed a structured process:

Establish a working baseline.
Introduce a controlled service failure.
Check host-level IP reachability.
Test the affected remote-management service.
Inspect the service state on the affected host.
Restore the service through console access.
Recheck service state and Ansible connectivity.
Document observations, root cause, remediation, and validation.
4. Monitoring Improvements

The exercise highlights the value of monitoring different failure conditions independently:

Host reachability
SSH service availability
Remote-management connectivity
Service state changes and relevant system logs

A host-level ICMP check alone would not detect every SSH availability problem. Any additional monitoring should be configured and tested before being described as implemented.

5. Operational Improvements

For a production environment, consider:

Alerting on unexpected SSH service stops.
Reviewing systemd logs and authentication logs during an outage.
Controlling administrative access to critical services.
Using change management for planned service interruptions.
Maintaining documented recovery procedures.
Recording timestamps and command outputs during incident response.

These are improvement recommendations rather than claims about controls already deployed in this lab.

6. Evidence-Based Diagnosis

The key observations were:

SSH worked before failure injection.
sshd became inactive after it was deliberately stopped.
ICMP connectivity continued to work.
Ansible could not connect to TCP/22.
Restarting sshd restored the service.
Ansible connectivity succeeded after recovery.

Together, these observations support the conclusion that the controlled outage resulted from the stopped SSH service.

7. Final Takeaway

Reliable troubleshooting requires testing the affected service, correlating evidence across layers, and validating recovery from the perspective of the system that depends on it.

This exercise demonstrates a repeatable method for investigating and documenting a Linux remote-access service outage.
