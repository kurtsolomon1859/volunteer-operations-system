# End-to-End Volunteer Operations System (Power Platform)

<img width="962" height="515" alt="Screen Shot 2026-02-17 at 10 50 55 am" src="https://github.com/user-attachments/assets/518fc859-a863-40a2-8811-3fd269a0333f" />

## Project Overview
A full-stack operational system designed to manage the intake, vetting, and deployment of volunteers. This solution replaced manual email chains and spreadsheets with an automated architecture, reducing administrative overhead and identifying a 16-day bottleneck in compliance vetting.

## Technical Architecture
* **Ingestion:** Power Automate flows using advanced string manipulation to parse unstructured email bodies into structured SharePoint list items.
* **Dataverse:** SharePoint Lists acting as a relational database (Volunteer Tracker, Opportunity Master, National Roles).
* **Logic Engine:** Complex SLA monitoring using Switch cases, dynamic time variables, and a deterministic data-matching engine that dynamically builds OData queries to link unstandardised data.
* **Analytics:** Power BI Process Mining dashboard using DAX to calculate cycle times between 10+ operational stages.

## Key Features & Code Highlights

### 1. Process Mining with DAX
I implemented custom DAX measures to calculate the "Cycle Time" between specific status changes. This required handling edge cases, such as process regression (negative days) or skipping stages.
* *See code:* [`process-mining-logic.dax`](process-mining-logic.dax)

### 2. "No-Code" Regex for Email Parsing
Without access to Premium Connectors (Regex) or Azure Functions, I engineered a nested `split()` and `if()` logic to dynamically locate data fields (Email, Phone) within variable raw text blocks. The logic dynamically identifies the "next available field" to use as a delimiter.
* *See code:* [`email-parsing-logic.txt`](email-parsing-logic.txt)

### 3. SLA & Stagnation Logic
To prevent application backlog, I built a logic engine that applies different SLA timers based on the specific role type (e.g., "Disaster Welfare" vs "Meals on Wheels").
* *See code:* [`sla-filter-logic.json`](sla-filter-logic.json)

### 4. Deterministic Location Matching & Dynamic OData Routing
To map messy, historical applicant locations (e.g., missing hyphens, localized names) to a strict canonical database, I engineered a cascading exact-match logic engine. Avoiding probabilistic "fuzzy" matching (which risks false-positive role assignments), this engine normalizes strings (e.g., standardizing typography, stripping diacritics like Māori macrons) and routes specific regional outliers. It then dynamically constructs complex OData filter queries to safely search SharePoint based on cascading fallback conditions.
* *See code:* [`resilient-location-matching-logic.json`](resilient-location-matching-logic.json)

## Tech Stack
* **Power BI:** DAX, Power Query, Bookmarks, Process Mining.
* **Power Automate:** Cloud Flows, JSON Parsing, OData Filters, Error Handling (Run After).
* **SharePoint:** Custom Lists, JSON Column Formatting.

---
*Created by [Kurt Solomon](https://www.linkedin.com/in/kurtsolomon/)*
