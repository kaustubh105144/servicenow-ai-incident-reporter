# servicenow-ai-incident-reporter

> Transforming raw ServiceNow ticket exports into AI-driven operational intelligence — eliminating manual work note review and enabling stakeholder-ready insights in minutes.

## 🧩 Problem

In large ITSM environments, ServiceNow exports give you **raw data** — hundreds of tickets with lengthy work notes, no summaries, no ownership clarity, and no executive context.

The traditional process looked like this:

- Manually read through work notes across 100+ tickets
- Identify which tickets are stalled, aging, or blocked
- Determine who owns what and why it hasn't moved
- Prepare a summary for management review calls

This was **slow, error-prone, and impossible to scale** — especially ahead of operational review calls where stakeholders needed clear answers fast.

## 💡 Solution

An end-to-end **Python-based AI automation pipeline** that ingests a ServiceNow Excel export, processes every active ticket through **AWS Bedrock**, and produces a fully structured, executive-ready Excel report — with **zero manual intervention**.

The pipeline dynamically constructs LLM prompts per ticket, extracts structured insights from unstructured work notes, and compiles both a granular ticket view and a high-level executive summary.

## ⚙️ How It Works

### Step 1 — Ingest ServiceNow Export

Reads a raw ServiceNow Excel export containing:
`Number`, `State`, `Opened`, `Updated`, `Assigned To`, `Short Description`, `Work Notes`, `Additional Comments`

### Step 2 — Filter Active Tickets

Automatically drops `Closed` and `Resolved` tickets. Only active incidents are processed.

```
Input Tickets  : 105
Active Tickets : 77
```

### Step 3 — Aging Calculations

Two operational metrics are computed per ticket:

| Metric | Formula |
|---|---|
| Days Not Updated | Current Date − Last Updated Date |
| Ticket Aging | Current Date − Opened Date |

### Step 4 — AI Prompt Construction & Processing

For each ticket, a structured JSON payload is dynamically built and sent to **AWS Bedrock**:

```json
{
  "ticket_number": "INC100001",
  "description": "VPN access issue",
  "work_notes": "...",
  "additional_comments": "..."
}
```

The model returns structured insights:

```json
{
  "action_taken": "Network logs reviewed and VPN configuration updated.",
  "pending_with": "End User",
  "work_note_summary": "VPN issue investigated and fix applied.",
  "additional_note_summary": "User validation pending."
}
```

### Step 5 — Executive Summary Generation

After individual ticket analysis, a **second AI pass** runs across the full ticket population to generate cross-ticket operational insights:

- Aging risks
- Ownership gaps
- Operational bottlenecks
- Escalation trends
- Communication challenges

### Step 6 — Structured Excel Report Output

All insights are written into a formatted, template-driven Excel workbook.

## 📊 Output Report Structure

### Sheet 1 — Executive Summary

Free-form AI-generated narrative covering:

- Stalled ticket patterns
- Blockers and dependencies
- Teams with ownership gaps
- Escalation risk areas

Designed for **Incident Managers, Operations Leads, Service Delivery Managers, and Leadership Reviews**.

### Sheet 2 — Executive KPI Summary

| Section | Metrics |
|---|---|
| Incident Health | Total Active, Not Updated >3 / >5 / >7 / >10 Days |
| Assignment Analysis | Assignee, Active Count, Aging >7 Days, Aging >10 Days |
| Aging Distribution | Buckets: >3, >5, >7, >10 Days |

### Sheet 3 — Ticket Detail View

Each row contains:

| Column | Description |
|---|---|
| Ticket Number | Unique incident ID |
| Description | Short description of the issue |
| State | Current ticket state |
| Assigned To | Owner of the ticket |
| Created Date | When the ticket was opened |
| Last Updated Date | Most recent update timestamp |
| Days Not Updated | Staleness indicator |
| Action Taken | What has been done, per AI analysis |
| Pending With | Who is blocking resolution |
| Work Note Summary | AI-generated summary of work notes |
| Additional Comment Summary | AI-generated summary of additional comments |

Managers can review 100+ tickets in minutes — without reading a single work note.

Designed for **Incident Managers, Operations Leads, Service Delivery Managers, and Leadership Reviews**.

## 🏗️ Architecture

```
ServiceNow Export (Excel)
         │
         ▼
    Data Ingestion & Filtering
         │
         ▼
    Aging Calculations
         │
         ▼
    AI Prompt Construction
         │
         ▼
    AWS Bedrock (LLM)
         │
         ▼
    AI Input / Output JSON (Audit Logs)
         │
         ▼
    Executive Analytics & Summary
         │
         ▼
    Formatted Excel Report
```

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Python |
| Data Processing | Pandas, OpenPyXL |
| AI / LLM | AWS Bedrock |
| Reporting | Excel (template-driven) |
| Audit Logging | JSON (timestamped input & output files) |

## 🗂️ Audit & Traceability

Every AI interaction is logged:

```
AI_Input_YYYYMMDD_HHMMSS.json   → All prompts sent to Bedrock
AI_Output_YYYYMMDD_HHMMSS.json  → All responses received
```

This supports auditability, model validation, and future reprocessing.

## 📈 Outcome

- ✅ Adopted by management and currently in **active pilot** for operational review calls
- ✅ Replaced hours of manual ticket analysis with a fully automated pipeline
- ✅ Enabled **business value conversations** with stakeholders using AI-driven operational intelligence
- ✅ Supports **ITIL good practice adherence** through consistent, structured reporting
- ✅ Reduced stakeholder preparation time from hours to minutes by delivering AI-summarised, review-ready reports before every operational call
- ✅ Built a fully auditable AI pipeline with timestamped input/output logs — ensuring every insight is traceable and reproducible

## 🚀 Future Roadmap

- [ ] Risk scoring per ticket
- [ ] Recommended next actions for each ticket
- [ ] Trend analysis & historical comparison
- [ ] Power BI integration
- [ ] ServiceNow API integration (replace manual export)
- [ ] Automated email distribution

## 🔑 One-Line Summary

> Instead of showing raw incidents, this solution uses AI to convert operational ticket data into actionable summaries, ownership insights, and executive-level reporting — replacing manual analysis entirely.
