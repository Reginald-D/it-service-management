<div align="center">

# IT Helpdesk Portfolio
### Ticketing/Process-Related — Ticket Documentation

![Category](https://img.shields.io/badge/Category-Ticketing%2FProcess-0A66C2?style=for-the-badge)
![Tickets](https://img.shields.io/badge/Tickets-4-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-ServiceNow%20ITSM-informational?style=for-the-badge)
![Focus](https://img.shields.io/badge/Focus-SLA%20%7C%20Routing%20%7C%20Problem%20Mgmt-blueviolet?style=for-the-badge)

</div>

---

## About This Portfolio

This collection showcases four representative tickets spanning the most common **ticketing and process-related** challenges an enterprise helpdesk team manages — from SLA breach escalation and queue misrouting to knowledge base gap-filling and identifying repeat-incident patterns as formal problem tickets.

Each ticket follows a standard **ServiceNow-style format**:

> `Reported Symptom` → `Diagnostic Steps` → `Root Cause` → `Resolution` → `User Communication`

> [!NOTE]
> These are representative scenarios built to demonstrate structured troubleshooting methodology and confirm real world experiences. They are not records of actual incidents to keep the integrity and confidentiality of our clients and employees.

---

## Ticket Index

| # | Ticket ID | Title | Priority | Time to Resolve |
|---|-----------|-------|:--------:|:----------------:|
| 1 | [`INC0081109`](#inc0081109--p2-ticket-approaching-sla-breach--escalation-required) | P2 Ticket Approaching SLA Breach — Escalation Required | P2 | Escalated 3h 15m; resolved 3h 50m |
| 2 | [`INC0081154`](#inc0081154--ticket-repeatedly-misrouted-between-queues) | Ticket Repeatedly Misrouted Between Queues | P3 | 25 min + rule fix |
| 3 | [`INC0081198`](#inc0081198--recurring-user-question-flagged-for-knowledge-base-article) | Recurring User Question Flagged for Knowledge Base Article | P4 | 45 min |
| 4 | [`INC0081235`](#inc0081235--repeat-incident-pattern-identified--candidate-for-problem-ticket) | Repeat Incident Pattern Identified — Candidate for Problem Ticket | P3 | Pattern found over 3 wks; opened same day |

---

<br>

## `INC0081109` — P2 Ticket Approaching SLA Breach — Escalation Required

<table>
<tr><td><b>Environment</b></td><td>ServiceNow ITSM, P2 SLA target: 4-hour resolution</td></tr>
<tr><td><b>Priority</b></td><td>P2 – High</td></tr>
<tr><td><b>Category</b></td><td>Ticketing/Process-Related</td></tr>
<tr><td><b>Time to Resolve</b></td><td>Escalated at 3h 15m; resolved at 3h 50m (within revised target)</td></tr>
</table>

**Reported Symptom**
A P2 ticket for a department-wide printing outage was approaching its 4-hour SLA deadline with no update logged in over two hours, and the assigned technician was unresponsive to chat pings.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reviewed the ticket's work notes and activity log to confirm the last update timestamp and identify where progress had stalled.
2. Checked the assigned technician's current ticket queue and status to determine whether they were on a call, out of office, or simply overloaded.
3. Escalated to the shift lead per the SLA breach escalation procedure once the two-hour silent gap was confirmed.
4. Reassigned the ticket to an available technician with the relevant print-server access to resume troubleshooting immediately.

</details>

**Root Cause**
The originally assigned technician had gone into an unplanned extended call on an unrelated critical incident and had not flagged their capacity or requested reassignment, leaving the P2 ticket unattended past the point where escalation should have triggered automatically.

**Resolution**
Reassigned the ticket to a technician with the necessary access, who resolved the underlying print server service crash within 35 minutes. Notified the requester and department stakeholders of the revised resolution timeline and the reason for the delay.

> **User Communication (Ticket Notes)**
> *Logged the SLA breach root cause (workload conflict, not a technical blocker) for the team's post-incident review to evaluate whether automated SLA-warning alerts need a shorter lead time.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0081154` — Ticket Repeatedly Misrouted Between Queues

<table>
<tr><td><b>Environment</b></td><td>ServiceNow ITSM, automated category-based routing rules</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium</td></tr>
<tr><td><b>Category</b></td><td>Ticketing/Process-Related</td></tr>
<tr><td><b>Time to Resolve</b></td><td>25 minutes (routing correction) + follow-up rule fix</td></tr>
</table>

**Reported Symptom**
A user's VPN connectivity ticket was auto-routed to the Network queue, reassigned to the Security queue by that team, then bounced back to Network — cycling three times over two days without a technician taking ownership.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Reviewed the ticket's reassignment history to trace exactly which queue changes occurred and at what timestamps.
2. Checked the category/subcategory selected at ticket creation against the automated routing rule that maps categories to queues.
3. Identified that the ticket's subcategory ("VPN Authentication") matched routing keywords for both the Network and Security queues due to overlapping rule conditions.
4. Manually assigned the ticket to the Network queue with clear ownership notes to break the reassignment cycle immediately.

</details>

**Root Cause**
Two automated routing rules had overlapping keyword conditions for the "VPN Authentication" subcategory, one mapping it to Network and another to Security, causing tickets in that subcategory to repeatedly bounce between queues with neither team clearly designated as the owner.

**Resolution**
Manually resolved the immediate ticket by assigning direct ownership and working the VPN issue to completion. Submitted a change request to the ITSM admin team to correct the overlapping routing rule, designating Network as primary owner for that subcategory with Security added only as a watcher, not an auto-assignee.

> **User Communication (Ticket Notes)**
> *Apologized to the user for the multi-day delay caused by the routing loop and confirmed their VPN access was fully restored.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0081198` — Recurring User Question Flagged for Knowledge Base Article

<table>
<tr><td><b>Environment</b></td><td>ServiceNow ITSM, Knowledge Base module</td></tr>
<tr><td><b>Priority</b></td><td>P4 – Low (process improvement)</td></tr>
<tr><td><b>Category</b></td><td>Ticketing/Process-Related</td></tr>
<tr><td><b>Time to Resolve</b></td><td>45 minutes (article drafted and published)</td></tr>
</table>

**Reported Symptom**
Over a two-week period, the helpdesk received 11 separate tickets asking how to set up email on a personal mobile device, each handled individually with no existing knowledge base article to reference.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Searched the internal knowledge base to confirm no existing article covered mobile email setup, or that an existing article was outdated/hard to find.
2. Reviewed the 11 related tickets to identify the most common device types and configuration steps requested (primarily iOS and Android with the company's MDM enrollment requirement).
3. Drafted a step-by-step knowledge base article covering both platforms, including the MDM enrollment prerequisite that several users had missed.
4. Routed the draft through the standard KB review/approval process before publishing.

</details>

**Root Cause**
No self-service documentation existed for a frequently requested, low-complexity task, resulting in avoidable ticket volume that could otherwise be resolved without technician involvement.

**Resolution**
Published the new knowledge base article to the self-service portal and linked it in the ticket closure notes for all 11 related tickets. Added the article link to the auto-reply template for tickets categorized under "Mobile Email Setup."

> **User Communication (Ticket Notes)**
> *Monitored ticket volume for the category over the following two weeks and confirmed a significant drop in new tickets for the same request, validating the KB article's effectiveness.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## `INC0081235` — Repeat Incident Pattern Identified — Candidate for Problem Ticket

<table>
<tr><td><b>Environment</b></td><td>ServiceNow ITSM, Problem Management module, shared file server</td></tr>
<tr><td><b>Priority</b></td><td>P3 – Medium (process/root cause investigation)</td></tr>
<tr><td><b>Category</b></td><td>Ticketing/Process-Related</td></tr>
<tr><td><b>Time to Resolve</b></td><td>Pattern identified over 3 weeks; problem ticket opened same day</td></tr>
</table>

**Reported Symptom**
While closing out a routine "file share not accessible" incident, noticed the same shared drive had generated seven nearly identical incident tickets over the past three weeks, each resolved individually with a service restart but no root cause investigation.

<details>
<summary><b>Diagnostic Steps</b></summary>

1. Searched closed incident tickets by category and affected CI (configuration item) to confirm the pattern and count of repeat occurrences.
2. Reviewed each prior ticket's resolution notes and found all seven were resolved the same way — restarting the file share service — with no deeper investigation logged.
3. Checked server event logs around each incident's timestamp for a common trigger, and found each outage coincided with the nightly backup job window.
4. Escalated the pattern to the infrastructure team by opening a formal Problem ticket linked to all seven related incidents, per problem management process.

</details>

**Root Cause**
Not yet confirmed at the incident level — pattern strongly suggested a resource contention conflict between the nightly backup job and the file share service, but full root cause analysis was assigned to the infrastructure team via the problem ticket rather than resolved at the incident level.

**Resolution**
Opened a Problem ticket consolidating all seven related incidents, provided the timestamp correlation with the backup window as an initial lead, and handed off to infrastructure for root cause analysis and a permanent fix, rather than continuing to treat each occurrence as an isolated incident.

> **User Communication (Ticket Notes)**
> *Explained to the requesting team that this recurring issue had been escalated as a formal problem investigation to prevent the same outage from continuing to recur nightly.*

<div align="right"><a href="#ticket-index">back to top</a></div>

---

## Portfolio Snapshot

| Metric | Value |
|---|---|
| **Total Tickets Documented** | 4 |
| **High Priority (P2) Incidents** | 1 |
| **Medium Priority (P3) Incidents** | 2 |
| **Low Priority (P4) Incidents** | 1 |
| **Process Areas Covered** | SLA Escalation · Routing Rule Correction · Knowledge Management · Problem Management |
| **Systems Touched** | ServiceNow ITSM · Knowledge Base Module · Problem Management Module |

## Skills Demonstrated

`SLA Breach Escalation Procedures` · `Ticket Routing Rule Analysis & Correction` · `Knowledge Base Authoring & Publishing` · `Repeat Incident Pattern Recognition` · `Formal Problem Ticket Creation` · `Root Cause Analysis` · `ServiceNow-Style Ticket Documentation` · `Cross-Team Coordination (Shift Leads, ITSM Admin, Infrastructure)`

---

<div align="center">

*Built to demonstrate structured process management beyond individual tickets — proactively improving SLA adherence, routing accuracy, self-service coverage, and root cause visibility.*

</div>
