<div align="center">

# IT Helpdesk Portfolio
### Hardware — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Hardware-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-6-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-Windows%2011%20%7C%20Dell%20%7C%20Lenovo%20%7C%20HP-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases six representative incident tickets spanning the most common **hardware** issues an enterprise helpdesk team encounters — from slow boot performance and printer connectivity to undetected peripherals, battery health, physical damage replacement, and docking station faults.

Each ticket follows a standard **ServiceNow-style incident format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are **representative scenarios** built to demonstrate structured troubleshooting methodology — they are not records of actual incidents.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0061102`](#inc0061102--laptop-extremely-slow-to-boot-and-load-applications) | Laptop Extremely Slow to Boot and Load Applications | P3 | 40 min |
| 2 | [`INC0061145`](#inc0061145--printer-shows-offline-and-will-not-print) | Printer Shows Offline and Will Not Print | P4 | 20 min |
| 3 | [`INC0061189`](#inc0061189--external-monitor-not-detected-through-docking-station) | External Monitor Not Detected Through Docking Station | P3 | 30 min |
| 4 | [`INC0061223`](#inc0061223--laptop-battery-draining-rapidly--not-holding-charge) | Laptop Battery Draining Rapidly / Not Holding Charge | P4 | 25 min (escalated) |
| 5 | [`INC0061267`](#inc0061267--hardware-replacement-request-after-liquid-damage) | Hardware Replacement Request After Liquid Damage | P2 | 1 hr (loaner same day) |
| 6 | [`INC0061301`](#inc0061301--docking-station-intermittently-disconnects-peripherals) | Docking Station Intermittently Disconnects Peripherals | P3 | 35 min |

---

<br>

## `INC0061102` — Laptop Extremely Slow to Boot and Load Applications

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Dell Latitude, SSD-equipped</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Hardware</td></tr>
<tr><td><b>Time to Resolve</b></td><td>40 minutes</td></tr>
</table>

**Reported Symptom**
User reports laptop takes over 8 minutes to reach the login screen and applications take an unusually long time to open afterward. Issue has worsened over the past week.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Checked Task Manager Startup tab and found an unusually high number of high-impact startup applications enabled.
2. Reviewed disk usage via Resource Monitor during boot and found sustained 100% disk activity for several minutes after login.
3. Ran a quick SMART status check on the drive to rule out failing hardware before pursuing a software fix.
4. Checked Windows Update history and confirmed a pending cumulative update had failed to complete on the last three attempts.

</details>

**Root Cause**
A stuck/failed Windows Update was repeatedly attempting to reapply on every boot, consuming disk I/O and CPU cycles during startup, compounded by an excessive number of unnecessary startup applications accumulated over time.

**Resolution**
Cleared the Windows Update cache (SoftwareDistribution folder) and manually reapplied the pending update successfully. Disabled unnecessary startup applications via Task Manager. Boot time returned to under 90 seconds on verification.

> **User Communication (Ticket Notes)**
> *Recommended the user periodically review startup apps and confirmed automatic updates were now completing normally going forward.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0061145` — Printer Shows Offline and Will Not Print

<table>
<tr><td><b>Environment</b></td><td>Windows 11, network printer (HP LaserJet), print server-managed</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low</td></tr>
<tr><td><b>Category</b></td><td>Hardware</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User reports the shared department printer shows "Offline" in the printer queue, and print jobs remain stuck without printing.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the printer had power and was showing a ready state on its physical display, ruling out a hardware fault.
2. Pinged the printer's IP address from the user's machine to check network reachability — no response.
3. Checked the print server for the printer's queue status and confirmed other users were also unable to print, pointing to a shared issue rather than one device.
4. Verified the printer's IP address hadn't changed due to a DHCP lease renewal, comparing it against the address configured in the print server's printer port.

</details>

**Root Cause**
The printer had received a new DHCP-assigned IP address after a lease renewal, but the print server's printer port was still pointing to the old, now-stale IP address, causing all print jobs from that queue to fail silently.

**Resolution**
Reserved a static DHCP lease for the printer to prevent future IP changes, then updated the print server's port configuration to the printer's current IP address. Verified test print completed successfully for the reporting user and two others in the queue.

> **User Communication (Ticket Notes)**
> *Notified the rest of the department that the printer had been restored and apologized for the delay in the queue clearing.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0061189` — External Monitor Not Detected Through Docking Station

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Lenovo ThinkPad, USB-C docking station, dual external monitors</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Hardware</td></tr>
<tr><td><b>Time to Resolve</b></td><td>30 minutes</td></tr>
</table>

**Reported Symptom**
User reports one of two external monitors connected through their docking station is not being detected after undocking and redocking the laptop. The second monitor works normally.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the affected monitor worked when connected directly to the laptop, ruling out a faulty monitor or cable.
2. Checked Windows Display Settings to see if the second monitor appeared as "Not Connected" or was missing entirely from the detected displays list.
3. Tested the affected monitor's video cable in the working port position on the dock to isolate whether the fault was port-specific.
4. Updated the docking station's firmware and the laptop's USB-C/Thunderbolt drivers, both of which were several versions out of date.

</details>

**Root Cause**
The specific display port on the docking station had an outdated firmware handshake issue with the laptop's GPU driver, causing intermittent failure to enumerate the second monitor after a fresh dock/undock cycle.

**Resolution**
Updated dock firmware and GPU/USB-C drivers to current versions, then power-cycled the dock. Verified both monitors were detected reliably across five consecutive dock/undock cycles before closing the ticket.

> **User Communication (Ticket Notes)**
> *Advised the user to keep the dock connected to power at all times, since firmware updates only apply while it's actively powered, and to report any recurrence immediately.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0061223` — Laptop Battery Draining Rapidly / Not Holding Charge

<table>
<tr><td><b>Environment</b></td><td>Windows 11, HP EliteBook, 3 years in service</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low</td></tr>
<tr><td><b>Category</b></td><td>Hardware</td></tr>
<tr><td><b>Time to Resolve</b></td><td>25 minutes (escalated for hardware replacement)</td></tr>
</table>

**Reported Symptom**
User reports laptop battery drops from 100% to under 20% in about 90 minutes of normal use, and the device shuts down unexpectedly even when the battery indicator shows remaining charge.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Ran the built-in battery report tool (powercfg /batteryreport) to compare the battery's design capacity against its current full charge capacity.
2. Checked Task Manager for background processes with unusually high power/CPU usage that could accelerate drain.
3. Confirmed the charger and charging cable were functioning correctly by testing with a known-good charger on another device.
4. Reviewed the battery report's cycle count against the manufacturer's expected lifespan for this laptop model.

</details>

**Root Cause**
The battery report showed the full charge capacity had degraded to roughly 45% of its original design capacity after three years of daily use, consistent with normal lithium-ion battery wear rather than a software or charging fault.

**Resolution**
Confirmed the device was still within its hardware refresh eligibility window and submitted a battery replacement request through the asset management team. Issued the user a loaner laptop in the interim to avoid workflow disruption.

> **User Communication (Ticket Notes)**
> *Explained the battery had reached end-of-life through normal wear and that a replacement was being processed, with an estimated turnaround based on parts availability.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0061267` — Hardware Replacement Request After Liquid Damage

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Dell Latitude, company-owned asset</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High (user unable to work)</td></tr>
<tr><td><b>Category</b></td><td>Hardware</td></tr>
<tr><td><b>Time to Resolve</b></td><td>1 hour (initial triage), loaner issued same day</td></tr>
</table>

**Reported Symptom**
User reports spilling coffee on their laptop keyboard. Device powers on but the keyboard and trackpad are unresponsive, and there is visible liquid residue near the hinge.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Had the user power off and unplug the device immediately to prevent further internal damage — did not attempt further troubleshooting on the live device given liquid exposure.
2. Logged the incident in the asset management system with damage description and photos for the hardware repair vendor.
3. Checked asset inventory for an available loaner device matching the user's role requirements (dock compatibility, software image).
4. Initiated the vendor RMA/repair process for the damaged unit under the applicable warranty or accidental damage coverage.

</details>

**Root Cause**
Physical liquid damage to the internal keyboard and trackpad components, unrelated to any software or configuration issue — a hardware repair/replacement case rather than a troubleshooting case.

**Resolution**
Issued a loaner laptop from the standby pool, imaged and ready within the same business day, and migrated the user's profile settings and OneDrive sync to minimize downtime. Submitted the damaged unit for vendor repair under warranty.

> **User Communication (Ticket Notes)**
> *Confirmed with the user that all files synced to OneDrive were unaffected and available immediately on the loaner device, and gave an estimated repair/return timeline for their original laptop.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0061301` — Docking Station Intermittently Disconnects Peripherals

<table>
<tr><td><b>Environment</b></td><td>Windows 11, Dell WD19 docking station, desk-mounted triple-monitor setup</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Hardware</td></tr>
<tr><td><b>Time to Resolve</b></td><td>35 minutes</td></tr>
</table>

**Reported Symptom**
User reports their docking station randomly disconnects all peripherals (monitors, keyboard, mouse, network) for a few seconds at a time, several times per day, then reconnects on its own.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Checked Windows Event Viewer for USB and display driver disconnect/reconnect events correlating with the reported drop times.
2. Verified the dock's power adapter was delivering full rated wattage using a multimeter, since underpowered docks commonly cause intermittent peripheral drops.
3. Tested the same dock on a different laptop to determine whether the fault followed the dock or the original laptop.
4. Checked USB-C cable and port for physical wear, and tried an alternate cable between the laptop and dock.

</details>

**Root Cause**
The dock's original power adapter had degraded and was intermittently under-delivering wattage under load, causing the dock to momentarily brown out and drop all connected peripherals before recovering.

**Resolution**
Replaced the docking station's power adapter with a new unit matching the manufacturer's rated wattage specification. Verified stability with the full peripheral load (three monitors, keyboard, mouse, wired network) over a full business day with no drops.

> **User Communication (Ticket Notes)**
> *Advised the user to report any further drops immediately in case the dock itself — not just the adapter — needs replacement under warranty.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 6 |
| **High Priority (P2) Incidents** | 1 |
| **Medium Priority (P3) Incidents** | 3 |
| **Low Priority (P4) Incidents** | 2 |
| **Average Time to Resolve** | ~35 minutes |
| **Systems Touched** | Windows 11 · Dell Latitude / WD19 Dock · Lenovo ThinkPad · HP EliteBook · Print Server · Asset Management System |

## Skills Demonstrated

`Boot & Performance Diagnostics` · `Windows Update Troubleshooting` · `Network Printer & DHCP/Port Configuration` · `Dock Firmware & Driver Management` · `Battery Health Analysis` · `Asset Lifecycle & Loaner Management` · `Physical Damage Triage & RMA Processing` · `Root Cause Analysis` · `ServiceNow-Style Incident Documentation` · `Cross-Team Coordination (Asset Management, Vendor Repair)`

---

<div align="center">

*Built to demonstrate structured, repeatable troubleshooting methodology across the full hardware lifecycle — from performance issues to physical failure and replacement.*

</div>
