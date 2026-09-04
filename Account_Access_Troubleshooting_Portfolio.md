# IT Helpdesk Portfolio

**Category: Account & Access — Ticket Documentation**

The following six incident tickets illustrate common root causes and resolution paths for account and access issues, including password resets/lockouts, MFA enrollment and lost authenticator recovery, new hire provisioning, termination/offboarding access removal, shared drive access requests, and VPN authentication failures. Each entry follows a standard ServiceNow-style incident format: reported symptom, diagnostic steps, root cause, resolution, and user-facing ticket notes. These are representative scenarios built to demonstrate structured troubleshooting methodology, not records of actual incidents.

## Ticket INC0066108 — User Locked Out After Multiple Failed Login Attempts

### Reported Symptom

User reports their account is locked after several failed sign-in attempts and they cannot access email, Teams, or any SSO-connected applications.

### Diagnostic Steps

Verified the user's identity per the identity-verification procedure (employee ID and manager confirmation) before taking any account action.

Checked Entra ID sign-in logs to confirm the failed attempts originated from the user's known device and location, ruling out a compromise attempt.

Reviewed the account's lockout status and lockout policy threshold in Entra ID.

Reset the user's password following the organization's complexity policy and unlocked the account.

### Root Cause

The user had recently changed their password on a personal device that was not updated, causing repeated failed authentication attempts from a cached credential on their phone's mail app, which tripped the smart lockout threshold.

### Resolution

Unlocked the account and issued a temporary password with forced change at next sign-in. Had the user update the cached credential on their mobile mail app to prevent recurring lockouts.

### User Communication (Ticket Notes)

Reminded the user that after any password change, they need to update saved credentials on all mobile and secondary devices to avoid repeat lockouts.

## Ticket INC0066142 — User Lost Authenticator Device, Cannot Complete MFA

### Reported Symptom

User reports their phone with Microsoft Authenticator installed was lost over the weekend, and they cannot complete MFA to sign in from their laptop.

### Diagnostic Steps

Verified the user's identity through an out-of-band method (video call with camera on and badge/ID check) per the lost-authenticator verification procedure, since the normal MFA channel was unavailable.

Confirmed with the user whether a backup MFA method (phone call or backup code) was registered on the account.

Checked Entra ID for the device's registered authentication methods to determine what needed to be revoked.

Coordinated with the security team to temporarily disable the lost device's registered authenticator entry.

### Root Cause

The user's only registered MFA method was push notification through the lost device, with no backup method configured, leaving no self-service recovery path.

### Resolution

Removed the lost device's authenticator registration in Entra ID after identity verification, issued a temporary access pass for one-time sign-in, and had the user register a new device along with a backup MFA method (phone call) to prevent a repeat lockout scenario.

### User Communication (Ticket Notes)

Advised the user to report the lost phone to their carrier for remote wipe, and confirmed with security that no suspicious sign-in attempts had occurred on the account.

## Ticket INC0066178 — New Hire Account Not Provisioned Before Start Date

### Reported Symptom

Hiring manager reports a new employee starting tomorrow has no account, email, or system access provisioned, despite HR confirming the hire was submitted a week ago.

### Diagnostic Steps

Checked the HR system (Workday) to confirm the new hire record existed and had the correct start date and department assigned.

Reviewed the Entra ID provisioning connector logs for any failed sync jobs tied to that employee record.

Identified a provisioning error in the sync log related to a missing required field (cost center code) that blocked the automated account creation workflow.

Escalated to the HR/IAM provisioning team to correct the missing field and manually trigger the sync.

### Root Cause

The HR record was missing a required cost center field, which caused the automated Entra ID provisioning connector to silently fail validation and skip account creation, with no alert generated to notify HR or IT.

### Resolution

HR corrected the missing cost center field, and the IAM team manually triggered the provisioning sync, which created the account, mailbox, and baseline application access within 30 minutes. Verified login and MFA enrollment worked ahead of the start date.

### User Communication (Ticket Notes)

Recommended to the IAM team that a validation alert be added for incomplete HR records to prevent similar last-minute Day 1 issues in the future.

## Ticket INC0066215 — Offboarding — Access Not Fully Removed After Termination

### Reported Symptom

Security team flagged that a terminated employee's account was still able to authenticate to VPN two days after their official termination date, despite an offboarding ticket having been submitted.

### Diagnostic Steps

Reviewed the offboarding ticket and checklist to determine which steps had been completed versus skipped.

Checked Entra ID sign-in logs and confirmed the account had active sign-ins to VPN and email after the termination date/time.

Identified that the account had been disabled in Entra ID, but the on-premises VPN system authenticated against a separate legacy RADIUS server that hadn't received the disable sync.

Escalated to the network/security team to immediately force-disable the account on the RADIUS server directly.

### Root Cause

The offboarding automation only disabled the account in Entra ID and Microsoft 365, but the legacy VPN RADIUS server was not integrated into that automated workflow and required a separate manual disable step that was missed on the offboarding checklist.

### Resolution

Immediately disabled the account on the RADIUS server, confirmed no further VPN sign-ins occurred, and reviewed VPN session logs with security to confirm no unauthorized activity took place during the gap. Updated the offboarding checklist and automation scope to include the RADIUS server going forward.

### User Communication (Ticket Notes)

Escalated as a security finding per policy and documented the gap and remediation in the incident record for the post-termination access review.

## Ticket INC0066249 — Access Request to Department Shared Drive Denied

### Reported Symptom

User reports receiving an 'Access Denied' error when attempting to open the Finance department's shared drive, which they need for a new cross-team project.

### Diagnostic Steps

Confirmed the user's manager had approved the access request per the standard access-request approval workflow before taking any action.

Checked Active Directory to confirm the user's account was not already a member of the required security group.

Verified the correct security group name and associated NTFS permissions on the target shared drive path.

Added the user to the appropriate AD security group and confirmed replication completed across domain controllers.

### Root Cause

The user's manager had approved the access request, but the group membership had not yet been actioned by IT, which was a standard manual step in the access-provisioning workflow that had not been completed.

### Resolution

Added the user to the correct AD security group, forced a Group Policy update on the user's device, and confirmed folder access was restored after a fresh login. Access verified working within 20 minutes of ticket assignment.

### User Communication (Ticket Notes)

Confirmed with the user that access changes can take up to 15 minutes to apply after group membership updates due to AD replication.

## Ticket INC0066283 — VPN Connection Fails with Authentication Error

### Reported Symptom

Remote user reports VPN client fails to connect with an 'Authentication Failed' error, despite entering the correct password and completing MFA push approval.

### Diagnostic Steps

Confirmed the user's account was not locked or disabled in Entra ID/Active Directory.

Checked the VPN concentrator logs for the specific failure reason tied to the user's connection attempts.

Verified the user's device certificate (used for VPN client authentication) had not expired, since expired certs are a common silent-failure cause.

Had the user test from a different network (mobile hotspot) to rule out a local ISP or ports-blocked issue.

### Root Cause

The user's VPN client device certificate had expired the previous day, causing certificate-based authentication to fail even though the username, password, and MFA steps completed successfully.

### Resolution

Triggered a manual certificate renewal push through Intune and had the user restart their device to pick up the new certificate. Verified successful VPN connection afterward.

### User Communication (Ticket Notes)

Flagged the certificate auto-renewal policy to the network team, since renewal should occur automatically before expiration and did not in this case.
