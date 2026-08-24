# Windows Enterprise Homelab

## Overview

This project documents the design, implementation, and troubleshooting
of a multi-server Microsoft Windows enterprise environment.

The lab includes centralized identity, DNS, departmental file services,
Group Policy, enterprise certificate services, computer certificate
enrollment, IIS, and internal HTTPS.

## Goals

The purpose of this lab is to gain practical experience with:

- Enterprise Windows administration
- Identity management
- Group Policy
- Enterprise PKI
- Windows networking
- File services
- Security best practices

## Environment

| System | Purpose |
|---|---|
| DC01 | Active Directory Domain Services and DNS |
| FILE01 | SMB file services and IIS web hosting |
| PKI01 | Active Directory Certificate Services Enterprise Root CA |
| Windows10 | Domain client, testing system, and RSAT management workstation |

## Active Directory

**Domain Name**

homelab.local

**Forest**

homelab.local

## Technologies

- Windows Server 2022
- Windows 10
- Active Directory Domain Services
- DNS
- SMB
- NTFS
- PowerShell
- IIS
- Active Directory Certificate Services
- Group Policy
- RSAT
- VMware Workstation

## Skills Demonstrated

- Domain deployment
- DNS configuration
- Organizational Unit design
- Security group management
- User provisioning
- NTFS permissions
- SMB share configuration
- Group Policy administration
- Certificate enrollment
- HTTPS configuration
- Windows troubleshooting
- PowerShell scripting

## Repository Structure

architecture/
    Network diagrams

dc01-active-directory/
    Active Directory documentation

file01-file-services/
    SMB shares and NTFS permissions

pki01-certificate-services/
    Enterprise CA configuration

iis-https/
    HTTPS deployment

scripts/
    PowerShell automation

screenshots/
    Evidence from the lab
    
## Lessons Learned

During deployment I learned:

- Why DNS is essential for Active Directory.
- Why permissions should be assigned to groups instead of users.
- How Group Policy distributes certificate enrollment settings.
- Why modern browsers require Subject Alternative Names in certificates.
- How IIS uses certificates to establish HTTPS.

## Future Improvements

- Deploy DHCP
- Configure WSUS
- Implement DFS
- Configure roaming profiles
- Add a subordinate CA
- Configure VPN
- Implement Microsoft Defender for Endpoint
- Add monitoring with Windows Admin Center

## Security

This repository contains documentation, scripts, and screenshots from a
lab environment.

No passwords, private keys, certificate exports, product keys, or
sensitive information are included.

```text
homelab.local