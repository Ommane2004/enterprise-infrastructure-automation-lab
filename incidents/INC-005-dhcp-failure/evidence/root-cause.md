INC-005 — Root Cause Analysis
Status

Pending investigation.

The DHCP baseline has not yet been verified, and no controlled failure has been performed. A root cause cannot be assigned until the lab observations support it.

Incident Scope

The planned investigation concerns DHCP address assignment for the Windows 10 client on the 10.10.10.0/24 client network, with VyOS providing DHCP services.

The client's current address assignment method must be confirmed before proceeding.

Evidence Required

Before assigning a root cause, collect:

Windows ipconfig /all output for the active adapter.
Confirmation of whether DHCP is enabled on the client.
VyOS DHCP configuration, including the relevant subnet, pool, and advertised options.
DHCP service state and relevant logs.
Client symptoms during the controlled failure.
Connectivity tests that distinguish DHCP problems from general network problems.
Post-remediation validation results.

Root-Cause Assessment

The following are possible investigation paths, not confirmed causes:

Possible cause                                               	Evidence required
DHCP service unavailable	                            Service state and relevant logs
Incorrect DHCP pool or subnet	             VyOS DHCP configuration compared with the intended addressing plan
Incorrect DHCP options	                  Advertised gateway and DNS settings compared with the intended configuration
Client not configured for DHCP                      	Client adapter configuration
Network path issue	                      Relevant connectivity checks and, if needed, DHCP traffic analysis
Address pool exhaustion                 	Pool configuration, lease state, and available address capacity

Confirmed Root Cause

Not established. This section should be updated only after the controlled test has been completed and the evidence identifies the failure mechanism.

Contributing Factors

Pending investigation. Do not infer contributing factors without supporting observations.

Remediation

Pending root-cause confirmation. The remediation must address the cause established by the evidence rather than an assumed DHCP problem.

Recovery Validation

After remediation, verify the intended client configuration and confirm that the client obtains a valid DHCP lease if DHCP is the intended configuration method.

Also test gateway reachability and DNS resolution independently.

Conclusion

This document will be updated after the baseline, failure injection, and recovery checks have been completed. The final root-cause statement must distinguish observed facts from hypotheses.
