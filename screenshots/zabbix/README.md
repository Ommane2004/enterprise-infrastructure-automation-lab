## Zabbix Infrastructure Inventory

![Zabbix Infrastructure Inventory]([screenshots/zabbix/09-zabbix-host-inventory.png](https://github.com/Ommane2004/enterprise-infrastructure-automation-lab/blob/main/screenshots/zabbix/09-zabbix-host-inventory.png))

The Zabbix server provides centralized visibility across the lab, including Windows, Linux, network infrastructure, and virtualization systems.

## pfSense SNMP Monitoring

![pfSense SNMP Monitoring](screenshots/zabbix/10-zabbix-pfsense-snmp.png)

Zabbix collects operational telemetry from the pfSense firewall through SNMP. The monitored data includes device availability, interface traffic, errors, discards, and uptime.

## VyOS SNMP Monitoring

![VyOS SNMP Monitoring](screenshots/zabbix/11-zabbix-vyos-snmp.png)

Zabbix monitors the VyOS router through SNMP and collects device and interface telemetry for infrastructure visibility and fault detection.

## Proxmox Monitoring

![Proxmox Monitoring](screenshots/zabbix/12-zabbix-proxmox-monitoring.png)

The Proxmox virtualization environment is integrated into Zabbix to provide centralized infrastructure monitoring and operational visibility.

## Zabbix Problems

![Zabbix Problems](screenshots/zabbix/13-zabbix-problems.png)

The Problems view provides a centralized operational view of detected monitoring conditions and supports the investigation and validation workflow used in the lab.

## NOC Monitoring Dashboard

![Zabbix NOC Dashboard](screenshots/zabbix/14-zabbix-noc-dashboard.png)

The NOC dashboard provides a centralized operational view of the lab environment, bringing together infrastructure availability, detected problems, network monitoring, servers, and virtualization monitoring.
