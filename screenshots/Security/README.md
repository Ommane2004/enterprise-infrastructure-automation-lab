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

## Web Service Enumeration

![Web Service Enumeration](screenshots/security/24-web-service-enumeration.png)

Nmap service detection was used to enumerate common web-service ports on the isolated Metasploitable2 target. The assessment identified an HTTP service running Apache HTTP Server.

## HTTP Service Validation

![HTTP Service Validation](screenshots/security/25-http-service-validation.png)

HTTP response headers are retrieved from the isolated Metasploitable2 web service to validate application-layer connectivity and confirm the detected HTTP server.

### Web Application Discovery

![Web Application Discovery](screenshots/security/26-web-application-discovery.png)

The exposed HTTP service was validated through a browser to identify the web application surface available for controlled security testing.
