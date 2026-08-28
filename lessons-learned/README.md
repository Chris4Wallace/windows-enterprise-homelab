# Lessons Learned

> **“The best lessons are learned the hard way.”**

## From an Accidental Installation to an Enterprise Homelab

This project did not begin with a perfectly planned enterprise architecture.

It began with a mistake.

While learning Windows Server, I accidentally installed Windows Server on my personal desktop. Recovering from that mistake required using a USB installation/recovery process to get my desktop working normally again.

It was an uncomfortable introduction to Windows Server, but it became the starting point for a much larger learning experience.

Instead of ending the project there, I continued learning and eventually built a virtualized Windows enterprise environment using VMware Workstation Pro.

What began as an accidental installation developed into a lab containing:

- Active Directory Domain Services
- Active Directory-integrated DNS
- Organizational Units
- Domain users and computers
- Security groups
- Group Policy
- SMB file services
- NTFS permissions
- Active Directory Certificate Services
- Enterprise certificate templates and issuance
- IIS
- Internal HTTPS
- Windows client validation
- PowerShell and RSAT administration

The most valuable part of the project was not simply getting these services working. It was learning how they depend on one another and how to troubleshoot them when they do not.

---

## Lesson 1 — Understand the Environment Before Making Changes

The first lesson came before the enterprise lab even existed.

Accidentally installing Windows Server on my personal desktop demonstrated how important it is to understand the target system, installation media, disks, and intended operating system before making major changes.

Recovering the desktop taught me that system administration is not only about deploying systems. It also requires being able to recover from mistakes.

### What I Learned

- Verify the target system before beginning an operating-system installation.
- Understand what installation media is going to change.
- Treat destructive operations carefully.
- Have recovery media available.
- Recovery skills are just as important as deployment skills.
- A mistake can become a useful learning experience if the cause and recovery process are understood.

---

## Lesson 2 — Build a Lab Before Experimenting on Important Systems

After recovering my desktop, VMware Workstation Pro provided a much safer environment for continuing to learn Windows Server.

Virtual machines allowed me to experiment with servers, networking, domain services, and configuration changes without putting the physical desktop operating system at the same level of risk.

This became the foundation for the rest of the HomeLab.

### What I Learned

Virtualization makes it possible to:

- Isolate experimental systems
- Build multiple servers on one physical computer
- Test networking between systems
- Reconfigure infrastructure without affecting production systems
- Troubleshoot failures in a controlled environment
- Learn through experimentation with significantly less risk

The transition from experimenting directly on a physical desktop to using a virtualized lab was one of the most important improvements in how I approached the project.

---

## How the Lab Evolved

The lab was not designed in its final form from the beginning.

It evolved as I learned each technology and discovered that the next service depended on knowledge gained from the previous one.

What started as learning how to safely run Windows Server in a virtual environment gradually became a connected enterprise infrastructure project.

The progression was approximately:

```text
Desktop recovery
      ↓
VMware Workstation Pro
      ↓
Windows Server virtual machines
      ↓
DC01 / Active Directory
      ↓
DNS and domain connectivity
      ↓
Users, computers, OUs, and security groups
      ↓
FILE01 / SMB and NTFS permissions
      ↓
Group Policy
      ↓
PKI01 / Active Directory Certificate Services
      ↓
Certificate enrollment and trust
      ↓
IIS / HTTPS
      ↓
Windows client validation
      ↓
PowerShell validation
      ↓
GitHub documentation
```

Each stage introduced new dependencies and new troubleshooting problems.

As the environment became more complex, I had to move from simply following configuration steps to understanding how the services interacted.

---

## Troubleshooting Was Part of the Learning Process

A large part of building the HomeLab involved troubleshooting problems that I did not initially know how to diagnose.

Early in the project, I frequently entered PowerShell commands incorrectly, mistyped parameters or paths, and sometimes did not understand why a command failed.

Rather than treating those failures as separate from the project, they became part of learning how Windows administration actually works.

Over time, I became more comfortable reading command output, checking paths, verifying configuration, and correcting mistakes instead of assuming that a failed command meant the entire configuration was broken.

### Windows 10 Client Problems

The Windows 10 system did not always work as expected while the environment was being built.

Because the workstation eventually became the domain client and validation system for the lab, client-side problems required me to investigate the infrastructure the workstation depended on rather than looking only at Windows 10 itself.

This reinforced an important troubleshooting principle:

```text
Client problem
     ↓
Check network configuration
     ↓
Check DNS
     ↓
Check server connectivity
     ↓
Check authentication
     ↓
Check the service being accessed
```

I learned that a problem visible on one machine may actually originate somewhere else in the infrastructure.

### Command-Line Mistakes

Working with PowerShell involved many mistakes.

Commands were mistyped, syntax was entered incorrectly, and commands sometimes had to be corrected before they produced the expected result.

Initially, this could be frustrating because I often needed help determining exactly what command to enter.

Repeatedly correcting those commands, however, gradually made PowerShell less intimidating and made me more familiar with how Windows administrative commands are structured.

The lesson was not to memorize every command.

The more important lesson was learning how to:

- Read the command that was actually entered
- Compare it with the intended syntax
- Identify spelling and parameter mistakes
- Read PowerShell error output
- Correct one problem at a time
- Verify the result after the command succeeds

### File Paths Matter

File paths became another recurring source of troubleshooting.

At different points in the project, files or folders were created through commands and then appeared to be missing when I tried to access them later.

This forced me to pay closer attention to:

- The current working directory
- Absolute versus relative paths
- Folder names
- File names and extensions
- Where a command actually creates its output
- Whether the path being referenced really exists

This lesson became useful again while building the GitHub portfolio, where incorrect relative screenshot paths caused documentation images not to render correctly.

The underlying lesson was the same:

```text
A file can exist and still appear to be "missing"
if I am looking for it through the wrong path.
```

### Password Recovery and Server Core

Forgetting passwords created another practical administration problem during the project.

Because DC01 runs Windows Server Core, there was no full desktop interface to fall back on. Account and password administration had to be handled using the administrative tools available for the environment.

Working through password changes reinforced the importance of becoming comfortable administering Windows without depending entirely on a graphical interface.

It also demonstrated why credential-management procedures matter in a real environment.

Passwords themselves are not included in this repository.

### When the Preferred Method Does Not Work

One recurring inconvenience was the inability to reliably copy and paste between some of the virtual machines.

That issue was never fully resolved.

Instead of allowing it to stop the project, I eventually used Windows networking and administrative shares to access files from another system.

For example:

```text
\\DC01\C$
```

provided a practical way to access files on DC01 from the Windows workstation.

This taught me another troubleshooting lesson: not every problem has to be solved using the originally intended method before work can continue.

Sometimes the practical solution is to understand the limitation, find a safe alternative, verify that it works, and keep moving.

### Learning to Work Through Frustration

There were many points during the project where troubleshooting became frustrating.

I do not remember the exact technical cause of every one of those moments, and I do not want this documentation to invent problems that I cannot verify.

What I do remember is how my response to those situations changed.

Early frustration often came from not knowing what to check next.

As the lab developed, troubleshooting became more structured:

```text
What am I trying to accomplish?
        ↓
What is actually happening?
        ↓
What does the error say?
        ↓
What service does this depend on?
        ↓
How can I verify that dependency?
        ↓
Change one thing
        ↓
Test again
```

That change in troubleshooting approach is one of the most important skills I developed during the project.

---

## Lesson 3 — DNS Is Foundational to Active Directory

As the environment developed, DNS became one of the most important dependencies in the lab.

Active Directory does not simply use DNS for convenient hostname resolution. Domain systems rely on DNS to locate domain controllers and other Active Directory services.

The lab reinforced the importance of checking DNS configuration when troubleshooting domain communication, authentication, and service-discovery problems.

### What I Learned

When troubleshooting a Windows domain, verify the fundamentals first:

```text
IP configuration
      ↓
DNS configuration
      ↓
Name resolution
      ↓
Domain connectivity
      ↓
Authentication
```

A problem that appears to be an Active Directory problem can originate from incorrect DNS or network configuration.

This became a recurring theme throughout the HomeLab. DNS was not an isolated service that I configured once and forgot about. It became part of validating domain connectivity, server communication, and eventually access to services such as `file01.homelab.local`.

As the project grew, checking DNS became one of the first steps I learned to consider when a domain service could not be reached as expected.

---

## Lesson 4 — Active Directory Structure Matters

Building Organizational Units, users, computers, and security groups demonstrated that Active Directory is more than a database of usernames.

The structure of the directory affects how systems can be administered and how policies and permissions can be applied.

Separating infrastructure, users, computers, groups, service accounts, and departmental resources created a more manageable environment than placing everything into default containers.

### What I Learned

Good directory design makes later administration easier.

Organizational Units provide administrative and policy structure, while security groups provide a scalable way to manage authorization.

Those are different responsibilities, and understanding that distinction became important as the lab expanded.

---

## Lesson 5 — File Services Require More Than Creating a Shared Folder

Building FILE01 demonstrated that creating a folder and sharing it is only the beginning of configuring enterprise file services.

One of the first problems occurred while planning the storage volume. The original plan was to use the `E:` drive letter, but that letter was already in use.

Rather than assuming the planned configuration would always match the actual system, I had to inspect the existing environment and adjust the design. The departmental storage was ultimately configured on:

```text
F:\CompanyData
```

This was a simple problem, but it reinforced an important administrative habit: verify the current state of a system before applying a planned configuration.

### Getting CompanyData Accessible

After creating the CompanyData structure, getting the shared resource accessible from other systems was another troubleshooting step.

The intended network path was:

```text
\\FILE01\CompanyData
```

Creating the folders on FILE01 did not automatically mean that clients could access them correctly.

Troubleshooting required thinking about several different layers:

```text
Folder exists on FILE01
        ↓
SMB share exists
        ↓
Network path is reachable
        ↓
User can authenticate
        ↓
Share permissions allow access
        ↓
NTFS permissions allow access
```

This helped me understand that a file-sharing problem seen from a client does not necessarily mean that the folder itself is the problem.

### Share Permissions and NTFS Permissions

One of the more confusing parts of configuring FILE01 was understanding the relationship between SMB share permissions and NTFS permissions.

Both can affect what a user is ultimately allowed to do.

That meant troubleshooting access required looking beyond a single permission screen.

The effective result depends on the permissions applied throughout the access path.

This became an important lesson because a user can appear to have the correct access in one place while still being restricted somewhere else.

### Group-Based Authorization

The final authorization model used Active Directory security groups rather than assigning departmental permissions directly to individual users.

```text
Domain User
     ↓
Active Directory Security Group
     ↓
SMB / NTFS Permissions
     ↓
Departmental Resource
```

This made the environment easier to administer because access could be controlled through group membership instead of repeatedly modifying permissions on FILE01 for individual users.

### Testing With Different Users

Changing the group memberships of Sarah Johnson and Sara Davis was part of deliberately testing the authorization model.

I understood that changing a user's group membership was not enough by itself to demonstrate that the permissions worked.

The user sessions also needed to reflect the updated security context, which is why logging out and back in was part of the testing process.

Test files were then created to prove that authorized users could actually write to the appropriate locations.

The tests included both successful and denied access.

For example:

```text
Sarah Johnson
     ↓
Finance authorization
     ↓
Finance access succeeds
     ↓
Engineering access denied
```

and:

```text
Sara Davis
     ↓
HR authorization
     ↓
HR access succeeds
     ↓
Finance access denied
```

Public write access was also tested as part of validating the intended permission model.

### Why Both Allow and Deny Tests Matter

I understood why both sides of the authorization model needed to be tested.

Showing that an authorized user can access a resource proves only half of the configuration.

A properly validated security model should demonstrate:

```text
Authorized user
      ↓
Access succeeds

AND

Unauthorized user
      ↓
Access denied
```

The denied tests were therefore not failed tests.

They were **successful security tests** because the system prevented users from accessing departmental resources they were not authorized to use.

### File Paths Continued to Matter

FILE01 also reinforced the earlier lesson about file paths.

Commands involving folders or resources could fail when the path or folder name being referenced did not match the actual location.

As the file-server configuration became more complex, I had to become more deliberate about verifying paths rather than assuming that a file or folder had been created where I expected it to be.

### What I Learned

FILE01 brought several earlier lessons together:

- Verify existing storage before assigning drive letters.
- Confirm that a folder actually exists at the expected path.
- Creating a folder is different from sharing it.
- Sharing a folder does not automatically mean a client can access it.
- SMB share permissions and NTFS permissions both matter.
- Use Active Directory security groups to manage resource authorization.
- Refresh the user's security context after relevant group-membership changes.
- Test permissions from the perspective of actual domain users.
- Successful access proves authorization.
- Access denied can also be a successful test when denial is the intended result.
- Troubleshoot file access as a chain of dependencies rather than as a single setting.

By the end of the FILE01 portion of the project, I was no longer only checking whether a shared folder opened.

I was testing whether the **right user could perform the right action on the right resource while the wrong user was prevented from doing so**.

---

## Lesson 6 — Installation Is Not the Same as Validation

One of the recurring lessons throughout the project was that successfully installing a Windows role does not prove that the service is configured correctly.

The lab therefore evolved toward validating configurations after implementation.

Examples included:

- Querying Active Directory with PowerShell
- Verifying FSMO role ownership
- Reviewing Organizational Units
- Querying domain users and computers
- Checking security-group membership
- Verifying DNS records
- Testing SMB permissions
- Testing both authorized and unauthorized access
- Checking the AD CS service
- Inspecting issued certificates
- Validating certificate chains
- Reviewing HTTPS bindings
- Testing HTTPS from a Windows client

### What I Learned

A better administrative workflow is:

```text
Configure
   ↓
Verify
   ↓
Test
   ↓
Troubleshoot
   ↓
Validate again
   ↓
Document
```

That approach became increasingly important as the environment grew more interconnected.

Another important change was learning that the expected result is not always a successful connection.

When testing security controls, an `Access Denied` result can be the evidence that proves the configuration is working correctly.

This changed the way I thought about validation. The question was no longer simply:

```text
"Did it work?"
```

It became:

```text
"Did the system behave exactly as the design intended?"
```

---

## Lesson 7 — PKI Taught Me to Troubleshoot Dependencies

Building PKI01 was one of the more challenging parts of the HomeLab.

Until this stage, many of the technologies I had worked with were easier to see directly. A user could log in, a folder could be opened, or a DNS name could be resolved.

Certificate services introduced several new concepts at once.

The final environment used PKI01 to host Active Directory Certificate Services as the Enterprise Root Certification Authority:

```text
HOMELAB-Root-CA
```

Getting from installing AD CS to successfully issuing and using certificates required significantly more troubleshooting than simply installing the server role.

### Configuring Active Directory Certificate Services

Installing and configuring AD CS on PKI01 was one of the first challenges.

I had to understand that installing the role was only the beginning. The Certification Authority itself needed to be configured correctly before the rest of the certificate infrastructure could depend on it.

This reinforced an earlier lesson:

```text
Installed ≠ Configured ≠ Validated
```

A Windows role appearing as installed does not prove that the service is ready to perform its intended function.

### Certificate Templates and Publication

Certificate templates introduced another layer of complexity.

It was not enough for the Certification Authority to exist. The correct certificate template also needed to be configured and published so that systems in the domain could use it.

This helped me understand that certificate issuance is controlled through more than the CA itself.

The process involved several connected pieces:

```text
Certification Authority
        ↓
Certificate Template
        ↓
Template Publication
        ↓
Certificate Enrollment
        ↓
Certificate Issuance
```

If one part of that chain was incorrect, the certificate might not be available or issued as expected.

### Getting FILE01 a Certificate

Getting FILE01 to request and receive the intended certificate was another troubleshooting challenge.

This was where certificate services stopped being an isolated PKI01 configuration exercise and became an infrastructure-integration problem.

PKI01 had to provide the certificate infrastructure, while FILE01 had to successfully enroll for and receive a certificate that could later be used by IIS.

I also had to work through certificate enrollment and auto-enrollment behavior.

This helped demonstrate why Group Policy became part of the certificate architecture rather than being a completely separate part of the lab.

The final environment included the:

```text
Computer Certificate Auto-Enrollment
```

Group Policy Object linked at the `homelab.local` domain level.

### Certificate Purpose Matters

Another concept I had to learn was that simply possessing a certificate does not mean that certificate is appropriate for every purpose.

For the FILE01 web service, the certificate needed to support:

```text
Server Authentication
```

Understanding certificate purpose helped me move beyond thinking of a certificate as simply a file that proves trust.

Certificates contain information that determines how they are intended to be used.

### Trust Has to Reach the Client

Issuing the FILE01 certificate still did not complete the process.

The Windows 10 client also needed to trust the Certification Authority that issued it.

That meant understanding the relationship between:

```text
FILE01 Certificate
        ↓
Issued by HOMELAB-Root-CA
        ↓
Root CA must be trusted
        ↓
Certificate chain validates
        ↓
Windows client trusts the service
```

Getting Windows 10 to trust `HOMELAB-Root-CA` was an important part of understanding PKI.

A certificate can be correctly issued and installed on a server while still producing a trust problem from the client's perspective.

### PowerShell and Certificate Troubleshooting

Certificate troubleshooting also involved PowerShell commands that I sometimes mistyped or that did not initially return what I expected.

Certificates added another challenge because I had to understand not only the command syntax but also what information I was actually looking for.

Over time, I became more comfortable using command output to verify certificate state rather than treating an unexpected result as a dead end.

### What I Learned

The PKI portion of the project taught me to think about certificates as part of a system rather than as individual files.

```text
AD CS
  ↓
Certification Authority
  ↓
Certificate Template
  ↓
Publication
  ↓
Enrollment
  ↓
Issuance
  ↓
Certificate Purpose
  ↓
Trust
  ↓
Application
```

When certificate deployment fails, troubleshooting needs to identify **which part of that chain is failing**.

That was a significant change from simply asking:

```text
"Why isn't the certificate working?"
```

The better question became:

```text
"Which dependency in the certificate process is not working?"
```

---

## Lesson 8 — HTTPS Requires the Entire Infrastructure to Work Together

After FILE01 received its certificate, the next challenge was actually using it with IIS.

This became one of the clearest examples in the project of multiple enterprise technologies depending on one another.

The goal appeared simple:

```text
https://file01.homelab.local
```

Getting there was not.

### IIS Needed the Correct Certificate

Having a certificate installed on FILE01 did not automatically mean IIS was using it.

I had to work through configuring IIS to use the intended server certificate.

The HTTPS binding then needed to be configured for TCP port:

```text
443
```

This reinforced the difference between possessing a certificate and actually configuring an application to use that certificate.

The dependency became:

```text
Certificate issued
      ↓
Certificate installed on FILE01
      ↓
IIS configured
      ↓
HTTPS binding configured
      ↓
Certificate assigned to binding
```

### Client Testing Exposed the Full Dependency Chain

Getting HTTPS working from Windows 10 was another troubleshooting challenge.

At that point, there were many possible places where something could be wrong.

A failed HTTPS connection could potentially involve:

- Windows 10
- Network connectivity
- DNS
- FILE01
- IIS
- The HTTPS binding
- The FILE01 certificate
- Certificate purpose
- Certificate trust
- `HOMELAB-Root-CA`

Initially, having that many possible causes made troubleshooting difficult.

The important lesson was learning not to change everything at once.

Instead, the problem could be broken into layers.

```text
Can Windows 10 reach FILE01?
        ↓
Can DNS resolve file01.homelab.local?
        ↓
Is IIS running?
        ↓
Is HTTPS bound to port 443?
        ↓
Is the correct certificate assigned?
        ↓
Is the certificate valid for server authentication?
        ↓
Does the client trust HOMELAB-Root-CA?
        ↓
Does the certificate chain validate?
        ↓
Does HTTPS work?
```

This became one of the strongest examples in the project of dependency-based troubleshooting.

### The Problem Is Not Always Where the Error Appears

One of the difficult parts of troubleshooting HTTPS was determining whether a problem belonged to DNS, the certificate, certificate trust, IIS, or the Windows client.

The error was visible in one place, but the cause could exist somewhere else.

That reinforced a lesson that had already appeared while troubleshooting Active Directory and FILE01:

```text
Where the problem appears
          ≠
Where the problem originates
```

Instead of focusing only on the final error, I learned to work backward through the services that had to succeed before the final result could work.

### Successful HTTPS Meant More Than a Web Page

Eventually, Windows 10 successfully accessed:

```text
https://file01.homelab.local
```

using the certificate issued through the HomeLab PKI.

At that point, the successful page represented much more than IIS working.

It demonstrated that multiple parts of the environment were functioning together:

```text
Active Directory
      +
DNS
      +
Group Policy
      +
PKI01 / AD CS
      +
Certificate Template
      +
Certificate Issuance
      +
Root CA Trust
      +
FILE01
      +
IIS
      +
HTTPS Binding
      +
Windows 10
      ↓
Successful trusted HTTPS connection
```

### What I Learned

The HTTPS portion of the project changed the way I approached troubleshooting.

When many technologies are involved, guessing at the final problem is inefficient.

A better approach is to identify the dependencies and validate them individually.

```text
Identify the symptom
        ↓
Map the dependencies
        ↓
Test each dependency
        ↓
Find the failing layer
        ↓
Correct it
        ↓
Test again
        ↓
Validate from the client
```

By the time HTTPS was working, I had not simply learned how to create an IIS binding.

I had learned how **DNS, Active Directory, Group Policy, certificate services, trust, server configuration, and client validation can combine to deliver one apparently simple service**.

---

## Lesson 9 — PowerShell Is Both an Administration and Troubleshooting Tool

PowerShell became increasingly important as the project developed.

It was used not only to configure and query systems but also to verify that the environment matched the intended design.

Examples included Active Directory queries, certificate inspection, service validation, permission checks, and configuration verification.

This was particularly important when administering DC01 as Windows Server Core, where command-line and remote administration are central parts of managing the server.

### What I Learned

Knowing how to verify a configuration from the command line makes troubleshooting more repeatable and produces useful evidence for documentation.

Instead of relying only on what a graphical interface appears to show, PowerShell can provide direct and repeatable validation of system state.

I also learned that becoming comfortable with PowerShell does not mean never making syntax mistakes. It means becoming better at recognizing, correcting, and learning from them.

---

## Lesson 10 — Documentation Is Part of the Build

The GitHub portfolio was not simply created after the technical work was finished.

Organizing screenshots, correcting filenames, fixing relative image paths, reviewing Markdown, documenting commands, and comparing documentation against actual evidence became another part of the project.

This process exposed its own mistakes, including broken screenshot references and documentation that needed to be corrected to match the actual configuration.

### What I Learned

Technical documentation should be treated like technical configuration:

```text
Write
 ↓
Review
 ↓
Test
 ↓
Correct
 ↓
Verify
```

If a screenshot does not render, a command is documented incorrectly, or a README claims something the evidence does not demonstrate, the documentation is not finished.

Documenting the lab also forced me to go back through previous work and make sure I could explain what had been configured and why.

---

## The Biggest Lesson

The project did not progress in a straight line.

There were mistakes, troubleshooting sessions, mistyped commands, incorrect paths, forgotten passwords, configuration changes, validation steps, documentation corrections, and moments where I had to understand *why* something worked instead of simply finding a command that made it work.

That became one of the most valuable parts of the HomeLab.

The goal is no longer simply:

```text
Make it work.
```

The goal is:

```text
Understand it.
Build it.
Break it.
Troubleshoot it.
Fix it.
Validate it.
Document it.
Be able to explain it.
```

The environment that exists now is important, but the troubleshooting and learning required to build it are what made the project valuable.

---

## Continuing the Lab

The HomeLab remains an ongoing learning environment.

Future additions will introduce new technologies, new integrations, and inevitably new problems to solve.

When those problems occur, they will become part of the project rather than something to hide.

After all:

> **The best lessons are learned the hard way.**