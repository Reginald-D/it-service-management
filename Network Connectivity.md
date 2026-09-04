<div align="center">

# IT Helpdesk Portfolio
### Network & Connectivity — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Network%20%26%20Connectivity-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-5-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20Style-informational?style=for-the-badge)
![Environment](https://img.shields.io/badge/Environment-Wi--Fi%20%7C%20LAN%20%7C%20DNS%20%7C%20DHCP-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases five representative incident tickets spanning the most common **network and connectivity** issues an enterprise helpdesk/network team encounters — from Wi-Fi drops and localized performance degradation to isolating outage scope, mapped drive failures, and DHCP/IP configuration faults.

Each ticket follows a standard **ServiceNow-style incident format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are representative scenarios built to demonstrate structured troubleshooting methodology and confirm real world experiences. They are not records of actual incidents to keep the integrity and confidentiality of our clients and employees.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0071106`](#inc0071106--laptop-repeatedly-drops-wi-fi-connection) | Laptop Repeatedly Drops Wi-Fi Connection | P3 | 30 min |
| 2 | [`INC0071149`](#inc0071149--persistent-slow-network-performance-in-one-office-area) | Persistent Slow Network Performance in One Office Area | P3 | 1 hr 10 min (escalated) |
| 3 | [`INC0071183`](#inc0071183--no-internet-access--isolating-local-vs-isp-vs-network-cause) | No Internet Access — Isolating Local vs. ISP vs. Network Cause | P1 | 45 min (escalated) |
| 4 | [`INC0071227`](#inc0071227--mapped-network-drive-not-accessible) | Mapped Network Drive Not Accessible | P3 | 20 min |
| 5 | [`INC0071261`](#inc0071261--device-receiving-incorrect-ip-configuration-apipa-address) | Device Receiving Incorrect IP Configuration (APIPA Address) | P3 | 25 min |

---

<br>

## `INC0071106` — Laptop Repeatedly Drops Wi-Fi Connection

<table>
<tr><td><b>Environment</b></td><td>Windows 11, corporate Wi-Fi (WPA2-Enterprise), Dell Latitude</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Network &amp; Connectivity</td></tr>
<tr><td><b>Time to Resolve</b></td><td>30 minutes</td></tr>
</table>

**Reported Symptom**
User reports their laptop disconnects from Wi-Fi every 10-15 minutes throughout the day, requiring a manual reconnect each time. Other users nearby are not reporting the same issue.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the issue was device-specific by checking whether coworkers on the same access point were experiencing drops — they were not.
2. Reviewed Windows Event Viewer for WLAN-AutoConfig disconnect events and captured the disconnect reason codes.
3. Checked the wireless adapter's power management settings, since Windows allowing the OS to power down the adapter to save battery is a frequent cause of intermittent drops.
4. Updated the wireless adapter driver, which was several versions behind the vendor's current release.

</details>

**Root Cause**
Windows power management was allowed to turn off the wireless adapter to conserve battery, causing periodic disconnects that coincided with the laptop's idle power-saving cycle.

**Resolution**
Disabled "Allow the computer to turn off this device to save power" for the wireless adapter and updated the adapter driver to the latest version. Verified stable connection with no drops over a 2-hour monitored period.

> **User Communication (Ticket Notes)**
> *Explained to the user that this setting change may slightly reduce battery life on battery power, which is an expected trade-off for connection stability.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0071149` — Persistent Slow Network Performance in One Office Area

<table>
<tr><td><b>Environment</b></td><td>Windows 11 fleet, wired and Wi-Fi, single office floor</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Network &amp; Connectivity</td></tr>
<tr><td><b>Time to Resolve</b></td><td>1 hour 10 minutes (escalated to network team)</td></tr>
</table>

**Reported Symptom**
Multiple users on the east side of the third floor report slow file transfers, laggy video calls, and general network sluggishness, while users elsewhere in the building report normal performance.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the issue was localized to a specific physical area rather than account- or application-specific by checking with unaffected users on other floors.
2. Ran wired and wireless throughput tests from multiple affected workstations against an internal file server to quantify the slowdown.
3. Checked switch port utilization and error counters on the access switch serving that section of the floor.
4. Reviewed the wireless access point's client count and channel utilization for that zone, since the area also had unusually high Wi-Fi client density.

</details>

**Root Cause**
An access switch serving that section of the floor had a partially failing uplink port showing a high rate of CRC errors, causing packet retransmissions and effective throughput well below normal for both wired and Wi-Fi clients routed through it.

**Resolution**
Escalated to the network team, who replaced the faulting uplink cable and moved the connection to a spare switch port. Throughput tests returned to baseline (matching unaffected floors) within 20 minutes of the fix.

> **User Communication (Ticket Notes)**
> *Notified the affected users once performance was restored and asked them to report if slowness returned, in case a second failing component was involved.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0071183` — No Internet Access — Isolating Local vs. ISP vs. Network Cause

<table>
<tr><td><b>Environment</b></td><td>Windows 11, small branch office, single ISP circuit</td></tr>
<tr><td><b>Priority</b></td><td>P1 – Critical (site-wide outage)</td></tr>
<tr><td><b>Category</b></td><td>Network &amp; Connectivity</td></tr>
<tr><td><b>Time to Resolve</b></td><td>45 minutes (escalated to ISP)</td></tr>
</table>

**Reported Symptom**
All users at a branch office report no internet access. Internal file shares and printers on the local network are still reachable.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed internal (LAN) resources such as local file shares and printers were still reachable, isolating the issue to the internet-facing path rather than the internal network.
2. Attempted to ping the default gateway from a workstation — successful, ruling out a local switch/cabling failure.
3. Attempted to ping the ISP-provided router/modem interface directly, and then a public IP address (bypassing DNS) to isolate DNS from a broader routing failure.
4. Checked the ISP circuit's status lights on the modem/router and confirmed a loss-of-signal indicator was active.

</details>

**Root Cause**
The ISP's fiber circuit serving the branch office had an upstream outage, confirmed by the loss-of-signal indicator on the local ISP-provided equipment — not a local network or configuration fault.

**Resolution**
Opened a circuit outage ticket with the ISP using their support line and referenced the account's circuit ID. ISP confirmed a regional fiber cut and restored service after their field repair completed. Verified full internet access for all branch users afterward.

> **User Communication (Ticket Notes)**
> *Kept the branch office updated with the ISP's outage status and estimated restoration time throughout, since local troubleshooting could not resolve an upstream carrier issue.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0071227` — Mapped Network Drive Not Accessible

<table>
<tr><td><b>Environment</b></td><td>Windows 11, on-premises file server, mapped drive via Group Policy</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Network &amp; Connectivity</td></tr>
<tr><td><b>Time to Resolve</b></td><td>20 minutes</td></tr>
</table>

**Reported Symptom**
User reports their mapped "S: drive" shows a red X in File Explorer and returns "network path not found" when clicked, though it worked yesterday.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Confirmed the user could reach other network resources (intranet, other mapped drives) to rule out a full network outage.
2. Attempted to reach the file server by hostname and then by IP address directly from the user's machine to isolate a name-resolution issue from a connectivity issue.
3. Checked whether the file server itself was reachable and online by testing from a second, unaffected workstation.
4. Reviewed the Group Policy drive-mapping object for the user's OU for any recent changes to the target server path.

</details>

**Root Cause**
The mapped drive resolved fine by IP address but failed by hostname, pointing to a DNS resolution issue specific to the user's machine rather than a server outage, since other users' mapped drives to the same server were unaffected.

**Resolution**
Flushed the DNS resolver cache on the user's machine (ipconfig /flushdns) and confirmed the correct DNS server was assigned via DHCP. Remapped the drive and verified it connected successfully by hostname afterward.

> **User Communication (Ticket Notes)**
> *Advised the user to restart their machine if the issue recurs, since a stale DNS cache entry was the root cause and a reboot would clear it as well.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0071261` — Device Receiving Incorrect IP Configuration (APIPA Address)

<table>
<tr><td><b>Environment</b></td><td>Windows 11, DHCP-assigned network, wired connection</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Network &amp; Connectivity</td></tr>
<tr><td><b>Time to Resolve</b></td><td>25 minutes</td></tr>
</table>

**Reported Symptom**
User's laptop shows "No internet access" with a limited connectivity warning. Checking IP configuration shows an address in the 169.254.x.x range.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Ran ipconfig /all to confirm the device had self-assigned an APIPA address, indicating it failed to receive a valid lease from a DHCP server.
2. Checked the physical network connection (cable, port light activity) to rule out a layer 1 issue preventing the DHCP request from reaching the server.
3. Attempted ipconfig /release followed by ipconfig /renew to force a fresh DHCP request and observed whether a valid lease was returned.
4. Checked with the network team whether the DHCP scope for that subnet was near exhaustion, since scope exhaustion is a common cause of APIPA fallback.

</details>

**Root Cause**
The DHCP scope for that subnet had reached its lease exhaustion threshold due to a large number of stale leases from decommissioned devices that had not yet expired, leaving no available addresses to issue to the user's device.

**Resolution**
Network team cleared stale/expired leases from the DHCP scope to free up addresses, and the user's device received a valid IP lease on the next renew attempt. Verified internet and internal resource access afterward.

> **User Communication (Ticket Notes)**
> *Flagged the DHCP scope size to the network team as a candidate for expansion, since lease exhaustion is likely to recur as more devices are added to that subnet.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 5 |
| **Critical (P1) Incidents** | 1 |
| **Medium Priority (P3) Incidents** | 4 |
| **Average Time to Resolve** | ~46 minutes |
| **Systems Touched** | Wi-Fi (WPA2-Enterprise) · Access Switches · ISP Circuit/Modem · Group Policy Mapped Drives · DNS · DHCP |

## Skills Demonstrated

`Wireless Adapter & Power Management Troubleshooting` · `Switch Port & Uplink Diagnostics` · `Outage Isolation (Local vs. ISP vs. Network)` · `DNS Resolution & Cache Management` · `DHCP Scope & Lease Troubleshooting` · `Root Cause Analysis` · `ServiceNow-Style Incident Documentation` · `Cross-Team Escalation (Network Team, ISP Support)`

---

<div align="center">

*Built to demonstrate structured, repeatable troubleshooting methodology across the full network and connectivity stack — from the endpoint to the ISP circuit.*

</div>
