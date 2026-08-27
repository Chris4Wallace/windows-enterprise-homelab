# Architecture

## Overview

The Windows Enterprise Homelab is designed as a small multi-server domain environment that demonstrates how core Microsoft infrastructure services integrate with one another.

The environment is centered on the `homelab.local` Active Directory domain and uses separate systems for identity, file/web services, certificate services, and client validation.

---

## Logical Architecture

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

---

## DC01

`DC01` provides the core identity and management services for the environment.

Primary responsibilities include:

- Active Directory Domain Services
- `homelab.local` domain authentication
- Active Directory-integrated DNS
- Organizational Unit structure
- User and computer management
- Security-group management
- Group Policy administration

DC01 provides the identity foundation used by FILE01, PKI01, and domain clients.

---

## PKI01

`PKI01` provides internal certificate services using Active Directory Certificate Services.

Primary responsibilities include:

- Enterprise Root Certification Authority
- `HOMELAB-Root-CA`
- Certificate templates
- Certificate issuance
- Certificate publication and CRL configuration
- Certificate trust infrastructure

PKI01 issues certificates used by other systems in the environment, including the FILE01 IIS web service.

---

## FILE01

`FILE01` provides both file services and internal web services.

### File Services

FILE01 hosts departmental SMB resources protected through Active Directory security groups and NTFS permissions.

The authorization model follows:

```text
Domain User
    ↓
Active Directory Security Group
    ↓
SMB / NTFS Permissions
    ↓
Departmental Resource
```

### IIS / HTTPS

FILE01 also hosts IIS and uses a server certificate issued by `HOMELAB-Root-CA` to provide HTTPS for:

```text
https://file01.homelab.local
```

---

## Windows 10 Client

The Windows 10 workstation provides client-side validation for the environment.

It is used to test:

- Domain authentication
- DNS name resolution
- SMB access
- Departmental authorization
- Access-denied behavior
- Certificate trust
- HTTPS connectivity
- RSAT-based administration

This allows infrastructure changes to be validated from the perspective of an actual domain client.

---

## Service Integration

The environment demonstrates several important enterprise dependencies.

### Active Directory and DNS

Active Directory depends on DNS for domain service discovery and internal name resolution.

### Active Directory and FILE01

FILE01 uses domain identities and security groups to control access to departmental resources.

### Active Directory and Group Policy

Group Policy provides centralized configuration for domain systems and supports certificate-services integration.

### Active Directory and PKI

PKI01 operates as an Enterprise Certification Authority integrated with the Active Directory environment.

### PKI and IIS

The internal CA issues the FILE01 web-server certificate used for HTTPS.

### DNS and HTTPS

The FILE01 certificate identifies:

```text
file01.homelab.local
```

Internal DNS allows clients to resolve that FQDN to the FILE01 server.

---

## Security Model

The lab uses centralized identity and group-based authorization rather than assigning permissions directly to individual users.

```text
Identity
   ↓
Security Group
   ↓
Policy / Resource Permission
   ↓
Authorized Service Access
```

Certificate-based trust is used separately to secure internal HTTPS communications.

---

## Validation Model

The architecture was validated at multiple layers:

- Identity — users, computers, and groups in Active Directory
- Name resolution — internal DNS records
- Authorization — SMB and NTFS permission testing
- Policy — Group Policy configuration and linking
- Certificate services — CA, templates, issuance, and trust
- Application services — IIS and HTTPS
- Client experience — Windows 10 access and denial testing

This layered validation approach confirms that the services function together rather than only verifying that individual roles were installed.