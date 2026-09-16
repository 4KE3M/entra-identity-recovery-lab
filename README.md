<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Microsoft Entra ID Identity Recovery Lab

**Project Link:** [View Project](https://nextwork.ai/projects/6834030d-3737-424b-9e06-0206548c549c)

**Author:** Akeem Adams

**Skills demonstrated:** Microsoft Entra ID, Microsoft Graph PowerShell, identity troubleshooting, sign-in and audit log analysis, RBAC, authentication vs. authorization, password recovery, and least-privilege administration.

## Overview

This project simulates a help desk identity-recovery incident in Microsoft Entra ID. I investigated failed user sign-ins, analyzed authentication evidence, identified separate root causes, and restored access using delegated administrative permissions and least-privilege principles.

The goal was to troubleshoot the incident based on evidence rather than assuming every reported password problem requires a password reset.

---

<p align="center">
  <img src="https://nextwork.ai/surprised_yellow_clever_kingfisher/uploads/6834030d-3737-424b-9e06-0206548c549c_ucjkc3ot" width="700" alt="Entra identity recovery evidence">
</p>

## Identity Recovery Incident

### Investigating the incident

I analyzed Microsoft Entra sign-in and audit evidence to determine why the user was unable to authenticate and to select the appropriate corrective action.

### Evidence-based remediation

The investigation identified two separate authentication failures:

- **AADSTS50057 – User account is disabled:** I confirmed the account was disabled and re-enabled it rather than performing an unnecessary password reset.
- **AADSTS50126 – Invalid username or password:** After confirming the account was enabled, I determined that the second failure was credential-related and performed a password reset.

Although both failures appeared to the user as sign-in problems, the underlying causes required different corrective actions.

### Protecting sensitive information

Evidence included in this repository was sanitized before publication. Passwords, tokens, IP addresses, tenant-specific identifiers, and other sensitive information were excluded while preserving the technical evidence needed to demonstrate the troubleshooting process.

<p align="center">
  <img src="https://nextwork.ai/surprised_yellow_clever_kingfisher/uploads/6834030d-3737-424b-9e06-0206548c549c_ucjkc3ot" width="700" alt="Entra identity recovery evidence">
</p>

## Delegating Least-Privilege Password Recovery

### Testing scoped helpdesk recovery access

I tested the help desk account before and after assigning delegated administrative permissions to determine the minimum access required for password recovery.

### Separating authentication from authorization

The help desk account successfully authenticated to Microsoft Entra ID but was initially unable to reset the user's password because it lacked the required administrative permissions.

I assigned the **Password Administrator** role for password recovery and the **Reports Reader** role for read-only access to sign-in and audit evidence. This allowed the recovery workflow to be completed without granting the broader **Global Administrator** role.

<p align="center">
  <img src="https://nextwork.ai/surprised_yellow_clever_kingfisher/uploads/6834030d-3737-424b-9e06-0206548c549c_uq6e14wz" width="450" alt="Password recovery evidence">
</p>

## Diagnosing a Disabled Account with Sign-In Evidence

### Investigating the login failure

I reproduced the user's sign-in failure and reviewed Microsoft Entra sign-in logs and account properties before making any changes.

### Identifying the root cause

The sign-in logs returned **AADSTS50057**, indicating that the user account was disabled. I confirmed this by reviewing the account properties, where **Account enabled** was set to **No**.

The evidence showed that the account state, not the password, was causing the authentication failure. I re-enabled the account and tested authentication again rather than performing an unnecessary password reset.

<p align="center">
  <img src="https://nextwork.ai/surprised_yellow_clever_kingfisher/uploads/6834030d-3737-424b-9e06-0206548c549c_9vh5nngl" width="700" alt="Disabled Entra ID account evidence">
</p>

## Creating a Safe Entra Identity Lab

### Establishing test identities

I created fictional employee and help desk accounts with different account states to safely reproduce identity and access scenarios in the lab environment. I verified the identities using both the Microsoft Entra admin center and Microsoft Graph PowerShell.

### Authentication vs. authorization

**Authentication** verifies who a user is, while **authorization** determines what an authenticated user is permitted to access or do.

The lab accounts were intentionally configured with different states and permissions. This allowed me to test authentication failures separately from administrative authorization and demonstrate how account state and assigned roles affect access.

<p align="center">
  <img src="https://nextwork.ai/surprised_yellow_clever_kingfisher/uploads/6834030d-3737-424b-9e06-0206548c549c_41zcxpwa" width="700" alt="Entra ID test identities">
</p>

## Preparing a Secure Administration Environment

### Configuring the Entra lab and Microsoft Graph

I configured a dedicated Microsoft Entra ID test tenant and installed Microsoft Graph PowerShell to perform identity administration and troubleshooting from the command line.

I authenticated to Microsoft Graph and used PowerShell alongside the Entra admin center to inspect and verify user identities and account states.

### Isolating administrative testing

All identity changes were performed in a dedicated lab tenant isolated from production and employer environments. A bootstrap administrator account with **Global Administrator** privileges was used to configure the lab before testing delegated, least-privilege administrative access.

<p align="center">
  <img src="https://nextwork.ai/surprised_yellow_clever_kingfisher/uploads/6834030d-3737-424b-9e06-0206548c549c_eo9phxck" width="700" alt="Entra ID administration environment">
</p>

## Applying Evidence Before Password Resets

### Evidence-driven identity troubleshooting

A reported password problem does not necessarily mean the password is incorrect. I used Microsoft Entra sign-in logs and account-state information to identify the root cause of each authentication failure before taking corrective action.

This approach prevented unnecessary password resets and demonstrated a repeatable troubleshooting process:

**Review evidence → identify root cause → apply the appropriate remediation → verify access**

## Skills and Lessons Learned

### Tools and concepts applied

This project provided hands-on experience with **Microsoft Entra ID, the Entra admin center, Microsoft Graph PowerShell, sign-in logs, audit logs, and role-based access control (RBAC)**.

Key concepts demonstrated include authentication vs. authorization, least privilege, delegated administrative roles, identity troubleshooting, account-state investigation, password recovery, and evidence-based incident analysis.

### Troubleshooting challenges

The most challenging part of the project was locating and correlating the appropriate Entra sign-in and audit events. I had to distinguish between account-state changes, role assignments, authentication failures, and password-reset activity to determine what actually occurred during each stage of the incident.

Working through those events strengthened my understanding of how **authentication, authorization, RBAC, and audit evidence** work together during identity troubleshooting.

### Next steps

A logical next step is automating common identity administration and troubleshooting tasks with **PowerShell and Microsoft Graph**, including account-state checks, user investigations, and repeatable recovery workflows.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/6834030d-3737-424b-9e06-0206548c549c)*
