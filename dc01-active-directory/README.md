# DC01 - Active Directory Domain Services

## Overview

DC01 serves as the primary domain controller for the HomeLab Windows enterprise environment.

Active Directory Domain Services (AD DS) provides centralized identity, authentication, authorization, and management for domain-joined systems and users throughout the lab.

This portion of the project demonstrates the deployment and administration of a Windows Active Directory environment, including domain services, Organizational Unit (OU) design, user and computer management, DNS integration, and PowerShell-based validation.

---

## Environment

| Component | Configuration |
|---|---|
| Domain | `homelab.local` |
| NetBIOS Name | `HOMELAB` |
| Domain Controller | `DC01` |
| Server Role | Active Directory Domain Services |
| DNS | Active Directory-integrated DNS |
| Administration | PowerShell and Windows administrative tools |
| Domain Clients | Windows servers and workstations |

---

## Active Directory Implementation

The Active Directory environment was configured to provide centralized administration for the HomeLab domain.

Key components include:

- Active Directory Domain Services
- `homelab.local` domain
- Organizational Units
- Domain user accounts
- Security groups
- Domain computer accounts
- Active Directory-integrated DNS
- Group Policy integration
- Centralized authentication and authorization
- PowerShell-based administration

Active Directory provides the identity foundation used by the other Windows infrastructure services throughout the lab.

---

## Domain and FSMO Role Validation

The Active Directory domain configuration and domain-level FSMO role ownership were verified using PowerShell.

```powershell
Get-ADDomain | Format-List DNSRoot,NetBIOSName,DomainMode,PDCEmulator,RIDMaster,InfrastructureMaster
```

The results confirmed:

- DNS domain: `homelab.local`
- NetBIOS domain: `HOMELAB`
- Domain mode: `Windows2016Domain`
- PDC Emulator: `DC01.homelab.local`
- RID Master: `DC01.homelab.local`
- Infrastructure Master: `DC01.homelab.local`

![DC01 Domain Configuration](../screenshots/DC01-Domain-Configuration.png)

---

## Organizational Unit Design

Organizational Units are used to logically organize Active Directory objects and provide a structure for administration and Group Policy targeting.

The OU structure was verified with:

```powershell
Get-ADOrganizationalUnit -Filter * |
Select-Object Name,DistinguishedName |
Format-Table -AutoSize
```

The environment contains OUs for infrastructure and departmental resources, including:

- HomeLab
- Users
- Computers
- Servers
- Groups
- IT
- HR
- Finance
- Sales
- Marketing
- Engineering
- Executive
- Help Desk
- Service Accounts

This structure separates users, computers, servers, groups, service accounts, and departmental resources instead of placing all domain objects into the default Active Directory containers.

![DC01 Organizational Units](../screenshots/DC01-Organizational-Units.png)

---

## Domain Computer Management

Computer objects were queried through Active Directory to verify systems registered within the domain.

```powershell
Get-ADComputer -Filter * -Properties OperatingSystem |
Select-Object Name,OperatingSystem,DistinguishedName |
Format-Table -AutoSize
```

The results demonstrate centralized management of Windows Server and Windows client computer objects within `homelab.local`.

The domain includes infrastructure systems such as:

- `DC01`
- `FILE01`
- `PKI01`
- Windows client systems
- Departmental computer objects

![DC01 Domain Computers](../screenshots/DC01-Domain-Computers.png)

---

## Active Directory User Management

Multiple user accounts were created and managed through Active Directory.

Enabled users were validated using:

```powershell
Get-ADUser -Filter * -Properties Department |
Where-Object {$_.Enabled -eq $true} |
Select-Object Name,SamAccountName,Department |
Format-Table -AutoSize
```

The results demonstrate centralized identity administration using individual domain accounts and unique `SamAccountName` values.

These identities are later used throughout the lab for authentication and resource-access testing.

![DC01 Active Directory Users](../screenshots/DC01-Active-Directory-Users.png)

---

## Security Groups and Authorization

Active Directory security groups are used to manage access to resources rather than assigning permissions directly to individual users.

This approach allows permissions to be managed centrally by changing group membership.

The group-based authorization model is used elsewhere in the lab for departmental access to resources hosted by FILE01.

This demonstrates the relationship between:

`User Account → Security Group → Resource Permission`

---

## DNS Integration

DNS is integrated with Active Directory to provide internal name resolution and support domain services.

The internal DNS namespace is:

`homelab.local`

Domain systems use the internal DNS infrastructure to locate resources such as:

- Domain controllers
- File servers
- Certificate services
- Internal web services
- Active Directory services

DNS integration allows systems to communicate using names such as:

`dc01.homelab.local`

`file01.homelab.local`

`pki01.homelab.local`

rather than relying exclusively on IP addresses.

---

## Domain Authentication

Domain-joined systems authenticate users through Active Directory rather than relying exclusively on independent local accounts.

This provides centralized control over:

- User identities
- Computer identities
- Security groups
- Authentication
- Authorization
- Resource access

This centralized identity model is used throughout the HomeLab environment.

---

## PowerShell Administration

PowerShell is used extensively for Active Directory administration, validation, and troubleshooting.

Examples demonstrated in this project include:

```powershell
Get-ADDomain
Get-ADOrganizationalUnit
Get-ADComputer
Get-ADUser
```

Using PowerShell provides command-line experience with Windows enterprise administration while also producing repeatable methods for validating configuration.

---

## Integration with the HomeLab

Active Directory provides the identity and authentication foundation for the rest of the environment.

DC01 integrates with:

### FILE01

FILE01 uses Active Directory identities and security groups to control access to departmental SMB resources.

### PKI01

PKI01 uses Active Directory Certificate Services to provide certificate infrastructure within the domain.

### Group Policy

The OU structure provides logical targets for Group Policy configuration and centralized management.

### Windows Clients

Domain-joined Windows clients authenticate against Active Directory and are used to validate permissions and access to enterprise resources.

### IIS / HTTPS

Internal DNS and domain trust support access to PKI-secured services such as:

`https://file01.homelab.local`

---

## Validation

The Active Directory implementation was validated by confirming:

- The `homelab.local` domain is operational
- DC01 holds the documented domain FSMO roles
- Organizational Units exist and follow the intended hierarchy
- Domain computer objects are registered in Active Directory
- Multiple enabled domain users exist
- Windows infrastructure servers participate in the domain
- Active Directory identities can be used by other services in the environment

---

## Skills Demonstrated

This portion of the project demonstrates hands-on experience with:

- Windows Server administration
- Active Directory Domain Services
- Domain controller administration
- Active Directory domain architecture
- Organizational Unit design
- User account administration
- Computer object administration
- Security group management
- Active Directory-integrated DNS
- Domain authentication and authorization
- PowerShell administration
- Enterprise identity management
- Infrastructure validation and documentation

---

## Related Project Sections

Additional components of the HomeLab build are documented elsewhere in this repository:

- **FILE01 File Services** - SMB shares, NTFS permissions, and group-based access control
- **Group Policy** - Centralized Windows configuration
- **PKI01 Certificate Services** - Active Directory Certificate Services and certificate enrollment
- **IIS / HTTPS** - Internal TLS using certificates issued by the HomeLab PKI