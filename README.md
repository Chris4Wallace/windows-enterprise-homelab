# Windows Enterprise Homelab

## Overview

This project documents the design, implementation, validation, and troubleshooting of a multi-server Microsoft Windows enterprise environment.

The lab integrates Active Directory, DNS, Group Policy, departmental file services, enterprise certificate services, IIS, and internal HTTPS into a single domain-based infrastructure.

Rather than configuring each service independently, the environment demonstrates how core Windows enterprise technologies work together to provide centralized identity, authorization, policy management, certificate services, secure file access, and internal web services.

---

## Environment

| System | Purpose |
|---|---|
| `DC01` | Active Directory Domain Services, DNS, and Group Policy |
| `FILE01` | SMB file services and IIS web hosting |
| `PKI01` | Active Directory Certificate Services Enterprise Root CA |
| `Windows10` | Domain client, validation system, and RSAT management workstation |

### Active Directory Domain

```text
homelab.local
```

### Certification Authority

```text
HOMELAB-Root-CA
```

---

## Architecture

The environment is built around the `homelab.local` Active Directory domain.

```text
                         homelab.local
                              |
                    +---------+---------+
                    |                   |
                  DC01                PKI01
             AD DS / DNS / GPO        AD CS
                    |             HOMELAB-Root-CA
                    |                   |
                    +---------+---------+
                              |
                            FILE01
                      SMB File Services
                         IIS / HTTPS
                              |
                         Windows10
                    Domain Client / RSAT
```

Active Directory provides centralized identity and authentication, while DNS provides internal name resolution.

Group Policy provides centralized Windows configuration and integrates domain systems with certificate services.

PKI01 provides the internal certificate infrastructure used to issue certificates to domain systems and services.

FILE01 uses Active Directory security groups for departmental file authorization and a certificate issued by the internal PKI to provide HTTPS.

Windows10 is used to validate domain authentication, resource permissions, certificate trust, and service accessibility.

---

## Active Directory and DNS

`DC01` provides the identity and authentication foundation for the environment.

The Active Directory implementation includes:

- `homelab.local` domain
- Organizational Unit hierarchy
- Domain users and computer objects
- Departmental security groups
- Group-based authorization
- Active Directory-integrated DNS
- FSMO role validation
- PowerShell-based administration

The OU structure separates infrastructure, users, computers, groups, service accounts, and departmental resources.

**Documentation:** [DC01 Active Directory](dc01-active-directory/README.md)

---

## FILE01 File Services

`FILE01` provides centralized departmental file storage using SMB and NTFS permissions.

The implementation demonstrates:

- SMB share configuration
- NTFS permissions
- Departmental folder structure
- Active Directory security-group integration
- Group-based access control
- Authorized-user access validation
- Unauthorized-user access-denied validation

Permissions are assigned through Active Directory security groups rather than directly to individual users.

```text
User Account
     ↓
Security Group
     ↓
NTFS / SMB Permission
     ↓
Department Resource
```

**Documentation:** [FILE01 File Services](file01-file-services/README.md)

---

## Group Policy

Group Policy provides centralized configuration within the Active Directory environment.

A custom:

```text
Computer Certificate Auto-Enrollment
```

GPO was created and linked at the `homelab.local` domain level.

The documented configuration demonstrates:

- Group Policy Object administration
- Domain-level GPO linking
- Security filtering
- Active Directory integration
- Certificate-services integration

**Documentation:** [Group Policy](group-policy/README.md)

---

## Enterprise PKI

`PKI01` hosts Microsoft Active Directory Certificate Services and operates as the internal Enterprise Root Certification Authority:

```text
HOMELAB-Root-CA
```

The PKI implementation includes:

- Active Directory Certificate Services
- Enterprise Root CA deployment
- Certificate templates
- Certificate-template publication
- Certificate issuance
- CA database validation
- CA publication configuration
- Certificate Revocation List configuration
- Root CA certificate verification
- Certification Authority connectivity testing
- FILE01 web-server certificate issuance

The PKI provides the certificate infrastructure used by other services in the lab.

**Documentation:** [PKI01 Certificate Services](pki01-certificate-services/README.md)

---

## IIS and HTTPS

`FILE01` also hosts Internet Information Services.

A server certificate issued by `HOMELAB-Root-CA` was deployed to FILE01 and used to provide HTTPS for:

```text
https://file01.homelab.local
```

The implementation validates:

- Web-server certificate deployment
- Server Authentication certificate purpose
- TCP 443 HTTPS binding
- Certificate-chain validation
- Internal Root CA trust
- Windows client HTTPS access

This demonstrates the complete integration path between Active Directory, DNS, PKI, certificate issuance, IIS, and a domain client.

**Documentation:** [IIS / HTTPS](iis-https/README.md)

---

## Technologies

- Windows Server 2022
- Windows 10
- Active Directory Domain Services
- Active Directory-integrated DNS
- Group Policy
- Active Directory Certificate Services
- SMB
- NTFS
- IIS
- HTTPS / TLS
- PowerShell
- RSAT
- VMware Workstation

---

## Skills Demonstrated

This project demonstrates hands-on experience with:

- Windows Server administration
- Active Directory domain administration
- Organizational Unit design
- User and computer administration
- Security-group management
- Group-based authorization
- DNS configuration
- SMB file services
- NTFS permissions
- Group Policy administration
- Enterprise PKI
- Certificate templates and issuance
- Certificate trust and validation
- IIS administration
- HTTPS configuration
- PowerShell administration
- Windows client validation
- Troubleshooting and infrastructure documentation

---

## Repository Structure

```text
windows-enterprise-homelab/
├── architecture/
├── dc01-active-directory/
│   └── README.md
├── file01-file-services/
│   └── README.md
├── group-policy/
│   └── README.md
├── iis-https/
│   └── README.md
├── lessons-learned/
├── pki01-certificate-services/
│   └── README.md
├── screenshots/
├── scripts/
└── README.md
```

Each component README contains implementation details, validation steps, and supporting evidence from the lab environment.

---

## Validation Approach

Configuration was validated rather than relying only on successful installation.

Validation included:

- Active Directory domain and FSMO role verification
- OU, user, computer, and security-group queries
- DNS record verification
- SMB and NTFS permission testing
- Authorized and unauthorized user-access testing
- AD CS service validation
- Certificate issuance and CA database inspection
- Root CA certificate verification
- Certificate-chain validation
- HTTPS binding verification
- Windows client HTTPS testing

PowerShell and native Windows administrative tools were used throughout the environment to provide repeatable configuration and validation methods.

---

## Lessons Learned

Key lessons from the project include:

- DNS is foundational to Active Directory and domain service discovery.
- Resource permissions are easier to manage when assigned through security groups rather than directly to individual users.
- Organizational Unit design affects administration and Group Policy targeting.
- Group Policy provides centralized configuration across domain systems.
- Certificate templates and Enterprise CAs allow certificate deployment to be standardized.
- Certificate trust requires more than simply issuing a certificate; the complete trust chain must validate.
- HTTPS depends on DNS, certificate identity, trust, and correct server bindings working together.
- PowerShell provides repeatable methods for both administration and troubleshooting.

Additional troubleshooting notes and implementation lessons are maintained in the `lessons-learned` section of the repository.

---

## Future Improvements

Potential future expansions include:

- DHCP
- WSUS
- Distributed File System (DFS)
- Additional Group Policy security baselines
- Subordinate Certification Authority
- VPN services
- Microsoft Defender integration
- Centralized monitoring and logging

---

## Security

This repository contains documentation, scripts, and screenshots from an isolated lab environment.

No passwords, private keys, certificate private-key exports, product keys, or production-sensitive information are intentionally included.