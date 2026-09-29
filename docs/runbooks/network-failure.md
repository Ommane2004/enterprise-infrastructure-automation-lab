# Runbook: Network Failure

## Purpose

This runbook provides a structured procedure for investigating network connectivity failures within the lab.

It covers failures involving:

- Client-to-gateway connectivity
- Server-to-gateway connectivity
- VyOS routing
- pfSense routing
- Internet connectivity
- DNS-related network symptoms
- Interface and link state

The procedure is designed to identify the failure domain before making configuration changes.

---

# 1. Network Architecture

The primary network path is:

```text
Client / Server
      |
      v
     VyOS
      |
      v
   pfSense
      |
      v
 VMware NAT
      |
      v
   Internet
