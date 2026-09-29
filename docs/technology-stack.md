# Technology Stack

This document describes the technologies used in the enterprise infrastructure lab and the role each technology plays in the environment.

---

## 1. Networking

| Technology              | Role                                              |
|-------------------------|---------------------------------------------------|
| VMware Virtual Networks | Virtual network segmentation and lab connectivity |
| pfSense                 | Firewall, gateway, NAT, and upstream routing      |
| VyOS                    | Core routing and internal network forwarding      |
| IPv4                    | Network addressing and routing                    |
| DHCP                    | Dynamic address assignment                        |
| DNS                     | Hostname and domain resolution                    |
| SNMP                    | Network-device monitoring                         |
| ICMP                    | Connectivity and availability testing             |

### Network Segments

```text
192.168.61.0/24  → WAN / VMware NAT
192.168.0.0/24   → pfSense ↔ VyOS transit
10.10.10.0/24    → Internal client network
10.10.20.0/24    → Server / management network
172.16.50.0/24   → Isolated security-testing network
