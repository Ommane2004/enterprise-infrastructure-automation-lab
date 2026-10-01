# Validation Evidence

This directory contains evidence demonstrating that implemented
infrastructure behaves according to the documented design.

Validation is performed after configuration or automation changes
to confirm that the expected operational state has been achieved.

---

## Validation Model

The laboratory follows:

```text
Configuration
     ↓
Execution
     ↓
Validation
     ↓
Observed Result
     ↓
Documentation

```
## Related Incident

### INC-001 — VyOS `eth1` Interface Failure

INC-001 provides a controlled validation scenario for the VyOS Internal LAN.

The incident demonstrated that disabling `eth1` causes the directly connected `10.10.10.0/24` route to disappear.

Recovery was validated through:

- `eth1` returning to `UP, LOWER_UP`
- `10.10.10.1/24` remaining configured
- `10.10.10.0/24` returning as directly connected through `eth1`
- Removal of the temporary `eth1 disable` configuration

The endpoint `10.10.10.10` was powered off during the final connectivity test, so endpoint reachability was not used as the final recovery criterion.

### Recovery Validation Chain

```text
Controlled Failure
      ↓
eth1 = A/D
      ↓
10.10.10.0/24 route absent
      ↓
Remediation
      ↓
eth1 = UP / LOWER_UP
      ↓
10.10.10.0/24 route restored
      ↓
Final configuration verified
```

Incident:https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/tree/5cbc1cf23528df0e4721def65cce694bcbc28350/incidents/INC-001-vyos-interface-failure/

Evidence Index:https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/ff9e39b33eab0d75df98bd8a7b08c3fc1366934b/evidence/INDEX.md

## INC-002 — DNS Failure Validation

A controlled DNS service failure was performed on `DC01` to validate DNS troubleshooting and recovery.

### Failure Validation

During the controlled failure:

- DC01 remained reachable over IP.
- The Windows DNS Server service was stopped.
- DNS queries to `10.10.20.10` timed out.
- `dc01.corp.lab` resolution failed.
- `corp.lab` resolution failed.

### Recovery Validation

After restarting the DNS Server service:

- DNS service returned to `Running`.
- `dc01.corp.lab` resolved to `10.10.20.10`.
- `corp.lab` resolved to `10.10.20.10`.
- Explicit DNS queries against `10.10.20.10` succeeded.
- DNS zones remained present.

### Incident Evidence

[INC-002 — DNS Failure](../../incidents/INC-002-dns-failure/README.md)

[INC-002 Command Output](../../incidents/INC-002-dns-failure/evidence/command-output.md)
