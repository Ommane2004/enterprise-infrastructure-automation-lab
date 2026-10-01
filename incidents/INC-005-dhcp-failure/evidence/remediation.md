INC-005 — Remediation
Status

Pending investigation.

The DHCP baseline and controlled failure have not yet been verified. Remediation must be selected only after the root cause is supported by evidence.

Remediation Objective

Restore reliable DHCP address assignment for the Windows 10 client on the 10.10.10.0/24 client network, if DHCP is confirmed to be the intended addressing method.

The remediation must preserve the intended network configuration and avoid unnecessary changes to unrelated lab systems.

Preconditions

Before making changes:

Capture the Windows client's current network configuration.
Confirm whether the client adapter is configured to obtain an IP address automatically.
Record the relevant VyOS DHCP configuration and service state.
Identify the DHCP server, applicable subnet, address pool, gateway, and DNS options.
Capture baseline connectivity and monitoring evidence.
Confirm the recovery procedure before introducing a controlled failure.

Remediation Plan

The specific action depends on the evidence gathered.

|Potential                                                    |              finding	Planned response                                                       |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------|
|DHCP service is stopped or unavailable	                      |    Restore the identified DHCP service and verify its state.                                 |
|DHCP pool or subnet configuration is incorrect	              |   Correct the confirmed configuration error and validate the applicable pool.                |
|DHCP options are incorrect	                                  |   Correct the affected gateway, DNS, or other advertised options.                            |
|Client is configured with a static address unintentionally	  |   Correct the client configuration only after confirming DHCP is intended.                   |
|DHCP requests cannot reach the server	                      |     Investigate the relevant network path, interfaces, and filtering rules.                  |
|Address pool is exhausted                                    |   	Verify lease usage and available addresses before making a controlled pool adjustment.   |

These are investigation hypotheses, not confirmed findings.

Changes Performed

Pending investigation.

Record each actual change here after remediation. Include the affected system, the change made, the reason, and the validation result. Do not record planned actions as completed actions.

Recovery Validation

After remediation, verify and document:

The intended DHCP service is running and available.
The client obtains a valid lease if DHCP is intended.
The assigned IP address and subnet mask are correct.
The default gateway is correct.
The DNS server is correct.
Connectivity to the gateway and required DNS services works.
Name resolution succeeds where applicable.
Monitoring and relevant logs show the expected recovery.

A successful ping alone does not prove that DHCP is working.

Rollback Plan

If a change causes unexpected impact:

Stop further changes.
Restore the last known-good DHCP configuration or service state.
Restore the client's previous addressing configuration if required.
Revalidate connectivity and DHCP behavior.
Document the rollback and any remaining issues.
Final Outcome

Not yet established. Update this section after the controlled investigation, remediation, and recovery validation are complete.
