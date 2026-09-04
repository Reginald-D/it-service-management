<div align="center">

# IT Helpdesk Portfolio
### Account & Access — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Account%20%26%20Access-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-6-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-Entra%20ID%20%7C%20M365%20%7C%20VPN-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases six representative incident tickets spanning the most common **account and access** issues an enterprise helpdesk/IAM team encounters — from password lockouts and lost MFA devices to new-hire provisioning gaps and post-termination access risks.

Each ticket follows a standard **ServiceNow-style incident format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are **representative scenarios** built to demonstrate structured troubleshooting methodology — they are not records of actual incidents.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0066108`](#inc0066108--user-locked-out-after-multiple-failed-login-attempts) | User Locked Out After Multiple Failed Login Attempts | P2 | 15 min |
| 2 | [`INC0066142`](#inc0066142--user-lost-authenticator-device-cannot-complete-mfa) | User Lost Authenticator Device, Cannot Complete MFA | P2 | 25 min |
| 3 | [`INC0066178`](#inc0066178--new-hire-account-not-provisioned-before-start-date) | New Hire Account Not Provisioned Before Start Date | P2 | 1 hr (escalated) |
| 4 | [`INC0066215`](#inc0066215--offboarding--access-not-fully-removed-after-termination) | Offboarding — Access Not Fully Removed After Termination | P1 | 35 min (escalated) |
| 5 | [`INC0066249`](#inc0066249--access-request-to-department-shared-drive-denied) | Access Request to Department Shared Drive Denied | P4 | 20 min |
| 6 | [`INC0066283`](#inc0066283--vpn-connection-fails-with-authentication-error) | VPN Connection Fails with Authentication Error | P2 | 30 min |

---

<br>

## `INC0066108` — User Locked Out After Multiple Failed Login Attempts

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Entra ID (Azure AD) joined device</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (blocking work)</td></tr>
<tr><td><b>Category</b></td><td>Account &amp; Access</td></tr>
<tr><td><b>Time to Resolve</b></td><td>15 minutes</td></tr>
</table>

**Reported Symptom**
User reports their account is locked after several failed sign-in attempts and cannot access email, Teams, or any SSO-connected applications.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Verified the user's identity per the identity-verification procedure (employee ID and manager confirmation) before taking any account action.
2. Checked Entra ID sign-in logs to confirm the failed attempts originated from the user's known device and location, ruling out a compromise attempt.
3. Reviewed the account's lockout status and lockout policy threshold in Entra ID.
4. Reset the user's password following the organization's complexity policy and unlocked the account.

</details>

**Root Cause**
The user had recently changed their password on a personal device that was not updated, causing repeated failed authentication attempts from a cached credential on their phone's mail app, which tripped the smart lockout threshold.

**Resolution**
Unlocked the account and issued a temporary password with forced change at next sign-in. Had the user update the cached credential on their mobile mail app to prevent recurring lockouts.

> **User Communication (Ticket Notes)**
> *Reminded the user that after any password change, they need to update saved credentials on all mobile and secondary devices to avoid repeat lockouts.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0066142` — User Lost Authenticator Device, Cannot Complete MFA

<table>
<tr><td><b>Environment</b></td><td>Microsoft Authenticator, Entra ID Conditional Access (MFA required)</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (blocking work)</td></tr>
<tr><td><b>Category</b></td><td>Account &amp; Access</td></tr>
<tr><td><b>Time to Resolve</b></td><td>25 minutes</td></tr>
</table>

**Reported Symptom**
User reports their phone with Microsoft Authenticator installed was lost over the weekend, and they cannot complete MFA to sign in from their laptop.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Verified the user's identity through an out-of-band method (video call with camera on and badge/ID check) per the lost-authenticator verification procedure, since the normal MFA channel was unavailable.
2. Confirmed with the user whether a backup MFA method (phone call or backup code) was registered on the account.
3. Checked Entra ID for the device's registered authentication methods to determine what needed to be revoked.
4. Coordinated with the security team to temporarily disable the lost device's registered authenticator entry.

</details>

**Root Cause**
The user's only registered MFA method was push notification through the lost device, with no backup method configured, leaving no self-service recovery path.

**Resolution**
Removed the lost device's authenticator registration in Entra ID after identity verification, issued a temporary access pass for one-time sign-in, and had the user register a new device along with a backup MFA method (phone call) to prevent a repeat lockout scenario.

> **User Communication (Ticket Notes)**
> *Advised the user to report the lost phone to their carrier for remote wipe, and confirmed with security that no suspicious sign-in attempts had occurred on the account.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0066178` — New Hire Account Not Provisioned Before Start Date

<table>
<tr><td><b>Environment</b></td><td>Entra ID, Microsoft 365, HR system (Workday) integration</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (business impact — Day 1 readiness)</td></tr>
<tr><td><b>Category</b></td><td>Account &amp; Access</td></tr>
<tr><td><b>Time to Resolve</b></td><td>1 hour (escalated to HR/IAM)</td></tr>
</table>

**Reported Symptom**
Hiring manager reports a new employee starting tomorrow has no account, email, or system access provisioned, despite HR confirming the hire was submitted a week ago.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Checked the HR system (Workday) to confirm the new hire record existed and had the correct start date and department assigned.
2. Reviewed the Entra ID provisioning connector logs for any failed sync jobs tied to that employee record.
3. Identified a provisioning error in the sync log related to a missing required field (cost center code) that blocked the automated account creation workflow.
4. Escalated to the HR/IAM provisioning team to correct the missing field and manually trigger the sync.

</details>

**Root Cause**
The HR record was missing a required cost center field, which caused the automated Entra ID provisioning connector to silently fail validation and skip account creation, with no alert generated to notify HR or IT.

**Resolution**
HR corrected the missing cost center field, and the IAM team manually triggered the provisioning sync, which created the account, mailbox, and baseline application access within 30 minutes. Verified login and MFA enrollment worked ahead of the start date.

> **User Communication (Ticket Notes)**
> *Recommended to the IAM team that a validation alert be added for incomplete HR records to prevent similar last-minute Day 1 issues in the future.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0066215` — Offboarding — Access Not Fully Removed After Termination

<table>
<tr><td><b>Environment</b></td><td>Entra ID, Microsoft 365, VPN, shared drives</td></tr>
<tr><td><b>Priority</b></td><td>P1 – Critical (security risk)</td></tr>
<tr><td><b>Category</b></td><td>Account &amp; Access</td></tr>
<tr><td><b>Time to Resolve</b></td><td>35 minutes (escalated to security)</td></tr>
</table>

**Reported Symptom**
Security team flagged that a terminated employee's account was still able to authenticate to VPN two days after their official termination date, despite an offboarding ticket having been submitted.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reviewed the offboarding ticket and checklist to determine which steps had been completed versus skipped.
2. Checked Entra ID sign-in logs and confirmed the account had active sign-ins to VPN and email after the termination date/time.
3. Identified that the account had been disabled in Entra ID, but the on-premises VPN system authenticated against a separate legacy RADIUS server that hadn't received the disable sync.
4. Escalated to the network/security team to immediately force-disable the account on the RADIUS server directly.

</details>

**Root Cause**
The offboarding automation only disabled the account in Entra ID and Microsoft 365, but the legacy VPN RADIUS server was not integrated into that automated workflow and required a separate manual disable step that was missed on the offboarding checklist.

**Resolution**
Immediately disabled the account on the RADIUS server, confirmed no further VPN sign-ins occurred, and reviewed VPN session logs with security to confirm no unauthorized activity took place during the gap. Updated the offboarding checklist and automation scope to include the RADIUS server going forward.

> **User Communication (Ticket Notes)**
> *Escalated as a security finding per policy and documented the gap and remediation in the incident record for the post-termination access review.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0066249` — Access Request to Department Shared Drive Denied

<table>
<tr><td><b>Environment</b></td><td>Windows 11, on-premises file server, AD security groups</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low</td></tr>
<tr><td><b>Category</b></td><td>Account &amp; Access</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User reports receiving an "Access Denied" error when attempting to open the Finance department's shared drive, which they need for a new cross-team project.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the user's manager had approved the access request per the standard access-request approval workflow before taking any action.
2. Checked Active Directory to confirm the user's account was not already a member of the required security group.
3. Verified the correct security group name and associated NTFS permissions on the target shared drive path.
4. Added the user to the appropriate AD security group and confirmed replication completed across domain controllers.

</details>

**Root Cause**
The user's manager had approved the access request, but the group membership had not yet been actioned by IT, which was a standard manual step in the access-provisioning workflow that had not been completed.

**Resolution**
Added the user to the correct AD security group, forced a Group Policy update on the user's device, and confirmed folder access was restored after a fresh login. Access verified working within 20 minutes of ticket assignment.

> **User Communication (Ticket Notes)**
> *Confirmed with the user that access changes can take up to 15 minutes to apply after group membership updates due to AD replication.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0066283` — VPN Connection Fails with Authentication Error

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Cisco AnyConnect VPN client, remote user</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (blocking remote work)</td></tr>
<tr><td><b>Category</b></td><td>Account &amp; Access</td></tr>
<tr><td><b>Time to Resolve</b></td><td>30 minutes</td></tr>
</table>

**Reported Symptom**
Remote user reports VPN client fails to connect with an "Authentication Failed" error, despite entering the correct password and completing MFA push approval.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the user's account was not locked or disabled in Entra ID/Active Directory.
2. Checked the VPN concentrator logs for the specific failure reason tied to the user's connection attempts.
3. Verified the user's device certificate (used for VPN client authentication) had not expired, since expired certs are a common silent-failure cause.
4. Had the user test from a different network (mobile hotspot) to rule out a local ISP or ports-blocked issue.

</details>

**Root Cause**
The user's VPN client device certificate had expired the previous day, causing certificate-based authentication to fail even though the username, password, and MFA steps completed successfully.

**Resolution**
Triggered a manual certificate renewal push through Intune and had the user restart their device to pick up the new certificate. Verified successful VPN connection afterward.

> **User Communication (Ticket Notes)**
> *Flagged the certificate auto-renewal policy to the network team, since renewal should occur automatically before expiration and did not in this case.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 6 |
| **Critical (P1) Incidents** | 1 |
| **High Priority (P2) Incidents** | 4 |
| **Low Priority (P4) Incidents** | 1 |
| **Average Time to Resolve** | ~29 minutes |
| **Systems Touched** | Entra ID · Microsoft 365 · Active Directory · VPN (Cisco AnyConnect &amp; RADIUS) · Intune · Workday |

## Skills Demonstrated

`Identity Verification` · `Entra ID / Azure AD Administration` · `MFA & Conditional Access Troubleshooting` · `HRIS-to-IAM Provisioning` · `Offboarding & Access Revocation` · `Active Directory Security Groups` · `VPN & Certificate-Based Authentication` · `Root Cause Analysis` · `ServiceNow-Style Incident Documentation` · `Cross-Team Escalation (Security, Network, IAM, HR)`

---

<div align="center">

*Built to demonstrate structured, repeatable troubleshooting methodology across the full identity and access lifecycle — from onboarding to offboarding.*

</div>
