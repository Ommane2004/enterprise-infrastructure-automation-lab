INC-005 — Command Output and Validation Evidence
Status

Pending baseline collection.

This document will contain sanitized command outputs collected from the Windows 10 client, VyOS, and the automation/monitoring environment during the investigation.

Evidence Handling
Record the system and context where each command was executed.
Preserve relevant output accurately.
Include timestamps where practical.
Redact passwords, tokens, SNMP community strings, private keys, and other secrets.
Review IP addresses, MAC addresses, hostnames, and infrastructure details before publishing.
Do not include unrelated output or claim that an unexecuted command succeeded.
Capture actual lab output; do not replace it with illustrative or fabricated output.
1. Windows Client Baseline

System: Windows 10 client
Expected lab network: 10.10.10.0/24
Status: Pending collection.

Command:

ipconfig /all

Record the relevant active-adapter fields:

DHCP Enabled
IPv4 Address
Subnet Mask
Default Gateway
DHCP Server
DNS Servers
Lease Obtained
Lease Expires

Do not include the adapter's physical address unless it is necessary for the investigation.

2. VyOS DHCP Configuration

System: VyOS router (core01)
Status: Pending collection.

Collect the relevant DHCP configuration using non-destructive inspection commands appropriate to the installed VyOS version.

Record the applicable subnet, address pool, lease settings, and advertised options. Sanitize any community strings, credentials, or unrelated sensitive configuration.

3. DHCP Service State and Logs

Status: Pending collection.

Identify the DHCP service actually used by this VyOS installation before checking its state or logs. Do not assume a particular DHCP daemon or service name.

Record the service state and relevant log entries, including timestamps where available.

4. Baseline Connectivity

Status: Pending collection.

Record the results of appropriate tests to distinguish DHCP behavior from general network connectivity, including:

Client-to-gateway connectivity.
Client-to-DNS-server connectivity, where appropriate.
DNS resolution for a relevant lab hostname.
Existing lease and addressing details.
5. Controlled Failure

Status: Not performed.

After baseline verification and confirmation of a safe recovery procedure, record:

The controlled change made.
The affected system and service.
The observed client behavior.
DHCP service state and relevant logs.
Connectivity results during the failure.

Do not perform failure injection until the current client configuration and recovery procedure are confirmed.

6. Remediation and Recovery

Status: Pending.

Record the actual remediation, restored service state, client lease status, and post-recovery validation results.

7. Final Evidence Summary

|Check	                                    |     Result      |
|-------------------------------------------|-----------------|
|Client DHCP configuration confirmed	      |     Pending     |
|VyOS DHCP configuration inspected	        |     Pending     |
|DHCP service identified and checked	      |     Pending     |
|Baseline evidence captured	                |   Pending       |
|Controlled failure completed	              |   Pending       |
|Root cause supported by evidence           |  	 Pending      |
|Remediation documented	                    |   Pending       |
|Recovery validated	                        |   Pending       |

Update this table only when each check has been performed and its result documented.

Baseline Evidence — 1 October 2026
Windows 10 Client
Hostname: DESKTOP-JMEKHSC
DHCP Enabled: Yes
IPv4 Address: 10.10.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 10.10.10.1
DHCP Server: 10.10.10.1
DNS Server: 10.10.20.10
Lease Obtained: 1 October 2026, 21:27:30 (Windows local time)
Lease Expires: 2 October 2026, 21:27:29 (Windows local time)
VyOS DHCP Lease

Command: show dhcp server leases

Observed result:

Client address: 10.10.10.10
Lease state: active
Pool: INTERNAL
Hostname: desktop-jmekhsc.corp.lab
Lease start: 2026-10-01 15:44:51 (VyOS-reported time)
Lease expiration: 2026-10-02 15:44:51 (VyOS-reported time)

The client's MAC address has been omitted from this public evidence record.

DHCP Service State

Command: sudo systemctl --type=service --all | grep -Ei 'kea|dhcp'

Observed result:

isc-kea-dhcp4-server.service — loaded, active, running.

Preliminary Assessment

The client is configured for DHCP, the VyOS DHCPv4 service is running, and VyOS reports an active lease for the client. No DHCP failure has been established at baseline.
