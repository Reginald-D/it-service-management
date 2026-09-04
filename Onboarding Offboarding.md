<div align="center">

# IT Helpdesk Portfolio
### Onboarding/Offboarding — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Onboarding%2FOffboarding-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-4-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-SCCM%20%7C%20Entra%20ID%20%7C%20Intune-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases four representative incident/process tickets spanning the most common **onboarding and offboarding** tasks an enterprise IT team handles — from new equipment imaging and multi-system account provisioning to software bundle deployment and end-to-end equipment return with data sanitization.

Each ticket follows a standard **ServiceNow-style format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are **representative scenarios** built to demonstrate structured process execution and troubleshooting methodology — they are not records of actual incidents.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0076104`](#inc0076104--new-equipment-imaging-fails-to-complete) | New Equipment Imaging Fails to Complete | P3 | 40 min |
| 2 | [`INC0076148`](#inc0076148--new-hire-missing-access-to-two-of-four-required-saas-applications) | New Hire Missing Access to Two of Four Required SaaS Applications | P2 | 35 min |
| 3 | [`INC0076183`](#inc0076183--standard-software-bundle-fails-to-deploy-to-new-device) | Standard Software Bundle Fails to Deploy to New Device | P3 | 30 min |
| 4 | [`INC0076217`](#inc0076217--offboarding--equipment-return-and-data-wipe-confirmation) | Offboarding — Equipment Return and Data Wipe Confirmation | P3 | 20 min (tracked to completion) |

---

<br>

## `INC0076104` — New Equipment Imaging Fails to Complete

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Dell laptop, SCCM/MDT imaging task sequence</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Onboarding/Offboarding</td></tr>
<tr><td><b>Time to Resolve</b></td><td>40 minutes</td></tr>
</table>

**Reported Symptom**
New laptop being prepared for an upcoming hire fails partway through the automated imaging task sequence, hanging at the driver injection step with no error visible on screen.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reviewed the SCCM task sequence log (smsts.log) on the target machine to identify exactly which step the sequence stalled on.
2. Confirmed the laptop model's driver package had been imported into the driver library and was mapped to the correct task sequence step.
3. Checked whether the specific hardware model was newer than the last driver package update, since new hardware revisions commonly lack matching driver packages.
4. Tested imaging a second unit of the same model to confirm the failure was model-specific rather than a one-off hardware fault.

</details>

**Root Cause**
The laptop was a newer hardware revision that shipped with an updated Wi-Fi/network adapter not covered by the currently imported driver package, causing the task sequence to hang while waiting for network connectivity to resume mid-sequence.

**Resolution**
Downloaded and imported the updated driver pack from the vendor's support site into the SCCM driver library and added it to the task sequence's driver selection step. Re-ran the sequence successfully on both test units.

> **User Communication (Ticket Notes)**
> *Documented the new hardware revision and driver pack in the imaging team's internal notes so future units of this model image without delay.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0076148` — New Hire Missing Access to Two of Four Required SaaS Applications

<table>
<tr><td><b>Environment</b></td><td>Entra ID, Microsoft 365, Salesforce, Slack, SSO-integrated SaaS apps</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (Day 1 readiness)</td></tr>
<tr><td><b>Category</b></td><td>Onboarding/Offboarding</td></tr>
<tr><td><b>Time to Resolve</b></td><td>35 minutes</td></tr>
</table>

**Reported Symptom**
New hire's Active Directory account, email, and Slack were created successfully, but they report being unable to sign into Salesforce or the department's project management tool on their first day.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the user's Entra ID account and group memberships matched the standard onboarding template for their department/role.
2. Checked whether the two missing applications used SSO tied to Entra ID group membership versus a separate manual provisioning process.
3. Found Salesforce provisioning required a separate manual license assignment step outside the automated AD/email/Slack provisioning workflow.
4. Verified license availability in the Salesforce admin console before assigning.

</details>

**Root Cause**
The onboarding automation only covered AD, email, and Slack provisioning; Salesforce and the project management tool required manual license assignment steps that were not included on the onboarding checklist used by the requesting manager.

**Resolution**
Manually assigned Salesforce and project management tool licenses to the new hire and verified successful login to both. Updated the department's onboarding checklist template to include these two applications as explicit manual steps.

> **User Communication (Ticket Notes)**
> *Apologized for the Day 1 gap and confirmed the new hire had full access to all required systems within the first two hours of their start.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0076183` — Standard Software Bundle Fails to Deploy to New Device

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Intune-managed device, standard department app bundle</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Onboarding/Offboarding</td></tr>
<tr><td><b>Time to Resolve</b></td><td>30 minutes</td></tr>
</table>

**Reported Symptom**
A newly provisioned laptop for a new hire is not receiving the department's standard software bundle (Office, VPN client, department line-of-business app) through Intune, despite the device showing as enrolled and compliant.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the device appeared as enrolled and compliant in the Intune console, ruling out an enrollment failure.
2. Checked the device's assigned Azure AD group membership against the group targeted by the software bundle deployment policy.
3. Reviewed the Intune deployment status for the app bundle and found the device was not listed as a target at all.
4. Checked the dynamic group membership rule used to target the deployment policy for any recent rule changes.

</details>

**Root Cause**
The dynamic Azure AD group used to target the software bundle deployment was scoped by a device naming convention attribute, and the new laptop had been provisioned with a naming convention typo that excluded it from matching the dynamic group's membership rule.

**Resolution**
Corrected the device name to match the required naming convention and forced a group membership evaluation, which added the device to the correct dynamic group within a few minutes. Triggered a manual sync in Intune, and the full software bundle deployed successfully.

> **User Communication (Ticket Notes)**
> *Flagged the device-naming step in the imaging process as a common point of failure and suggested adding a naming convention validation check to the imaging task sequence.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0076217` — Offboarding — Equipment Return and Data Wipe Confirmation

<table>
<tr><td><b>Environment</b></td><td>Windows 11, company-owned laptop, remote employee</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Onboarding/Offboarding</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes (initial processing); tracked to completion over return shipping window</td></tr>
</table>

**Reported Symptom**
HR submitted an offboarding ticket for a remote employee's last day, including equipment return and required data wipe/sanitization before the device re-enters inventory.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the offboarding ticket included all required fields (return shipping label generation, device asset tag, and data sensitivity classification for wipe method).
2. Triggered a remote selective wipe of corporate data via Intune before the device shipped back, as an initial safeguard in case the device was delayed in transit.
3. Generated a prepaid return shipping label and sent it to the employee with return instructions and a deadline per company offboarding policy.
4. Upon device receipt, verified the asset tag matched inventory records and initiated a full drive wipe using the organization's approved sanitization standard before reissuing the device.

</details>

**Root Cause**
Not applicable — this is a standard offboarding process ticket rather than a fault-driven incident; documented here to demonstrate the end-to-end equipment return and data sanitization workflow.

**Resolution**
Completed the remote selective wipe immediately upon offboarding, tracked the device through return shipping, verified receipt against asset inventory, and performed a full disk sanitization per policy before the device was marked available for reissue.

> **User Communication (Ticket Notes)**
> *Confirmed with HR and the employee's manager that all corporate data access had been revoked and the device fully returned and processed within the standard offboarding window.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 4 |
| **High Priority (P2) Incidents** | 1 |
| **Medium Priority (P3) Incidents** | 3 |
| **Average Time to Resolve** | ~31 minutes |
| **Systems Touched** | SCCM/MDT · Entra ID · Microsoft 365 · Salesforce · Slack · Intune · Asset Inventory |

## Skills Demonstrated

`SCCM/MDT Imaging & Driver Management` · `Multi-System Account Provisioning (AD, Email, SSO, SaaS)` · `Intune App Deployment & Dynamic Group Targeting` · `Onboarding Checklist Process Improvement` · `Remote Wipe & Data Sanitization` · `Asset Lifecycle Management` · `Root Cause Analysis` · `ServiceNow-Style Ticket Documentation` · `Cross-Team Coordination (HR, Imaging, IAM)`

---

<div align="center">

*Built to demonstrate structured, repeatable process execution across the full employee lifecycle — from Day 1 provisioning to secure offboarding.*

</div>
