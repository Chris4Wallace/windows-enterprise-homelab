# PKI01 — Active Directory Certificate Services

## Overview

PKI01 provides certificate services for the `homelab.local` Windows enterprise environment using Microsoft Active Directory Certificate Services (AD CS).

The server functions as the internal Enterprise Root Certification Authority and issues certificates used by domain systems and services.

This portion of the project demonstrates:

- Active Directory Certificate Services
- Enterprise Root CA deployment
- Certificate templates
- Certificate enrollment
- Certificate issuance
- Certificate trust
- Certificate chain validation
- Certificate revocation validation
- IIS HTTPS integration

---

## Environment

| Component | Configuration |
|---|---|
| Certificate Server | `PKI01` |
| Domain | `homelab.local` |
| Certification Authority | `HOMELAB-Root-CA` |
| CA Type | Enterprise Root CA |
| Web Server | `FILE01` |
| Client Validation | Windows 10 |
| Web Certificate FQDN | `file01.homelab.local` |

PKI01 is joined to the Active Directory domain and uses the internal domain DNS infrastructure.

---

## Active Directory Certificate Services

The Active Directory Certificate Services role and Certification Authority role service were installed on PKI01.

The AD CS service was verified as:

- Service: `CertSvc`
- Display Name: Active Directory Certificate Services
- Status: Running
- Startup Type: Automatic

This confirms that certificate services are actively running rather than merely installed.

---

## Enterprise Root Certification Authority

PKI01 hosts the internal:

```text
HOMELAB-Root-CA