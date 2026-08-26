# FILE01 — Enterprise File Services

## Overview

FILE01 provides centralized file services for the `homelab.local` Active Directory environment.

The server hosts the `CompanyData` SMB share used for departmental and shared data. Access is controlled through Active Directory security groups and NTFS permissions, allowing domain identities to receive different levels of access to FILE01 resources.

The implementation was validated from a domain-joined Windows 10 workstation using multiple domain user accounts. Testing included both successful access and intentional access-denied scenarios to verify that authorization boundaries were functioning correctly.

---

## Server Role

FILE01 functions as a domain-integrated Windows member server providing:

- SMB file sharing
- Centralized company data storage
- Departmental folders
- NTFS access control
- Active Directory group-based authorization
- IIS web services
- HTTPS using a certificate issued by the internal PKI

FILE01 integrates with Active Directory, DNS, Windows client systems, and the HomeLab certificate infrastructure.

---

## Storage Configuration

Company data is stored on the dedicated `F:` volume rather than the operating system volume.

The primary data directory is:

```text
F:\CompanyData
```

The directory contains:

- Engineering
- Finance
- HR
- Marketing
- Public
- ServerInfo

Using a dedicated data volume separates shared organizational data from the Windows operating system volume.

![FILE01 CompanyData Folder Structure](../screenshots/file01/FILE01-CompanyData-Folder-Structure.png)

---

## SMB Share Configuration

The CompanyData directory is published as an SMB network share.

**UNC path:**

```text
\\FILE01\CompanyData
```

**Local path:**

```text
F:\CompanyData
```

The share provides domain users with a centralized namespace through which departmental and public resources can be accessed.

![FILE01 SMB Share](../screenshots/file01/FILE01-SMB-Share.png)

### Share and NTFS Permission Model

The SMB share layer is configured with broad access while NTFS permissions provide the more granular authorization controls on the underlying file system.

This separates:

```text
Network Share Access
        ↓
NTFS File-System Authorization
        ↓
Departmental Resource Access
```

Effective access therefore depends on both the SMB share configuration and the NTFS permissions applied to the requested resource.

---

## Active Directory Group-Based Access Control

FILE01 uses Active Directory security groups to manage access rather than assigning permissions directly to individual users.

Department-related security groups include:

- `Engineering_Users`
- `Finance_Users`
- `HR_Users`
- `Marketing_Users`

Administrative identities and groups are also represented in the NTFS access-control configuration.

This approach allows access to be managed centrally through Active Directory group membership.

Instead of repeatedly changing permissions for individual users, administrators can modify the appropriate group membership and allow the existing ACL structure to determine resource access.

![FILE01 CompanyData NTFS Permissions](../screenshots/file01/FILE01-CompanyData-NTFS-Permissions.png)

---

## Departmental Folder Permissions

Departmental folders exist beneath:

```text
F:\CompanyData
```

The structure provides separate locations for organizational resources while NTFS permissions determine which authenticated users can work with the contents of each folder.

### Engineering Example

The Engineering directory demonstrates the use of an Active Directory departmental security group within the NTFS ACL.

![FILE01 Engineering NTFS Permissions](../screenshots/file01/FILE01-Engineering-NTFS-Permissions.png)

Additional NTFS permission evidence was collected for the Finance, HR, Marketing, and Public directories and is retained in the repository's FILE01 screenshot collection.

---

## Windows 10 Client Validation

FILE01 was tested from a domain-joined Windows 10 workstation to verify the configuration from an end-user perspective.

Validation included:

- Domain-user authentication
- DNS/name resolution
- Network connectivity to FILE01
- SMB share connectivity
- CompanyData enumeration
- Authorized departmental access
- Successful file creation
- Unauthorized departmental access
- Public-folder access

Testing both successful and unsuccessful authorization scenarios was important because successful access alone would not prove that departmental access boundaries were being enforced.

---

## CompanyData Share Access

The Windows 10 domain client successfully connected to:

```text
\\FILE01\CompanyData
```

The client was able to enumerate the CompanyData namespace and view the available departmental and public folders.

This demonstrated the complete path between the client and FILE01:

```text
Domain User
    ↓
DNS / Network Connectivity
    ↓
FILE01
    ↓
SMB
    ↓
CompanyData Share
```

---

## Sarah Johnson Access Validation

FILE01 authorization was tested while authenticated as the Sarah Johnson domain account.

The tests demonstrated that access differed depending on the requested departmental resource.

### Finance — Authorized

Sarah Johnson successfully accessed the Finance directory and created a test file.

This confirmed effective write access to the authorized Finance resource.

![Sarah Johnson Finance Write Access](../screenshots/windows10/13-Windows10-SarahJohnson-Finance-Write-Access.png)

### Engineering — Access Denied

An attempt by Sarah Johnson to access:

```text
\\FILE01\CompanyData\Engineering
```

resulted in an access-denied response.

This demonstrated that being able to connect to the CompanyData share did not automatically provide access to every departmental resource contained within it.

![Sarah Johnson Engineering Access Denied](../screenshots/windows10/12-Windows10-SarahJohnson-Engineering-Access-Denied.png)

### Public — Authorized

Sarah Johnson was also able to access the Public directory and create a test file, confirming that the shared Public resource was available as designed.

---

## Sarah Davis Access Validation

A second domain user was used to verify that FILE01 authorization changed according to Active Directory group membership.

### HR — Authorized

Sarah Davis successfully accessed the HR directory and created a test file.

This confirmed effective write access to the authorized HR resource.

![Sarah Davis HR Write Access](../screenshots/windows10/09-Windows10-SaraDavis-HR-Write-Access.png)

### Finance — Access Denied

An attempt by Sarah Davis to access:

```text
\\FILE01\CompanyData\Finance
```

resulted in an access-denied response.

This provided negative authorization testing and demonstrated that the account could not access the protected Finance resource.

![Sarah Davis Finance Access Denied](../screenshots/windows10/08-Windows10-SaraDavis-Finance-Access-Denied.png)

### Public — Authorized

Sarah Davis successfully accessed the Public directory and created a test file.

This provided an additional validation point for the shared Public resource.

---

## Authorization Validation

The Windows 10 tests demonstrate the relationship between Active Directory identities, security groups, FILE01 permissions, and resource access.

The authorization model can be summarized as:

```text
Active Directory User
        ↓
AD Security Group Membership
        ↓
FILE01 NTFS Permissions
        ↓
Resource Authorization
        ↓
Allow / Deny Result
```

The tests demonstrated both sides of the authorization model:

**Positive testing**

Authorized users successfully accessed and created files within permitted resources.

**Negative testing**

Users attempting to access protected departmental resources for which they lacked authorization received an access-denied response.

This demonstrates that FILE01 is not simply providing universal access to every domain user.

---

## IIS and HTTPS

FILE01 also hosts Internet Information Services (IIS).

The internal web service is accessible using the fully qualified domain name:

```text
https://file01.homelab.local
```

HTTPS operates over TCP port `443` using a server certificate issued by the HomeLab internal Public Key Infrastructure.

The certificate presented by FILE01 identifies:

```text
file01.homelab.local
```

and was issued by:

```text
HOMELAB-Root-CA
```

The service was validated from the Windows 10 domain client by connecting to FILE01 over HTTPS and inspecting the certificate presented by the server.

![Windows 10 FILE01 HTTPS Validation](../screenshots/windows10/05-Windows10-FILE01-HTTPS-Success.png)

Detailed certificate-services configuration is documented separately in the PKI portion of this repository.

---

## Integration with Active Directory

FILE01 relies on the Active Directory environment hosted by DC01 for identity and authorization.

Active Directory provides:

- Domain user identities
- Security groups
- Computer identities
- Authentication
- Authorization context
- DNS integration

FILE01 then applies those identities and group memberships to its file-system permissions.

This creates an integrated authorization workflow rather than relying on independent local user accounts.

---

## DNS Integration

FILE01 participates in the internal `homelab.local` DNS namespace.

Domain clients can access the server using:

```text
FILE01
```

or its fully qualified domain name:

```text
file01.homelab.local
```

DNS-based access is used by both SMB and HTTPS services.

---

## Security Design

The FILE01 implementation demonstrates several Windows enterprise file-services concepts:

- Centralized network storage
- Dedicated data storage
- Active Directory authentication
- Security-group-based authorization
- NTFS access control
- Separation of SMB share and NTFS permissions
- Departmental resource boundaries
- Positive authorization testing
- Negative authorization testing
- Public shared resources
- DNS-based service access
- IIS integration
- Enterprise PKI integration

Using security groups rather than individual user ACL entries provides a more manageable and scalable authorization model.

---

## Validation Results

FILE01 was successfully validated as a domain-integrated Windows file server.

Testing confirmed that:

- FILE01 participates in the `homelab.local` domain.
- Domain clients can resolve and communicate with FILE01.
- `\\FILE01\CompanyData` is available over SMB.
- The CompanyData directory provides centralized departmental storage.
- Active Directory security groups participate in NTFS authorization.
- Authorized users can access and write to permitted departmental resources.
- Unauthorized departmental access can be denied.
- Public resources are accessible to the tested users as designed.
- FILE01 hosts IIS.
- HTTPS is available through `file01.homelab.local`.
- FILE01 presents a server certificate issued by `HOMELAB-Root-CA`.

Together, these results demonstrate integration between:

```text
Active Directory
      ↓
DNS
      ↓
FILE01
      ↓
SMB + NTFS
      ↓
Windows 10 Client Authorization
```

and:

```text
Active Directory
      ↓
Enterprise PKI
      ↓
FILE01 / IIS
      ↓
HTTPS
      ↓
Windows 10 Client Validation
```

---

## Skills Demonstrated

This portion of the HomeLab project demonstrates hands-on experience with:

- Windows Server administration
- Windows file services
- SMB share administration
- NTFS permissions
- Active Directory security groups
- Group-based resource authorization
- Windows ACL concepts
- Departmental file-server design
- Domain client testing
- Positive and negative access testing
- DNS integration
- IIS administration
- HTTPS configuration
- Enterprise PKI integration
- PowerShell administration
- Infrastructure validation
- Technical troubleshooting
- Technical documentation

---

## Related Project Sections

Additional components are documented elsewhere in this repository:

- **DC01 Active Directory** — Domain services, identities, Organizational Units, DNS, and computer objects
- **PKI01 Certificate Services** — Active Directory Certificate Services and certificate issuance
- **IIS / HTTPS** — FILE01 TLS configuration and certificate binding
- **Group Policy** — Centralized domain configuration and certificate auto-enrollment