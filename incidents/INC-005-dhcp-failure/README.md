INC-005 — DHCP Failure and Client Address Assignment
**Incident Summary**

|Field                   |                         	Details                                 |
|------------------------|------------------------------------------------------------------|
|Incident ID	           |                           INC-005                                |
|Title	                 |    DHCP Failure and Client Address Assignment Troubleshooting    |
|Category	               |          Network Services / Infrastructure Operations            |
|Environment             |           	Isolated enterprise infrastructure lab                |
|DHCP server	           |                             VyOS                                 |
|Affected network	       |                Client network — 10.10.10.0/24                    |
|Client	                 |                        Windows 10                                |
|Status	                 |         Planned — baseline validation not yet completed          |

Overview

This incident will investigate a controlled DHCP failure scenario in the enterprise infrastructure lab.

The objective is to understand how a client obtains its IPv4 configuration, identify why DHCP address assignment may fail, collect diagnostic evidence, and validate recovery.

The investigation will distinguish DHCP-related problems from general network connectivity failures.

Environment
DHCP server: VyOS router
Client network: 10.10.10.0/24
Client default gateway: 10.10.10.1
Windows client: 10.10.10.10 (previously recorded configuration; current assignment method must be verified)
Management network: 10.10.20.0/24

The Windows client's current DHCP setting has not yet been verified. Do not assume its address is dynamically assigned until the client configuration is inspected.

Objectives
Record the baseline DHCP configuration on VyOS.
Determine whether the Windows client uses DHCP or a static IPv4 configuration.
Inspect the client's IPv4 address, gateway, DNS, DHCP server, and lease information.
Identify the cause of the controlled DHCP failure.
Restore DHCP service or correct the identified configuration issue.
Validate that the client receives the expected network configuration.
Document the investigation, root cause, remediation, and lessons learned.
Planned Investigation

The investigation will begin with baseline checks on the Windows client and VyOS. A controlled failure will be introduced only after the baseline is documented and a safe test method is selected.

No DHCP service has been disabled as part of this planned incident at the time of writing.

Evidence Plan

Evidence will be collected for:

Baseline Windows IPv4 and DHCP configuration
VyOS DHCP configuration and operational state
Failure symptoms and relevant diagnostic output
Controlled failure action
Recovery procedure
Post-recovery DHCP lease and connectivity validation

Screenshots and command output must be reviewed before publication. Do not include passwords, tokens, private keys, or unrelated sensitive system information.

Success Criteria

The incident will be considered technically resolved when:

The DHCP issue has an evidence-supported root cause.
The appropriate remediation has been applied.
The client obtains the expected IPv4 configuration, if DHCP is the intended configuration method.
Gateway and required DNS connectivity are validated.
The final state and lessons learned are documented.
Related Evidence

Evidence links will be added after the baseline checks and evidence files have been created.

Current Status

Planned — baseline validation pending. No failure injection or recovery result is claimed yet.
