# PLAN.md

> Product backlog for OpenRPMS website.
> Part of the [OpenRPMS Product Cycle](https://productcycle.org) artifacts.
> Items are ordered by priority. Each item traces to a user story and persona.
> Work through items in order. Mark complete when done.

---

## Now

The public build page is `/build` ("How a release is built"): how a release is assembled from the public files, the certified line (BCER) kept current with patches, the YottaDB port, and the client programs a release has to support (VueCentric, iCare, BPRM). The page names what the public release leaves out, including the commercial chart libraries, and what that means for reuse.

Workflow pages stay the short patterns. The step-by-step journeys, with screens, live in one companion (rpms-ux). That companion is not public yet; the aim is to open it. Do not copy those journeys onto this site. When it is public, link it from the note in `_includes/journeys-note.html`.

The working record of the build is `lakeraven/rpms-ops`. When this page and that repo disagree, the repo wins and the page gets corrected. Gap counts on the page are from the October 23, 2026 release.

---

## In Progress

_(none)_

---

## Backlog

### 1. Consolidate workflows under `/workflows` with category routing
**Story:** US-07, US-08, US-05, US-06
**Persona:** B — Healthcare Implementer, C — Healthcare Staff
**Status:** Structural change — `/clinical-workflows` and `/administrative-workflows` exist as separate pages; need to consolidate and re-route

Each workflow is modeled as a **Process Model** (per [Touchpoints](https://touchpoints.app)) — a named BPMN workflow with:
- `name` — workflow title
- `category` — clinical | administrative
- `lanes` — swim lanes organized by role (Provider, Nurse, Pharmacist, Registration Clerk, etc.)
- `nodes` — individual steps/tasks
- `bpmn_xml` — the BPMN diagram definition
- `rpms_packages` — packages involved in this workflow

As a Healthcare Staff member,
I want to browse workflows by category and view each as a named process with role-based swim lanes,
so that I can find the workflows relevant to my role and follow them step by step.

**Acceptance Criteria:**
- Given I arrive at `/workflows`, when I scan the page, then I see all workflows listed with name, category, and a brief description
- Given I want to filter by category, when I navigate to `/workflows/clinical` or `/workflows/administrative`, then I see only workflows in that category
- Given I view a single workflow, when I read the process model, then I see: name, description, swim lanes by role, BPMN diagram, and which RPMS packages are involved
- All existing content from `/clinical-workflows` and `/administrative-workflows` is preserved and migrated
- Old URLs (`/clinical-workflows`, `/administrative-workflows`) redirect to the appropriate `/workflows/clinical` and `/workflows/administrative` paths

---

### 2. Author BPMN process models — Clinical workflows
**Story:** US-07, US-08
**Persona:** C — Healthcare Staff, B — Healthcare Implementer
**Status:** Not started — BPMN diagrams are placeholders on existing pages

Process models to author (minimum viable set):

| Name | Lanes |
|------|-------|
| Pre-Visit Preparation | Provider, Nurse, Registration Clerk |
| Encounter at Arrival | Provider, Nurse, Patient |
| Order Entry — Lab | Provider, Lab Technician |
| Order Entry — Medication | Provider, Pharmacist |
| Order Entry — Radiology | Provider, Radiology Technician |
| Result Reporting | Lab Technician / Radiology Technician, Provider |
| Clinical Documentation | Provider, CAC |

As a Healthcare Staff member,
I want to see rendered BPMN diagrams for each clinical workflow,
so that I can follow a patient encounter step-by-step and see where my role fits.

**Acceptance Criteria:**
- Given I view a clinical workflow page, when I view the BPMN diagram, then swim lanes are labeled by role, steps are labeled with the action and RPMS package, and flow is readable left-to-right
- All diagrams render in both light and dark mode
- Each diagram corresponds to a named process model in the site content

---

### 3. Author BPMN process models — Administrative workflows
**Story:** US-05, US-06, US-08
**Persona:** B — Healthcare Implementer, C — Healthcare Staff
**Status:** Not started — BPMN diagrams are placeholders on existing pages

Process models to author (minimum viable set):

| Name | Lanes |
|------|-------|
| Patient Registration | Registration Clerk, Patient |
| Scheduling | Registration Clerk, Provider, Patient |
| Billing and Charge Capture | Provider, Billing Staff, Business Office |
| Credential Tracking | Clinical Director, HIM, Administrator |
| Administrative Request Routing | Staff, Supervisor, Administrator |

As a Healthcare Implementer,
I want to see rendered BPMN diagrams for each administrative workflow,
so that I can map each workflow to a department and plan what configuration and training is required.

**Acceptance Criteria:**
- Given I view an administrative workflow page, when I view the BPMN diagram, then swim lanes are labeled by role, steps show the action and RPMS package, and flow is readable left-to-right
- All diagrams render in both light and dark mode
- Each diagram corresponds to a named process model in the site content

---

### 4. Add ER diagram to Domain Model page
**Story:** US-01, US-09
**Persona:** A — Systems Developer, D — Policy Analyst
**Status:** Not started — entity relationships described in prose only

As a Systems Developer,
I want to see a visual entity relationship diagram on the Domain Model page,
so that I can understand how core healthcare entities relate to each other at a glance.

**Acceptance Criteria:**
- Given I arrive at `/domain-model`, when I view the Core Entities section, then I see a mermaid erDiagram rendering the entities and relationships from `product-cycle/DATA_MODEL.md`
- Diagram is consistent with the data model artifact
- Renders in both light and dark mode

---

### 5. Build `/resources` index page
**Story:** US-10
**Persona:** B — Healthcare Implementer, D — Policy Analyst, C — Healthcare Staff
**Status:** Not started — no single index of operational resources exists

As a Policy/Admin Analyst,
I want a single page listing all operational resources on the site,
so that I can find surveys, checklists, and workflow guides without navigating the full site.

**Acceptance Criteria:**
- Given I arrive at `/resources`, when I scan the page, then I find grouped links to: EHR Site Survey, EHR Site Tracking, Clinical Workflows, Administrative Workflows
- Each resource has a brief description and indicates who it's for
- Each resource indicates its source (IHS program document, OpenRPMS synthesis, etc.)
- Page is linked from the homepage "What's here" section

---

### 6. Add print stylesheet for EHR Site Survey and Site Tracking
**Story:** US-04, US-05
**Persona:** B — Healthcare Implementer
**Status:** Not started — pages are web-only; no print/export support

As a Healthcare Implementer,
I want to print or export the EHR Site Survey and Site Tracking Record,
so that I can share them with department leads and submit them to my Area Office.

**Acceptance Criteria:**
- Given I print `/ehr-site-survey` from a browser, then the output is cleanly formatted with no nav, no footer, and all sections intact
- Given I print `/ehr-site-tracking`, then the checklist items render with visible checkboxes suitable for manual completion
- Print styles apply to both pages without a separate URL

---

### 7. Add security controls distinction table to Security page
**Story:** US-12
**Persona:** E — Auditor / Compliance Officer
**Status:** Not started — platform vs. facility-configurable distinction present in prose but not structured

As an Auditor/Compliance Officer,
I want a clear table distinguishing platform-enforced controls from facility-configurable controls,
so that I can accurately assign responsibility for gaps and make actionable recommendations.

**Acceptance Criteria:**
- Given I arrive at `/security`, when I view the controls summary, then I find a table with columns: Control, Enforced By, Configurable At Facility, Notes
- Table covers: access control/key assignment, audit logging, EPCS enrollment, credential expiration alerts, session management
- Table is accurate against RPMS kernel documentation

---

### 8. Add persona-based entry points to homepage
**Story:** US-09, US-10
**Persona:** All
**Status:** Not started — homepage treats all visitors identically

As a first-time visitor,
I want to quickly find content relevant to my role,
so that I don't have to navigate the full site to find what applies to me.

**Acceptance Criteria:**
- Given I arrive at the homepage, when I scan the page, then I find role-labeled entry points or a "Who are you?" section linking to relevant pages for each persona
- Entry points cover at minimum: Systems Developer, Healthcare Implementer, Healthcare Staff, Policy Analyst, Auditor
- Existing "What's here" grid is preserved or integrated

---

### 9. Add cross-links between Applications catalog and Workflow pages
**Story:** US-01, US-06
**Persona:** A — Systems Developer, B — Healthcare Implementer
**Status:** Not started — pages exist in isolation with no mutual linking

As a Systems Developer,
I want the Application Catalog to link to relevant workflow pages,
so that I can move from a package entry to its operational context without navigating manually.

**Acceptance Criteria:**
- Given I view a package entry in `/applications`, when that package has a corresponding workflow section, then I see a link to that section
- Given I view a workflow section in `/clinical-workflows` or `/administrative-workflows`, then I see links to the relevant packages in the Applications catalog
- At minimum: Pharmacy ↔ Administrative Workflows billing section; Lab ↔ Clinical Workflows order entry; PIMS ↔ Administrative Workflows registration

---

## Completed

_(none yet)_
