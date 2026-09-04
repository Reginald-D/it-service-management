<div align="center">

# IT Helpdesk Portfolio
### Screen Share Not Working in Meetings — Ticket Documentation

![Scenario](https://img.shields.io/badge/Scenario-Screen%20Share%20Failures-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-5-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-Microsoft%20Teams%20%7C%20Zoom%20%7C%20VPN-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases five representative incident tickets spanning the most common **screen-sharing failures** across Microsoft Teams and Zoom — from policy-driven and GPU-driver conflicts to audio-sharing gaps, external call breakage, and VPN-induced freezing mid-meeting.

Each ticket follows a standard **ServiceNow-style incident format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are **representative scenarios** built to demonstrate structured troubleshooting methodology — they are not records of actual incidents.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0045231`](#inc0045231--screen-share-button-grayed-out-in-teams) | Screen Share Button Grayed Out in Teams | P3 | 45 min (escalated) |
| 2 | [`INC0045298`](#inc0045298--attendees-see-black-screen-during-share) | Attendees See Black Screen During Share | P3 | 30 min |
| 3 | [`INC0045312`](#inc0045312--no-system-audio-shared-during-video-presentation) | No System Audio Shared During Video Presentation | P4 | 10 min |
| 4 | [`INC0045356`](#inc0045356--screen-share-fails-only-on-external-client-calls) | Screen Share Fails Only on External Client Calls | P2 | 1 hr 15 min (escalated) |
| 5 | [`INC0045389`](#inc0045389--screen-share-freezes-mid-meeting) | Screen Share Freezes Mid-Meeting | P3 | 40 min |

---

<br>

## `INC0045231` — Screen Share Button Grayed Out in Teams

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Microsoft Teams (New), Intune-managed device</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Collaboration / Conferencing</td></tr>
<tr><td><b>Time to Resolve</b></td><td>45 minutes (escalated)</td></tr>
</table>

**Reported Symptom**
User reports the Share button in Teams meetings is grayed out and unresponsive. Confirmed camera and microphone function normally.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reproduced issue on a loaner device using the same user account — confirmed issue follows the user, not the machine.
2. Reviewed Intune device compliance and DLP policy assignments for the user's security group.
3. Checked Teams Admin Center under Meeting Policies and confirmed the assigned policy had Screen Sharing Mode set to Disabled.
4. Cross-referenced recent policy change log — DLP policy was updated two days prior by the security team.

</details>

**Root Cause**
A newly applied Data Loss Prevention (DLP) policy restricted screen sharing for the user's department group as part of a broader security policy rollout, unrelated to any device fault.

**Resolution**
Escalated to the Security/IAM team with policy ID and user group details. Security team approved an exception and applied it to the correct policy group. Verified screen share functioned after policy sync (up to 24 hours, expedited via forced sync).

> **User Communication (Ticket Notes)**
> *Advised user this was a security policy change, not a device issue, and set expectation for policy sync delay before closing the ticket.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0045298` — Attendees See Black Screen During Share

<table>
<tr><td><b>Environment</b></td><td>Windows 11, dual-GPU laptop (Intel + NVIDIA), Teams</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Collaboration / Conferencing</td></tr>
<tr><td><b>Time to Resolve</b></td><td>30 minutes</td></tr>
</table>

**Reported Symptom**
Presenter's own display looks fine, but all meeting attendees report seeing a black or frozen screen during share.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the laptop has a dual-GPU configuration (integrated + discrete graphics), a known conflict point for screen capture APIs.
2. Checked NVIDIA and Intel graphics driver versions against vendor's latest release — drivers were three versions behind.
3. Tested sharing a single application window instead of the full desktop to isolate a compositing-layer issue.
4. Toggled Teams' GPU hardware acceleration setting off, then re-tested share.

</details>

**Root Cause**
Outdated discrete GPU driver was conflicting with Teams' hardware-accelerated screen capture, causing the captured frame buffer to render black on the receiving end.

**Resolution**
Updated GPU drivers via vendor utility, restarted the device, and disabled GPU hardware acceleration in Teams as a secondary safeguard. Verified with a test call to a second technician — screen share rendered correctly.

> **User Communication (Ticket Notes)**
> *Recommended user keep GPU drivers on auto-update going forward to prevent recurrence.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0045312` — No System Audio Shared During Video Presentation

<table>
<tr><td><b>Environment</b></td><td>Windows 10, Zoom Desktop Client</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low</td></tr>
<tr><td><b>Category</b></td><td>Collaboration / Conferencing</td></tr>
<tr><td><b>Time to Resolve</b></td><td>10 minutes</td></tr>
</table>

**Reported Symptom**
User sharing a training video during a client call; attendees could see the video but heard no audio.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed with user whether Share Computer Sound was enabled in the Zoom share dialog — it was not.
2. Checked Windows Sound Settings > App volume and device preferences to confirm Zoom was not muted at the OS level.
3. Had user re-share with Share Computer Sound checkbox enabled and replay a short clip to confirm fix.

</details>

**Root Cause**
User did not enable the Share Computer Sound option, which is off by default and must be explicitly selected each time system audio needs to be included in a share.

**Resolution**
Walked user through enabling the setting live on the call and confirmed audio was audible to attendees. No further action required.

> **User Communication (Ticket Notes)**
> *Sent a one-page quick reference to the user showing where the Share Computer Sound toggle is located in Zoom for future self-service.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0045356` — Screen Share Fails Only on External Client Calls

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Teams, Corporate VPN, Zscaler proxy</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (client-facing)</td></tr>
<tr><td><b>Category</b></td><td>Collaboration / Conferencing</td></tr>
<tr><td><b>Time to Resolve</b></td><td>1 hour 15 minutes (escalated to Network team)</td></tr>
</table>

**Reported Symptom**
User can screen share successfully in internal meetings but the share fails to load for external/guest participants on client calls.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed internal meetings shared without issue, isolating the problem to external/federated calls specifically.
2. Checked Teams Admin Center > External Access to confirm the client's domain was not on a blocked list.
3. Reviewed firewall rules for required Teams media port ranges (UDP 3478-3481, TCP 443) to confirm they were not being blocked outbound.
4. Tested the same call from a mobile hotspot, bypassing the corporate network entirely — screen share worked normally.
5. Escalated to Network team to review Zscaler SSL inspection rules applied to Teams media traffic.

</details>

**Root Cause**
Corporate proxy (Zscaler) was applying SSL inspection to Teams media streams for external calls, breaking the encrypted media path required for screen share to render on the receiving end.

**Resolution**
Network team added a Teams media traffic exclusion to the Zscaler SSL inspection policy per Microsoft's recommended bypass list. Verified fix with a follow-up external test call.

> **User Communication (Ticket Notes)**
> *Documented the Zscaler exclusion in the network team's knowledge base to prevent repeat tickets from other users on external calls.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0045389` — Screen Share Freezes Mid-Meeting

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Teams, Corporate VPN (full-tunnel)</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Collaboration / Conferencing</td></tr>
<tr><td><b>Time to Resolve</b></td><td>40 minutes</td></tr>
</table>

**Reported Symptom**
Screen share starts normally but freezes for attendees a few minutes into the call. Presenter's own screen appears unaffected.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Checked Task Manager during a live test call — CPU and memory usage were within normal range, ruling out local resource exhaustion.
2. Confirmed user was connected to corporate VPN in full-tunnel mode, routing all traffic including meeting media through the tunnel.
3. Ran a bandwidth test during a reproduced freeze — throughput dropped significantly during the freeze window, consistent with VPN tunnel congestion.
4. Reviewed VPN client configuration for split-tunneling support for Teams media traffic.

</details>

**Root Cause**
Full-tunnel VPN configuration was routing high-bandwidth meeting media traffic through the VPN tunnel instead of directly to Microsoft's network, causing congestion-related freezing during longer shares.

**Resolution**
Enabled split tunneling for Teams-related IP ranges per Microsoft's published optimization guidance, in coordination with the network team. Verified with a 20-minute test share with no freezing.

> **User Communication (Ticket Notes)**
> *Flagged this as a candidate for a broader split-tunneling rollout, since other remote users on full-tunnel VPN are likely affected by the same issue.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 5 |
| **High Priority (P2) Incidents** | 1 |
| **Medium Priority (P3) Incidents** | 3 |
| **Low Priority (P4) Incidents** | 1 |
| **Average Time to Resolve** | ~46 minutes |
| **Systems Touched** | Microsoft Teams · Zoom · Intune · Teams Admin Center · Zscaler Proxy · Corporate VPN |

## Skills Demonstrated

`Teams Admin Center & Meeting Policy Configuration` · `DLP Policy Escalation` · `GPU Driver & Hardware Acceleration Troubleshooting` · `Zoom Audio Sharing Configuration` · `SSL Inspection & Proxy Bypass Rules` · `VPN Split-Tunneling Optimization` · `Root Cause Analysis` · `ServiceNow-Style Incident Documentation` · `Cross-Team Escalation (Security/IAM, Network Team)`

---

<div align="center">

*Built to demonstrate structured, repeatable troubleshooting methodology across the full collaboration and conferencing stack — from client-side settings to network and security policy.*

</div>
