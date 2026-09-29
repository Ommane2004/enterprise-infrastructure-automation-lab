# Evidence Index

This index maps the laboratory architecture and engineering
capabilities to the evidence maintained in this repository.

The objective is to make the relationship between:

```text
Architecture
    ↓
Implementation
    ↓
Validation
    ↓
Evidence

```

# 2. Validation Evidence

Validation evidence demonstrates that implemented infrastructure
behaves according to the documented design.

Current validation artifacts:

| Artifact | Validation |
|---|---|
| [`vyos-health-validation.txt`](validation/vyos-health-validation.txt) | Verifies critical VyOS interfaces are present and configured |
| [`vyos-connectivity-validation.txt`](validation/vyos-connectivity-validation.txt) | Verifies successful automated connectivity validation |
| [`vyos-audit-validation.txt`](validation/vyos-audit-validation.txt) | Verifies successful automated VyOS configuration audit |
| [`vyos-backup-validation.txt`](validation/vyos-backup-validation.txt) | Verifies successful automated configuration backup |

The detailed validation evidence is maintained in:

[`validation/`](validation/)

These artifacts represent actual laboratory validation results,
not placeholder examples.
