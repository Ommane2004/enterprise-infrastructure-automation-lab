## Kali Security Network

![Kali Security Network](screenshots/security/20-kali-security-network.png)

Kali Linux is connected to a dedicated security-testing network in addition to the internal client network. The isolated security segment is used for controlled security exercises against lab targets.

## Security Lab Connectivity

![Kali to Metasploitable2 Connectivity](screenshots/security/21-kali-metasploitable-connectivity.png)

The isolated security network provides connectivity between Kali Linux and Metasploitable2 for controlled security testing within the lab environment.

## Metasploitable2 Service Enumeration

![Metasploitable2 Service Enumeration](screenshots/security/22-metasploitable-service-enumeration.png)

Nmap service discovery is used to identify exposed TCP services and their reported versions on the intentionally vulnerable Metasploitable2 target. Testing is restricted to the isolated security-lab network.

## Metasploitable2 Vulnerability Assessment

![Metasploitable2 Vulnerability Assessment](screenshots/security/23-metasploitable-vulnerability-assessment.png)

Nmap vulnerability scripts were used to assess the intentionally vulnerable Metasploitable2 target. The assessment identified the exposed FTP service and associated vulnerability findings within the isolated security-testing network.
