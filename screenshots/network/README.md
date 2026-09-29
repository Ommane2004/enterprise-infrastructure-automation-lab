## VyOS Interface Configuration

![VyOS Interfaces](screenshots/network/15-vyos-interfaces.png)

The VyOS router uses dedicated interfaces for the pfSense transit network, internal client network, and server network. The screenshot shows the configured IPv4 addresses, operational state, and interface roles.

## VyOS Routing Table

![VyOS Routing Table](screenshots/network/16-vyos-routing-table.png)

The VyOS routing table demonstrates the default path toward the pfSense gateway and the directly connected internal, server, and transit networks.

## pfSense Routing

![pfSense Routing](screenshots/network/17-pfsense-routing.png)

The pfSense firewall provides the upstream routing boundary for the lab. Static routes direct traffic for the internal client and server networks to the VyOS router across the transit network.

## pfSense SNMP Configuration

![pfSense SNMP Configuration](screenshots/network/18-pfsense-snmp-configuration.png)

pfSense is configured as an SNMP-monitored network device. The SNMP daemon operates on UDP port 161 with the required monitoring modules enabled. Authentication data is intentionally excluded from the public portfolio.

## End-to-End Connectivity

![End-to-End Network Connectivity](screenshots/network/19-end-to-end-connectivity.png)

Connectivity testing validates the complete network path from the server network through VyOS and pfSense to the Internet. The test also verifies DNS resolution.
