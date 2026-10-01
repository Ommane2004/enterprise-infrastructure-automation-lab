INC-005 — DHCP Incident Timeline
Objective

Track the baseline investigation, controlled DHCP failure test, diagnosis, remediation, and recovery validation.

Current status: Planned — baseline validation pending.

No failure injection or recovery result is claimed in this timeline yet.

Phase 1 — Baseline Validation (Pending)

Planned checks:

Inspect the Windows client's IPv4 configuration using ipconfig /all.
Determine whether the client uses DHCP or a static IPv4 configuration.
Record the IPv4 address, subnet mask, default gateway, DHCP server, DNS servers, and lease information, where applicable.
Inspect the relevant DHCP configuration on VyOS.
Confirm the intended address pool and gateway settings before testing.
Phase 2 — Failure Scenario Selection (Pending)

After the baseline is recorded:

Select a controlled failure that is appropriate for the actual DHCP configuration.
Confirm that the test affects only the intended lab network.
Record the expected symptoms and recovery procedure before making changes.

Do not disable DHCP or modify address pools until the baseline and recovery method are understood.

Phase 3 — Controlled Failure and Investigation (Pending)

Planned activities:

Introduce the selected failure in the isolated lab.
Record the client's symptoms.
Inspect the DHCP service and relevant configuration.
Distinguish a DHCP assignment failure from unrelated routing, DNS, or general connectivity issues.
Capture sanitized command output and screenshots.
Phase 4 — Remediation (Pending)

Planned activities:

Restore the correct DHCP service or configuration.
Confirm that the intended DHCP pool and gateway settings are available.
Avoid unrelated configuration changes.
Phase 5 — Recovery Validation (Pending)

Planned checks:

Renew the client's DHCP lease if the client is intended to use DHCP.
Confirm the expected IPv4 address, subnet mask, gateway, DNS, DHCP server, and lease details.
Test gateway reachability.
Test DNS resolution separately.
Record the final results and any remaining limitations.
Phase 6 — Documentation and Closure (Pending)

After the tests are complete:

Document the observed root cause.
Record the remediation and validation results.
Add the supporting evidence links.
Update the incident and evidence indexes.
Review all public artifacts for sensitive information.
Closure Criteria

INC-005 can be marked resolved after the failure has an evidence-supported cause, remediation has been applied, and the intended client configuration has been validated.
