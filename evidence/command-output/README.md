# Command Output Evidence

This directory contains sanitized command-line output demonstrating
successful execution and validation of infrastructure automation.

The purpose of this evidence is to show the actual execution result
of automation workflows rather than only documenting that the
automation exists.

---

## Evidence Categories

### VyOS Automation

Expected evidence includes:

- VyOS configuration audit
- VyOS connectivity validation
- VyOS health validation
- VyOS configuration backup

Representative Ansible validation has been successfully executed
against the laboratory VyOS environment.

---

### Linux Automation

Expected evidence includes:

- Linux server audit
- Connectivity validation
- System information
- Service and configuration checks

---

### Windows Automation

Expected evidence includes:

- Windows server audit
- WinRM connectivity
- System validation
- Active Directory-related automation

---

### Active Directory Automation

Expected evidence includes:

- Test-user creation
- Domain connectivity
- Successful Ansible execution against the domain controller

Sensitive credentials must never be included in command output.

---

## Evidence Format

Command output should be stored as sanitized text files where
practical.

Recommended naming convention:

```text
<platform>-<operation>-<date>.txt
