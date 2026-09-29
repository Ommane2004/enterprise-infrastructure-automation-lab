# Security Architecture

## Purpose

This document describes the security architecture implemented
within the Enterprise Infrastructure & Automation Lab.

The security architecture focuses on:

- Network segmentation
- Controlled administrative access
- Infrastructure hardening
- Monitoring
- Logging
- Security testing
- Evidence collection
- Incident investigation
- Controlled recovery

The environment is an authorized laboratory used for infrastructure
engineering and cybersecurity experimentation.

---

# Security Architecture

The security architecture uses multiple layers:

```text
                         INFRASTRUCTURE
                               |
                               v
                     NETWORK SEGMENTATION
                               |
                               v
                          FIREWALL
                               |
                               v
                       ACCESS CONTROL
                               |
                +--------------+--------------+
                |                             |
                v                             v
           ADMINISTRATION                 MONITORING
                |                             |
                v                             v
          Ansible / SSH /              Zabbix / Logs
              WinRM
                |                             |
                +--------------+--------------+
                               |
                               v
                       SECURITY ANALYSIS
                               |
                               v
                       INCIDENT RESPONSE
