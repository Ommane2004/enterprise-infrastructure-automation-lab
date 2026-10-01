INC-005 — DHCP Investigation
Objective

Investigate a planned DHCP failure scenario in the enterprise infrastructure lab. Establish the baseline configuration, identify the source of any address-assignment failure, and collect evidence before applying remediation.

Current status: Baseline validation pending. No failure has been injected or diagnosed yet.

Environment

|Component	         |           Expected role      |
|--------------------|------------------------------|
|VyOS	               |   DHCP server and router     |
|Client network	     |        10.10.10.0/24         |
|Client gateway	     |         10.10.10.1           |
|Windows 10	         |  Client under investigation  |
|Management network	 |        10.10.20.0/24         |

The Windows client's previously recorded IPv4 address is 10.10.10.10. Its current address assignment method must be verified before proceeding.

Investigation Plan
1. Establish the Windows Client Baseline

On the Windows client, run:

ipconfig /all

Record the following fields from the active network adapter:

DHCP Enabled
IPv4 Address
Subnet Mask
Default Gateway
DHCP Server
DNS Servers
Lease Obtained
Lease Expires

If DHCP is disabled for the adapter, do not assume that a DHCP failure has occurred. Determine whether the client is intentionally configured with a static address.

2. Inspect the VyOS DHCP Configuration

Inspect the DHCP configuration on VyOS and record:

DHCP shared networks and subnets
Address ranges or pools
Default-router option
DNS-server option
Lease duration
Relevant service state and logs, where available

Compare the configuration with the intended client network and addressing plan. Do not publish credentials or unrelated configuration details.

3. Verify the Baseline

Before changing anything:

Confirm the intended DHCP pool covers the client's network.
Confirm the advertised gateway matches the intended gateway.
Record the current client address and assignment method.
Determine whether the client currently has a valid lease.
Capture relevant command output and screenshots.
4. Select a Controlled Failure

Only after the baseline is documented, choose a safe failure scenario that matches the actual configuration.

Potential causes to investigate include:

DHCP service unavailable
Incorrect or exhausted address pool
Incorrect DHCP options
Client adapter not configured to use DHCP
Network connectivity issue affecting DHCP traffic

These are investigation possibilities, not confirmed findings.

Perform the test only in the isolated lab, and document the recovery procedure before changing the configuration.

5. Investigate the Symptoms

During the controlled test:

Record the exact change made.
Capture the client's resulting address and DHCP state.
Inspect relevant DHCP service state and logs.
Check network connectivity independently.
Use the evidence to distinguish DHCP failure from routing or DNS problems.
6. Validate Recovery

After remediation, verify that the client receives the intended configuration if DHCP is the expected method.

Check the IPv4 address, subnet mask, default gateway, DNS servers, DHCP server, and lease information. Test gateway reachability and DNS resolution separately.

Evidence Requirements

Capture and sanitize evidence for:

Windows client baseline
VyOS DHCP configuration baseline
Controlled failure and observed symptoms
Relevant diagnostic output
Remediation action
Post-recovery client configuration
Gateway and DNS validation

Use the repository's evidence conventions and update the evidence index when the artifacts exist.

Current Findings

No root cause has been established. The client configuration, VyOS DHCP configuration, and baseline lease state must be inspected before a cause can be determined.

Security Notes

Do not publish passwords, API tokens, private keys, customer data, or unrelated sensitive system information. Review screenshots and command output before committing them.
