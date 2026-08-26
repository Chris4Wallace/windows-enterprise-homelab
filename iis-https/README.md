# IIS HTTPS - FILE01

## Overview

FILE01 hosts Internet Information Services (IIS) and was configured to provide HTTPS using a certificate issued by the HomeLab internal Public Key Infrastructure (PKI).

This implementation connects several components of the lab:

- Active Directory-integrated DNS
- Active Directory Certificate Services (AD CS)
- Custom certificate templates
- Certificate enrollment
- IIS
- HTTP.sys SSL certificate bindings
- Windows client certificate validation

The completed configuration allows a domain client to securely access:

`https://file01.homelab.local`

## Environment

- Domain: `homelab.local`
- Web Server: `FILE01`
- Web Server FQDN: `file01.homelab.local`
- Certification Authority: `HOMELAB-Root-CA`
- Certificate Server: `PKI01`
- Client: `Windows10`
- Protocol: HTTPS
- Port: TCP 443

## Certificate Deployment

FILE01 was issued a server authentication certificate from the internal `HOMELAB-Root-CA`.

The certificate was installed in the Local Computer personal certificate store and included the corresponding private key.

![FILE01 certificate](../screenshots/FILE01/03-FILE01-Web-Server-Certificate-Installed.png)

## HTTPS Certificate Binding

The server certificate was bound to TCP port 443 using the Windows HTTP service configuration.

The SSL certificate binding was verified with:

```powershell
netsh http show sslcert
