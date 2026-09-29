# Runbook: DNS Failure

## Purpose

This runbook provides a structured procedure for investigating DNS resolution failures in the `corp.lab` environment.

It covers:

- Internal hostname resolution
- External DNS resolution
- DNS server availability
- DNS forwarding
- Client DNS configuration
- Domain-related DNS problems

---

# 1. DNS Architecture

DC01 provides internal DNS for the `corp.lab` environment.

```text
DC01
10.10.20.10
    |
    v
 Internal DNS
    |
    +---- corp.lab records
    |
    +---- Upstream DNS Forwarder
