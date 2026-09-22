
# Inventory

This directory contains the Ansible inventory structure for the
Enterprise Infrastructure & Automation Lab.

## Purpose

The inventory defines the systems managed by Ansible and organizes
them according to their infrastructure role.

The inventory is designed to support:

- Linux servers
- Windows systems
- Network devices
- Monitoring infrastructure
- Virtualization infrastructure
- Security systems

## Inventory Structure

The public repository contains only sanitized example inventory data.

```text
inventory/
├── README.md
├── hosts.example.ini
└── hosts.ini
