# Monitoring

## Overview

Zabbix is the central infrastructure monitoring platform for the lab.

The monitoring environment provides visibility across servers, clients, network devices, and virtualization infrastructure. It is designed to support availability monitoring, network telemetry, problem detection, troubleshooting, and NOC-style operational workflows.

The monitoring architecture is integrated with the network and automation layers rather than operating as an isolated monitoring system.

---

# 1. Monitoring Architecture

The basic monitoring model is:

```text
                         Zabbix01
                    Central Monitoring
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
      Linux/Windows    Network Devices    Proxmox
       Agent 2             SNMP            API/HTTP
          |                |                |
          v                v                v
       Servers          pfSense / VyOS   Virtualization
