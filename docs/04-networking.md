# Networking

## Overview

The lab uses a segmented virtual network architecture designed to separate WAN connectivity, routing transit, internal clients, servers/management systems, and security testing.

The network is built primarily with VMware virtual networks, pfSense, and VyOS.

---

## Network Topology

```text
                         INTERNET
                            |
                            |
                    VMware NAT / VMnet2
                    192.168.61.0/24
                            |
                            |
                    pfSense WAN
                  192.168.61.128/24
                            |
                            |
                    pfSense LAN
                  192.168.0.1/24
                            |
                       VMnet10
                            |
                    VyOS eth0
                  192.168.0.2/24
                            |
                 +----------+----------+
                 |                     |
                 |                     |
              eth1                   eth2
        10.10.10.1/24          10.10.20.1/24
                 |                     |
              VMnet11               VMnet12
                 |                     |
          Client Network         Server Network
                 |                     |
        +--------+--------+     +------+--------+
        |                 |     |      |        |
   Windows 10           Kali   DC01 Zabbix01 RHEL01
   10.10.10.10      10.10.10.11

The security-testing environment uses a separate isolated VMware network:
                 VMnet19
             172.16.50.0/24
                    |
             +------+------+
             |             |
          Kali          Metasploitable2
      172.16.50.20       172.16.50.10
