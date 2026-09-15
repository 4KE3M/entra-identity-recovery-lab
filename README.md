# Microsoft Entra ID Identity Recovery Lab

**Skills demonstrated:** Microsoft Entra ID, identity troubleshooting, sign-in log analysis, RBAC, authentication vs. authorization, password recovery, and least-privilege administration.

## Overview

This lab simulates a help desk identity-recovery incident in Microsoft Entra ID. I investigated failed user sign-ins, analyzed authentication evidence, identified separate root causes, and restored access using delegated administrative permissions and least-privilege principles.

The goal was to troubleshoot the incident based on evidence rather than assuming every reported password problem requires a password reset.

## Incident Scenario

A user reported being unable to sign in and believed their password was the problem. The investigation required determining whether the failure was caused by invalid credentials, an account state issue, or another identity-related issue before taking corrective action.

## Incident Timeline

### Initial Sign-In Failure
The user reported being unable to access their account. I reviewed Microsoft Entra ID sign-in evidence before making changes to the account.

The sign-in logs returned **AADSTS50057**, indicating that the user account was disabled. Rather than resetting the password, I corrected the account state by re-enabling the user.

### Second Sign-In Failure
After the account was enabled, another failed sign-in occurred. This attempt returned **AADSTS50126**, indicating that the credentials presented during authentication were invalid.

Because the account was now enabled and the sign-in evidence pointed to an authentication failure, I determined that a password reset was an appropriate corrective action.

### Delegated Administrative Access
A help desk administrator successfully authenticated to Microsoft Entra ID but initially could not perform the password reset. This demonstrated that successful authentication does not automatically provide authorization to perform administrative actions.

I assigned the administrator the **Password Administrator** role for password-recovery operations and the **Reports Reader** role for read-only access to sign-in and audit evidence.

### Resolution
Using the delegated permissions, I reset the affected user's password and verified that the identity-recovery workflow could be completed without granting the administrator Global Administrator privileges.

## Authentication vs. Authorization

This incident demonstrated the difference between **authentication** and **authorization**.

The help desk administrator was able to successfully sign in to Microsoft Entra ID, proving that their identity had been authenticated. However, the administrator's initial attempt to reset the user's password was denied because the account did not yet have permission to perform that administrative action.

**Authentication answered:** Who is this user?

**Authorization answered:** What is this user allowed to do?

Successful authentication alone was therefore not enough to perform the password reset. The appropriate administrative role had to be delegated before the recovery action could be completed.

## Root Cause Analysis

### Root Cause 1: Disabled Account

**Evidence:** Microsoft Entra ID sign-in logs returned `AADSTS50057`.

**Diagnosis:** The user's account was disabled. The reported "password problem" was therefore not initially a password issue.

**Corrective Action:** Re-enabled the affected user account and tested authentication again.

### Root Cause 2: Invalid Credentials

**Evidence:** After the account was enabled, a subsequent sign-in attempt returned `AADSTS50126`.

**Diagnosis:** The credentials supplied during authentication were invalid.

**Corrective Action:** Performed a password reset using appropriately delegated administrative permissions and retested access.

## Least-Privilege Administration

Administrative access was delegated according to the principle of least privilege rather than granting broad tenant-wide administrative permissions.

### Password Administrator

The **Password Administrator** role provided the help desk administrator with the password-reset capability required for routine identity-recovery tasks.

### Reports Reader

The **Reports Reader** role provided read-only access to sign-in and audit information needed to investigate authentication failures.

Together, these roles provided the capabilities necessary to investigate and resolve the incident without assigning the **Global Administrator** role.

This reduced unnecessary administrative privilege while still allowing the help desk administrator to perform the required recovery workflow.

## 60-Second Interview Answer

**How would you troubleshoot a user who says their password doesn't work?**

I wouldn't immediately reset the password. I'd first gather information about what the user is experiencing and review the available sign-in or authentication logs.

I'd look for evidence that tells me whether I'm dealing with invalid credentials, a disabled or locked account, a permissions issue, or another authentication problem.

For example, in this lab I encountered two sign-in failures that looked similar from the user's perspective but had different root causes. One was caused by a disabled account, while another was caused by invalid credentials.

Once I identify the root cause, I'd take the appropriate corrective action, such as enabling the account or resetting the password, and then verify that the user can successfully authenticate.

I'd also make sure any administrative actions are performed using appropriately delegated, least-privilege permissions.
