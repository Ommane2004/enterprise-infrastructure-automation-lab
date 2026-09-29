# Infrastructure Automation

## Overview

Ansible is used as the primary infrastructure-automation framework in the lab.

The automation environment is hosted on **Zabbix01** and is used to manage and validate Linux systems, Windows Server, Active Directory workflows, and VyOS network infrastructure.

The objective is to convert repeatable operational procedures into documented, reproducible automation while retaining explicit validation of the resulting state.

---

# 1. Automation Architecture

The automation model is:

```text
                    Zabbix01
              Automation Controller
                       |
          +------------+-------------+
          |            |             |
          v            v             v
        Linux       Windows         VyOS
       SSH/Ansible  WinRM/Ansible  Network CLI
          |            |             |
          v            v             v
       RHEL01        DC01          core01
                     |
                     v
                Active Directory
