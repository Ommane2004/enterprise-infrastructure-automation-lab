# Monitoring Architecture

## Purpose

This document describes the monitoring and observability
architecture implemented within the Enterprise Infrastructure &
Automation Lab.

The monitoring environment is designed to provide operational
visibility into:

- Host availability
- System health
- Network infrastructure
- Network interfaces
- Interface traffic
- Interface errors
- Interface discards
- Service availability
- Infrastructure problems
- Virtualization infrastructure

The monitoring platform used by the lab is Zabbix.

---

# Monitoring Platform

The primary monitoring server is:

```text
Zabbix01
Ubuntu 24.04 LTS
10.10.20.20/24
