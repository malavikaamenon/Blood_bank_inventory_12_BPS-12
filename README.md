# MyProject_BPS#12

**Blood Bank Inventory & Emergency Donor Matcher** (Problem Statement #12, Healthcare & Telemedicine)

| | |
|---|---|
| **Name** | Malavika Arun |
| **SRN** | PES1UG24CS257 |
| **Problem Statement** | BPS#12 |
| **Course** | Software Engineering, Dept. of CSE, PES University |

---

## 1. Project Overview

Blood banks need a real-time stock management system that tracks blood component shelf lives and, during critical shortages, sends geo-targeted emergency notifications to matching, eligible donors.

This repository contains all individual-project deliverables for the Software Engineering labs: requirements engineering, UML modelling, architecture, Agile project management (Jira), SRS, code generated with GitHub Copilot, and software testing practice.

**Actors:** Emergency Requester, Blood Bank Manager, Donor, SMS Gateway (external system)

## 2. Requirements at a Glance

| ID | Type | Summary | Priority |
|---|---|---|---|
| FR-001 | Functional | Cross-reference emergency requests against live inventory and SMS-alert compatible donors within 10 km | High |
| FR-002 | Functional | Register and update blood bag inventory (group, component, collection and expiry date) | High |
| FR-003 | Functional | Flag units within 48 h of expiry as "Near-Expiry"; remove expired units from allocatable stock | High |
| FR-004 | Functional | Emergency Requester submits a request (group, component, quantity, hospital location) | High |
| FR-005 | Functional | Donor confirms or declines availability by replying to the SMS alert | Medium |
| NFR-001 | Performance & Security | Strict transactional consistency in the inventory ledger; no double allocation of a blood bag | High |
| NFR-002 | Reliability / Availability | At least 99.9% availability over a rolling 30-day window | High |



## 3. Architecture Summary

**Style chosen: Layered Architecture** (Presentation, Business, Data), with an external SMS Gateway reached only through the Notification Service.

- **Presentation Layer:** User Interface Component (web / mobile portal)
- **Business Layer:** Inventory Manager, Emergency Request Manager, Donor Matching Service, Notification Service
- **Data Layer:** Database Component (inventory ledger, donor registry, request log)
- **External:** SMS Gateway

**Why layered:** a single ACID inventory ledger satisfies NFR-001 (no double allocation), and the layers map cleanly to the FRs. Microservices and Client-Server were considered and rejected (see the justification document).






## 4. Jira Project Management

Three Jira projects were created for this problem statement:

| Project | Key | What it shows |
|---|---|---|
| Scrum | `SBPS12` | Epic "Blood Bank Inventory & Emergency Donor Matcher", 7 stories (5 FR + 2 NFR) with subtasks, backlog, sprint, burndown reports |
| Kanban | `KBPS12` | Same epic and 7 stories on a Kanban board, subtasks, backlog |
| Bug Tracking | `BTBPS12` | 6 reported defects: expired unit shown as allocatable, SMS sent outside 10 km, duplicate blood bag ID accepted, donor NO reply marked YES, Near-Expiry tag missing at 48 h, concurrent requests reserving the same bag |



## 6. Core Use Case: UC-01 Request Emergency Blood Unit

- **Primary actor:** Emergency Requester
- **Supporting actors / systems:** Blood Bank Manager, Donor, SMS Gateway
- **«include»:** UC-02 Search Compatible Donors, UC-03 Send SMS Alert to Donors
- **«extend»:** UC-06 Verify Donor Eligibility (optionally extends UC-03)
- **Alternate flows:** 4a Invalid blood group entered; 9a No donor response within SLA (search radius widened to 20 km, request escalated)


_Malavika Arun, PES1UG24CS257, BPS#12_
