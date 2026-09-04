<div align="center">

# IT Helpdesk Portfolio
### Email & Communication — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Email%20%26%20Communication-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-5-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-Exchange%20Online%20%7C%20Outlook%20%7C%20Defender-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases five representative incident tickets spanning the most common **email and communication** issues an enterprise helpdesk/messaging team encounters — from mail flow and quota failures to shared mailbox access, phishing response, and cross-device calendar sync.

Each ticket follows a standard **ServiceNow-style incident format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are **representative scenarios** built to demonstrate structured troubleshooting methodology — they are not records of actual incidents.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0056104`](#inc0056104--user-unable-to-send-or-receive-external-email) | User Unable to Send or Receive External Email | P2 | 50 min |
| 2 | [`INC0056148`](#inc0056148--mailbox-full--unable-to-send-or-receive) | Mailbox Full — Unable to Send or Receive | P3 | 25 min |
| 3 | [`INC0056192`](#inc0056192--user-cannot-access-shared-mailbox) | User Cannot Access Shared Mailbox | P3 | 20 min |
| 4 | [`INC0056231`](#inc0056231--suspicious-phishing-email-reported-by-user) | Suspicious Phishing Email Reported by User | P2 | 40 min (escalated) |
| 5 | [`INC0056274`](#inc0056274--calendar-not-syncing-between-mobile-and-desktop) | Calendar Not Syncing Between Mobile and Desktop | P4 | 20 min |

---

<br>

## `INC0056104` — User Unable to Send or Receive External Email

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Outlook (Microsoft 365), Exchange Online</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High</td></tr>
<tr><td><b>Category</b></td><td>Email &amp; Communication</td></tr>
<tr><td><b>Time to Resolve</b></td><td>50 minutes</td></tr>
</table>

**Reported Symptom**
User reports emails to external recipients are stuck in the Outbox, and no new external mail has arrived in over an hour. Internal mail flows normally.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed internal mail delivery worked, isolating the issue to external mail flow specifically.
2. Checked Exchange Online Message Trace for the stuck outbound messages and found they were being deferred with a queue error.
3. Reviewed the Exchange Online mail flow (transport) rules for recent changes affecting the user's domain or distribution group.
4. Checked the sending domain's reputation and SPF/DKIM/DMARC status using Microsoft's mail flow diagnostics.

</details>

**Root Cause**
A recently modified transport rule intended to quarantine a different department had an overly broad condition that inadvertently matched this user's outbound traffic, causing messages to queue instead of deliver.

**Resolution**
Escalated to the Exchange admin team, who corrected the transport rule condition to properly scope it to the intended department. Verified stuck messages in the queue delivered automatically once the rule was corrected, and confirmed new test emails sent/received normally.

> **User Communication (Ticket Notes)**
> *Apologized for the delay in outbound mail and confirmed with the user that all previously stuck messages had since delivered successfully.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0056148` — Mailbox Full — Unable to Send or Receive

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Outlook (Microsoft 365), Exchange Online</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Email &amp; Communication</td></tr>
<tr><td><b>Time to Resolve</b></td><td>25 minutes</td></tr>
</table>

**Reported Symptom**
User receives a bounce-back stating their mailbox is full and cannot receive new mail. Sent items are also failing with a quota-exceeded error.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Checked mailbox size and quota in the Exchange Admin Center against the assigned storage limit.
2. Confirmed the mailbox had exceeded its warning and prohibit-send/receive thresholds.
3. Reviewed the largest folders (Inbox, Sent Items, and a large personal archive folder) to identify what was consuming the most space.
4. Checked whether the user had access to online archive/auto-expanding archive as a longer-term solution.

</details>

**Root Cause**
The user had several years of large file attachments retained directly in Inbox and Sent Items with no archiving policy applied, gradually exceeding the assigned mailbox quota.

**Resolution**
Performed an immediate cleanup of large, duplicate attachments with user approval to drop the mailbox below the prohibit-send threshold, restoring mail flow. Enabled Online Archive for the mailbox and set up an auto-archive policy to move items older than 12 months going forward.

> **User Communication (Ticket Notes)**
> *Explained the new auto-archive policy to the user and confirmed they could still search and access archived mail through Outlook without any workflow change.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0056192` — User Cannot Access Shared Mailbox

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Outlook (Microsoft 365), Exchange Online</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Email &amp; Communication</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User reports the department's shared mailbox (support@company.com) no longer appears in their Outlook folder list, though it was accessible last week.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed in the Exchange Admin Center whether the user's account still had Full Access and Send As permissions assigned to the shared mailbox.
2. Checked the shared mailbox's permission change history for any recent modifications.
3. Verified the user's Outlook profile was set to auto-mount shared mailboxes the account has access to.
4. Had the user remove and re-add their Outlook profile connection to force a permissions refresh.

</details>

**Root Cause**
The user's Full Access permission to the shared mailbox had been inadvertently removed during a routine access review the prior week, when their name was mistakenly included in a bulk permission cleanup for departed employees.

**Resolution**
Re-added Full Access and Send As permissions for the user on the shared mailbox and had them restart Outlook to trigger a folder list refresh. Confirmed the shared mailbox reappeared and the user could send/receive on its behalf.

> **User Communication (Ticket Notes)**
> *Flagged the access review process to the team lead, since an active employee was mistakenly caught in a departed-employee cleanup.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0056231` — Suspicious Phishing Email Reported by User

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Outlook (Microsoft 365), Microsoft Defender for Office 365</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (security)</td></tr>
<tr><td><b>Category</b></td><td>Email &amp; Communication</td></tr>
<tr><td><b>Time to Resolve</b></td><td>40 minutes (escalated to SOC)</td></tr>
</table>

**Reported Symptom**
User reports receiving an email impersonating IT support, asking them to click a link and re-enter their credentials to "avoid account suspension." User did not click the link but reported it via the Report Phishing button.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reviewed the reported message headers and sender domain to confirm it did not originate from a legitimate internal or vendor mail source.
2. Checked Microsoft Defender for Office 365's Threat Explorer to determine whether the same message had been delivered to other mailboxes.
3. Used the reported URL to check against threat intelligence feeds for known phishing indicators.
4. Escalated to the SOC/security team with the message ID and sender details for a broader tenant-wide search and takedown request.

</details>

**Root Cause**
A credential-harvesting phishing campaign impersonating the internal IT support alias was sent to multiple users, exploiting a spoofed display name that closely resembled the legitimate IT support sender.

**Resolution**
SOC team confirmed 14 additional mailboxes received the same message and used Defender's mail purge capability to remove it tenant-wide. Blocked the sending domain and submitted the phishing URL for external takedown. No credentials were confirmed compromised.

> **User Communication (Ticket Notes)**
> *Thanked the user for reporting rather than clicking, and confirmed no further action was needed on their end. Reminded them Report Phishing is always the right first step for similar messages.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0056274` — Calendar Not Syncing Between Mobile and Desktop

<table>
<tr><td><b>Environment</b></td><td>iOS Outlook Mobile App + Windows 11 Outlook Desktop, Exchange Online</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low</td></tr>
<tr><td><b>Category</b></td><td>Email &amp; Communication</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User reports meetings created on their desktop Outlook are not appearing on their mobile Outlook app, and vice versa. Both apps are signed into the same account.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed both devices were authenticated to the same mailbox and not accidentally connected to a duplicate or cached account.
2. Checked mobile app sync settings to confirm calendar sync was enabled and not restricted by a Focused Inbox or selective sync setting.
3. Verified the mobile device's MDM compliance status in Intune, since non-compliant devices can have mailbox sync throttled.
4. Had the user force-close and reopen the mobile app, then manually triggered a sync from account settings.

</details>

**Root Cause**
The mobile device had fallen out of Intune compliance (outdated OS version) two days prior, which triggered a conditional access policy that silently throttled background mailbox sync without blocking sign-in entirely.

**Resolution**
Guided the user through updating their mobile OS to bring the device back into compliance. Confirmed compliance status updated in Intune within 15 minutes, and calendar sync resumed normally between both devices.

> **User Communication (Ticket Notes)**
> *Explained to the user that keeping their mobile OS current is required for continued mailbox access under the company's conditional access policy.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 5 |
| **High Priority (P2) Incidents** | 2 |
| **Medium Priority (P3) Incidents** | 2 |
| **Low Priority (P4) Incidents** | 1 |
| **Average Time to Resolve** | ~31 minutes |
| **Systems Touched** | Exchange Online · Outlook (Desktop &amp; Mobile) · Microsoft Defender for Office 365 · Intune · Exchange Admin Center |

## Skills Demonstrated

`Exchange Online Mail Flow & Transport Rules` · `Message Trace & Queue Diagnostics` · `Mailbox Quota & Archive Management` · `Shared Mailbox Permissions` · `Phishing Triage & Threat Explorer` · `SOC Escalation & Tenant-Wide Remediation` · `Conditional Access & Intune Compliance` · `Root Cause Analysis` · `ServiceNow-Style Incident Documentation` · `Cross-Team Escalation (Exchange Admin, SOC, Security)`

---

<div align="center">

*Built to demonstrate structured, repeatable troubleshooting methodology across the full email and communication stack — from mail flow to mailbox security.*

</div>
