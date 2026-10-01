INC-005 — Lessons Learned
Status

Pending investigation and recovery validation.

Lessons will be finalized after the DHCP baseline, controlled failure, remediation, and recovery have been documented.

Incident Summary

The planned incident investigates DHCP address assignment for the Windows 10 client on the 10.10.10.0/24 client network, where VyOS is intended to provide DHCP services.

The client's actual addressing configuration must be verified before the incident is triggered.

Technical Lessons
1. Verify the Baseline First

Capture the client's current IP configuration, DHCP setting, default gateway, DNS server, and lease information before changing the environment.

2. Distinguish DHCP Failure from Connectivity Failure

A client may retain an existing lease while the DHCP service is unavailable. Successful ping tests therefore do not establish that DHCP requests and responses are working.

3. Verify Both Client and Server

Inspect the client's adapter configuration alongside the DHCP server's configuration, service state, address pool, and logs.

4. Validate DHCP Options

An assigned IP address does not guarantee a usable network configuration. Verify the subnet mask, default gateway, and DNS server separately.

5. Make Controlled Changes

Introduce only the planned failure, record the affected component, and keep a clear recovery procedure available.

6. Prove Recovery

After remediation, verify that the client receives the intended lease and that required network and DNS functions work. Capture the results rather than relying on assumptions.

Evidence-Based Findings

Record confirmed findings after the investigation:

Initial client configuration: Pending.
DHCP server and service: Pending.
Failure observed: Pending.
Confirmed root cause: Pending.
Remediation performed: Pending.
Recovery validation: Pending.
Process Improvements

After the investigation, identify any improvements needed in:

DHCP configuration documentation.
Address-pool and lease monitoring.
DHCP service availability monitoring.
Troubleshooting procedures.
Evidence capture and incident timelines.
Recovery and rollback procedures.

Only record improvements supported by the investigation.

Final Lessons

Pending. Update this section after reviewing the actual incident evidence and recovery results.
