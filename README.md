# UC00614 --- Web Services Integration and Security

### NGINX Reverse Proxy · Apache · SSL/TLS · Active Directory · DNS · SELinux

**Author:** Pedro Gonçalves\
**Project type:** Hands-on cybersecurity and systems integration
laboratory\
**Technical report:** [View the complete
PDF](./UC00614_Integrar_Sistemas_de_Informacao_Pedro_Goncalves_Layout_Corrigido.pdf)

## Project Overview

This project documents the deployment, configuration, and validation of
a multi-server web infrastructure in an isolated virtual laboratory. It
combines **NGINX reverse proxy and round-robin load balancing**,
**Apache web servers**, **TLS**, **Active Directory-based
authentication**, **DNS**, and **Linux security controls**.

The implementation was tested from a dedicated client machine using
domain names resolved by Windows Server DNS.

## Laboratory Architecture

  --------------------------------------------------------------------------
  Component         Platform          Role              Laboratory address
  ----------------- ----------------- ----------------- --------------------
  Central web       Rocky Linux 9     NGINX reverse     `192.168.130.2/29`
  server                              proxy, Apache,    (frontend),
                                      TLS and           `172.21.130.6/29`
                                      authentication    (backend)

  Backend 1         Rocky Linux 9     Apache site 1     `172.21.130.1/29`

  Backend 2         Rocky Linux 9     Apache site 2     `172.21.130.2/29`

  Backend 3         Rocky Linux 9     Apache site 3     `172.21.130.3/29`

  Windows Server    Windows Server    Active Directory  `192.168.130.1/29`
                                      and DNS           

  Test client       Debian Linux      Browser, curl and `192.168.130.3/29`
                                      DNS validation    
  --------------------------------------------------------------------------

**Networks:** Frontend `192.168.130.0/29` and backend `172.21.130.0/29`.

``` text
                          Windows Server (AD + DNS)
                                      |
Debian test client ---- Frontend LAN ---- Rocky Linux central server
                                              |
                                   NGINX reverse proxy
                                              |
                                        Backend LAN
                                      /       |       \
                                Apache 1  Apache 2  Apache 3
```

## Main Objectives and Implementation

### 1. NGINX Reverse Proxy and Round-Robin Load Balancing

Configured NGINX on the central Rocky Linux server to distribute
requests across **three Apache backend servers**. Each backend displays
a different page, allowing request distribution to be observed by
refreshing the browser.

-   HTTP entry point documented in the implementation:
    `www.batman.local:8282`
-   HTTPS endpoint: `www.batman.local:51515`
-   TLS configured using a self-signed laboratory certificate.

**Implementation note:** the initial assignment mentions port `8888`,
while the configuration and validation screenshots in the report use
`8282`. This README reflects the documented implementation; consult the
PDF for details.

### 2. Apache Authentication with Active Directory

Implemented an HTTPS-protected Apache website using **LDAP integration
with Active Directory**, with access restricted to an authorized group
and a defined time window.

-   Test endpoint: `https://www.soitezes.local:41414`
-   Group-based authorization and weekday/time restrictions.
-   Validation included authorized access and **403 Forbidden**
    responses outside the permitted schedule.

### 3. NGINX HTTP Basic Authentication

Configured a separate NGINX virtual host with local HTTP Basic
authentication.

-   Test endpoint: `http://www.nginxautentica.local:55555`
-   Authentication handled with an `htpasswd` file.

**Security note:** HTTP Basic authentication without TLS does not
protect credentials in transit. This configuration is an educational
example, not a recommended production deployment.

### 4. Apache HTTPS with Local Authentication

Configured another Apache virtual host using TLS and local HTTP Basic
authentication.

-   Test endpoint: `https://www.apacheseguro.local:34433`
-   Self-signed certificate and restricted private-key permissions.
-   Authentication and access logs used during validation.

### 5. Name-Based NGINX Virtual Hosts

Configured three independent websites on **TCP port 80**, selected by
hostname:

-   `www.marte.local`
-   `www.venus.local`
-   `www.lua.local`

Created the necessary DNS records on Windows Server and validated each
website from the Debian client.

## Security Controls

-   **SELinux** remained enabled, with required HTTP port contexts
    configured.
-   **Firewalld** rules were added for the relevant service ports.
-   **OpenSSL** was used to generate self-signed certificates for
    laboratory TLS testing.
-   **LDAP** provided directory-based authentication and group
    restrictions.
-   **DNS** enabled hostname-based access rather than direct IP-based
    browsing.

## Validation and Outcomes

The report documents browser and command-line tests for the reverse
proxy, backend rotation, DNS resolution, Apache and NGINX virtual hosts,
TLS, authentication, and time-based access control. It also records
troubleshooting of an Apache/NGINX port conflict and its correction.

The work demonstrates practical skills in **Linux server administration,
web service integration, network segmentation, reverse proxies, access
control, DNS, TLS, firewall configuration, SELinux, and technical
troubleshooting**.

## Full Technical Report

📄 [**UC00614 --- Integrar Sistemas de Informação
(PDF)**](./UC00614_Integrar_Sistemas_de_Informacao_Pedro_Goncalves_Layout_Corrigido.pdf)

The report contains the implementation steps, configuration examples,
screenshots and validation evidence.

## Publication and Security Notes

This is a controlled, isolated training laboratory. Before publishing
the PDF, review screenshots for personal data, third-party names,
institutional identifiers, service-account passwords and any other
sensitive information. The original report contains demonstration
credentials; **do not reuse those credentials in real systems**.

## Author

**Pedro Gonçalves**\
Cybersecurity \| Linux & Windows Server \| Network Security \| Web
Infrastructure
