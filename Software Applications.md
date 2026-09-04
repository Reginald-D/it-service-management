<div align="center">

# IT Helpdesk Portfolio
### Software & Applications — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Software%20%26%20Applications-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-6-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-Windows%2011%20%7C%20M365%20%7C%20SCCM-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases six representative incident tickets spanning the most common **software and application** issues an enterprise helpdesk team encounters — from application crashes and installation permissions to OS update compatibility, Outlook sync failures, browser certificate errors, and license activation problems.

Each ticket follows a standard **ServiceNow-style incident format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are **representative scenarios** built to demonstrate structured troubleshooting methodology — they are not records of actual incidents.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0051104`](#inc0051104--application-repeatedly-crashesfreezes-on-launch) | Application Repeatedly Crashes/Freezes on Launch | P3 | 35 min |
| 2 | [`INC0051147`](#inc0051147--software-installation-blocked-by-insufficient-permissions) | Software Installation Blocked by Insufficient Permissions | P4 | 20 min |
| 3 | [`INC0051189`](#inc0051189--os-update-causes-application-compatibility-failure) | OS Update Causes Application Compatibility Failure | P2 | 1 hr 30 min (escalated) |
| 4 | [`INC0051212`](#inc0051212--outlook-not-syncing-new-emails) | Outlook Not Syncing New Emails | P3 | 25 min |
| 5 | [`INC0051255`](#inc0051255--browser-blocking-internal-site-with-certificate-error) | Browser Blocking Internal Site with Certificate Error | P3 | 30 min |
| 6 | [`INC0051298`](#inc0051298--license-activation-error-after-reimage) | License Activation Error After Reimage | P3 | 20 min |

---

<br>

## `INC0051104` — Application Repeatedly Crashes/Freezes on Launch

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Adobe Acrobat Pro DC</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Software &amp; Applications</td></tr>
<tr><td><b>Time to Resolve</b></td><td>35 minutes</td></tr>
</table>

**Reported Symptom**
User reports Adobe Acrobat freezes for 30+ seconds on launch, then crashes to desktop with no error message. Issue started after a recent Windows update.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reviewed Windows Event Viewer (Application log) and identified repeated Application Error events pointing to a faulting module tied to a third-party plugin DLL.
2. Confirmed the crash was reproducible in a clean local admin profile, ruling out user-profile corruption.
3. Checked installed application version against the vendor's compatibility notes for the recent Windows feature update.
4. Disabled non-essential Acrobat plugins/add-ins one at a time to isolate the conflicting component.

</details>

**Root Cause**
A third-party PDF plugin (a legacy print-to-PDF add-in) was incompatible with the new Windows build and was crashing the host application on startup.

**Resolution**
Removed the outdated plugin and reinstalled the current Acrobat release from the approved software catalog. Verified stable launch and normal operation over three consecutive test opens.

> **User Communication (Ticket Notes)**
> *Advised user to report any further crashes immediately and avoided reinstalling the legacy plugin, since a supported alternative is available in the software catalog.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0051147` — Software Installation Blocked by Insufficient Permissions

<table>
<tr><td><b>Environment</b></td><td>Windows 11, standard (non-admin) user account, SCCM-managed</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low</td></tr>
<tr><td><b>Category</b></td><td>Software &amp; Applications</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User attempted to install a department-approved application from a shared installer link and received an "Administrator permissions required" error.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the user's account is a standard (non-admin) local account per company security baseline.
2. Checked whether the application was available for self-service install through the SCCM Software Center.
3. Verified the user's device was in the correct SCCM collection to receive the deployment.
4. Confirmed license availability for the requested title before proceeding with install.

</details>

**Root Cause**
The application was not yet pushed to the user's device collection in SCCM, so it did not appear in Software Center, prompting the user to attempt a manual install that required elevated rights they don't have by design.

**Resolution**
Added the device to the correct SCCM deployment collection and triggered a manual policy refresh. Application appeared in Software Center within 10 minutes and installed successfully without requiring elevated credentials.

> **User Communication (Ticket Notes)**
> *Explained to user why manual installs are blocked for standard accounts and pointed them to Software Center for future self-service requests.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0051189` — OS Update Causes Application Compatibility Failure

<table>
<tr><td><b>Environment</b></td><td>Windows 11 23H2, line-of-business finance application</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (business impact)</td></tr>
<tr><td><b>Category</b></td><td>Software &amp; Applications</td></tr>
<tr><td><b>Time to Resolve</b></td><td>1 hour 30 minutes (escalated)</td></tr>
</table>

**Reported Symptom**
Following an overnight OS feature update, a critical internal finance application fails to open, throwing a .NET runtime error on launch. Multiple users on the same update ring affected.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the issue was isolated to devices that received the latest Windows feature update by comparing build numbers across affected vs. unaffected machines.
2. Reviewed the .NET runtime error code and cross-referenced it against the application vendor's known-issues page.
3. Checked Windows Update history to confirm the specific KB update correlated with the failure timing.
4. Escalated to the application owner/vendor support with error logs and OS build details for compatibility confirmation.

</details>

**Root Cause**
The Windows feature update changed a .NET Framework dependency version that the finance application had not yet been certified against, breaking a core runtime component the app relied on.

**Resolution**
Vendor confirmed a compatibility patch was in progress; in the interim, paused the Windows update ring for the affected device group and applied a documented registry workaround to restore the required .NET behavior. Rolled out the vendor patch once released and resumed the update ring.

> **User Communication (Ticket Notes)**
> *Sent a mass notification to affected users explaining the temporary workaround and expected timeline for the permanent vendor fix.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0051212` — Outlook Not Syncing New Emails

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Outlook (Microsoft 365), Exchange Online</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Software &amp; Applications</td></tr>
<tr><td><b>Time to Resolve</b></td><td>25 minutes</td></tr>
</table>

**Reported Symptom**
User reports Outlook desktop client has not received new emails in over an hour, though mail is confirmed arriving via Outlook Web Access (OWA).

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed new mail was visible in OWA, isolating the issue to the desktop client/local profile rather than the mailbox itself.
2. Checked Outlook's connection status indicator in the bottom status bar — showed "Disconnected" intermittently.
3. Ran Outlook in Safe Mode to rule out a conflicting add-in.
4. Verified the local OST file size against Microsoft's known size-threshold issues, and checked for a stuck Send/Receive process in Task Manager.

</details>

**Root Cause**
The local Outlook OST cache file had grown near its performance threshold, causing sync operations to silently stall without generating a visible error.

**Resolution**
Closed Outlook, renamed the existing OST file to force a clean regeneration, and reopened Outlook to trigger a fresh sync. Mail began flowing normally within 15 minutes as the new OST rebuilt.

> **User Communication (Ticket Notes)**
> *Advised user this may recur if mailbox size continues to grow, and recommended archiving older mail to keep the local cache smaller going forward.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0051255` — Browser Blocking Internal Site with Certificate Error

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Google Chrome, internal HR portal</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Software &amp; Applications</td></tr>
<tr><td><b>Time to Resolve</b></td><td>30 minutes</td></tr>
</table>

**Reported Symptom**
User receives "Your connection is not private" (NET::ERR_CERT_AUTHORITY_INVALID) error when accessing the internal HR portal in Chrome. Site loads fine in Edge.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the error was browser-specific by testing the same URL in Edge and Firefox — only Chrome failed.
2. Checked Chrome's certificate trust store and compared it against the internal Root CA certificate installed via Group Policy.
3. Verified the internal Root CA certificate was present in the Windows Trusted Root store but not yet picked up by Chrome's certificate cache.
4. Cleared Chrome's SSL state and restarted the browser to force a certificate re-check.

</details>

**Root Cause**
Chrome had cached an outdated certificate trust state from before the internal Root CA was deployed via Group Policy, and had not yet refreshed against the updated Windows certificate store.

**Resolution**
Cleared Chrome's SSL state (chrome://net-internals/#hsts and browsing data cache) and restarted the browser. Confirmed the HR portal loaded without a certificate warning afterward.

> **User Communication (Ticket Notes)**
> *Noted for the team that this issue may recur fleet-wide after the recent internal CA rollout and flagged it as a candidate for a scripted fix.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0051298` — License Activation Error After Reimage

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Microsoft 365 Apps for Enterprise</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Software &amp; Applications</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User's laptop was reimaged after a hardware repair. Office apps now show "Unlicensed Product" banner and features are restricted, despite the user having an active license assignment.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed in the Microsoft 365 Admin Center that the user's account still had an active Office license assignment.
2. Checked the device's activation status via Office application (File > Account) — showed "Unlicensed Product."
3. Verified the device was properly re-enrolled in Azure AD/Entra ID and Intune after the reimage, since licensing activation depends on device and account trust.
4. Attempted a manual sign-out/sign-in cycle in the Office apps to force re-validation against the license server.

</details>

**Root Cause**
The reimage process reset the device's local activation token, and the device had not yet completed re-enrollment in Entra ID at the time Office first attempted to validate the license, leaving it in an unlicensed state.

**Resolution**
Completed Entra ID re-enrollment, then ran "ospp.vbs /act" from an elevated command prompt to force re-activation against the organization's license server. Confirmed all Office apps returned to fully licensed status.

> **User Communication (Ticket Notes)**
> *Added a step to the standard reimage checklist to confirm Entra ID enrollment completes before first Office launch, to prevent this recurring for other reimaged devices.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 6 |
| **High Priority (P2) Incidents** | 1 |
| **Medium Priority (P3) Incidents** | 4 |
| **Low Priority (P4) Incidents** | 1 |
| **Average Time to Resolve** | ~37 minutes |
| **Systems Touched** | Windows 11 · Microsoft 365 Apps · SCCM Software Center · Exchange Online/Outlook · Google Chrome · Entra ID/Intune |

## Skills Demonstrated

`Application Crash Diagnostics (Event Viewer)` · `SCCM Software Center & Deployment Collections` · `OS Update Compatibility Triage` · `Outlook OST Cache Troubleshooting` · `Certificate Trust Store & SSL State Management` · `Microsoft 365 License Activation` · `Root Cause Analysis` · `ServiceNow-Style Incident Documentation` · `Cross-Team Escalation (Vendor Support, Application Owners)`

---

<div align="center">

*Built to demonstrate structured, repeatable troubleshooting methodology across the full software and application stack — from local client issues to OS-level compatibility.*

</div>
