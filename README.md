# Vigilance ‘Q’: The Pharma Quality Watchtower

## Project Description

**Vigilance ‘Q’** is an agentic batch quality case management solution built for **UiPath AgentHack 2026 - Track 1: Maestro Case**.

The solution helps pharmaceutical quality teams detect weak quality signals early, assemble evidence, route human decisions, and update enterprise systems only after approval.

In pharmaceutical manufacturing, a batch rarely fails suddenly, it usually drifts first. Weak signals may appear across multiple enterprise systems, including:

- LIMS
- MES
- SAP/ERP
- QMS
- SOP/Specification documents
- Calibration records
- Analyst qualification records
- Historical batch data

Individually, each signal may appear acceptable. However, when combined, they can reveal an increasing batch quality risk.

Today, QA reviewers often become the integration layer. They manually:

- Search across multiple systems
- Chase missing evidence
- Reconcile spreadsheets
- Review screenshots
- Assemble audit packs after the fact

**Vigilance ‘Q’** addresses this challenge by creating **one live Maestro Batch Quality Case for every manufacturing batch**.

The case remains active throughout the complete quality lifecycle:

1. First quality signal detection
2. Readiness checks
3. In-process quality control
4. Final QC review
5. AI recommendation
6. Human QA decision
7. Controlled enterprise system updates
8. Audit-ready case closure

## Core Principle

> **AI investigates. Human QA decides. Systems update only after approval.**


## Demo Scenario

| Item | Details |
|------|---------|
| **Product** | Paracetamol 500 mg Immediate-Release Tablets |
| **Batch** | PTC-006 |
| **Scenario** | A batch that meets release specifications but gradually accumulates multiple weak quality signals across manufacturing, testing, and environmental data. Individually, these signals are not out of specification; collectively, they indicate an elevated quality risk. |
| **Agent Recommendation** | Recommend placing the batch on **Quality Hold** and initiating a **Phase I Investigation** based on the aggregated risk assessment. |
| **Human Decision** | The QA reviewer evaluates the assembled evidence, considers the AI recommendation, and makes the final batch disposition decision in accordance with GMP procedures. |

## Problem It Solves

Pharmaceutical batch quality review is inherently:

- Long-running
- Evidence-heavy
- Exception-driven
- Fragmented across multiple enterprise systems
- Dependent on human approval
- Highly audit-sensitive

Traditional automation is often too linear for this type of work. Fixed workflows struggle to accommodate:

- Dynamic evidence gaps
- Gradually increasing risk signals
- Missing or expired documents
- Parallel review activities
- Human overrides
- Controlled downstream system updates

**Vigilance ‘Q’** leverages **UiPath Maestro Case** to manage the process as a **live case lifecycle** instead of a rigid workflow.


## Solution Capabilities

The solution enables:

- Early quality drift detection
- Structured quality signal classification
- Evidence assembly across enterprise systems
- Human review for missing or expired evidence
- QA decision gates
- Controlled enterprise system updates after approval
- Audit-ready case closure records

## UiPath Components

| Component | What it does |
|---|---|
| **UiPath Maestro Case** | Orchestrates the end-to-end case. Manages stages, routing, human tasks, the audit trail, and built-in SLA timers and escalations at both the case and stage levels. |
| **UiPath Orchestrator** | Hosts and runs every RPA process, App, and Agent referenced in the case (via folder-path bindings), and provides execution logs/traceability for them. |
| **UiPath Coding Agents** | Used to accelerate the development of individual RPA workflows and agents by leveraging Gemini CLI and Codex. Assisted in generating, refining, and maintaining workflow logic and agent implementations. |
| **UiPath Agents** | Quality Signal Analysis Agent, Trend Analysis Agent, and Recommendation Agent. Agents checking data against fixed, version-controlled rules. The first two run at every QC phase. |
| **UiPath RPA workflows** | Retrieve records, validate batch data, write corrected data back, and save agent results as evidence. No decisions, just data movement and checks. |
| **UiPath Apps + Action Center** | Human-facing screens: Data Review & Verification for fixing batch data, Evidence Validation for checking agent results, Quality Review for the final QA decision. |
| **UiPath Data Fabric** | The database behind everything. Stores LIMS records, Historical Trend, and Evidence. Also triggers the case when a new batch starts. |
| **UiPath Integration Service** | The connector layer behind both the LIMS trigger and the Outlook email step - handles the event trigger and the outbound Send Email activity. |
| **UiPath Cloud Robots - Serverless** | Executes RPA processes without requiring dedicated unattended machine |

In pharma quality, classification must be reproducible, attributable, timestamped, and inspectable. An LLM should not decide whether a batch signal is OOT or OOS. A controlled rule running in a agent can.

**Structured rules decide. SOP context explains. Human QA approves.**

## Solution Architecture

Reference architecture: every signal becomes case evidence, every recommendation hits a human gate, and every system update is approved.

<img width="1153" height="615" alt="Slide7" src="https://github.com/user-attachments/assets/61b4778d-a2e0-445f-9165-481950728e05" />

Each batch = **one live Maestro case**. Stages activate as signals arrive; SLAs and audit history accumulate at every step regardless of which branch the case takes.

## Setup Instructions
 
### Prerequisites
 
- A UiPath Automation Cloud tenant with the following licensed/enabled:
  - Orchestrator
  - Maestro
  - Agent Builder
  - Data Fabric
  - Integration Service
  - Apps & Action Center
  - Studio Web
- Requires UiPath Cloud Robots – Serverless to execute RPA processes.
- An Outlook / Microsoft 365 account for the QA Notification connection.
- Data Fabric schema file (JSON): [Vigilance Q DataFabric Schema.json](https://drive.google.com/file/d/1XR_u331geyKIZRYd5TWpMWPydoEI8TpX/view?usp=sharing)
- Sample data representing batch `PTC-006`: [Vigilance Q_EntityData.xlsx](https://docs.google.com/spreadsheets/d/105PbZyZepOxiPCMxytCZ536m8aIL1IJq/edit?usp=sharing&ouid=103203708202152722396&rtpof=true&sd=true)
- Context grounding resource files - rule/reference documents used across the different agents: [Context Grounding](https://drive.google.com/drive/folders/12ulsAsz4_gtUotDDtDwR7CXMo9qBj1m7?usp=sharing)
  
### Clone the Repository into Studio Web
 
1. Log in to UiPath Automation Cloud.
2. Open Studio Web and go to Local Workspace.
3. Click Clone Solution.
4. Connect your GitHub account and provide the repository URL: [Vigilance Q](https://github.com/deepaksreeram/Vigilance-Q)
5. Click clone. When the permission pop-up appears, click Allow.
6. Wait for cloning to finish. This can take a few minutes depending on solution size.
   
### Resolve Validation Errors
 
Once cloning completes, the solution will typically show validation errors. Resolve them in this order:
 
1. **Connections** - reconnect/re-authenticate Data Fabric and Outlook Connections.
2. **Indexes** - recreate the index used for context grounding/rule documents (see 5.4 below).
3. **Entities** - create or refresh Data Fabric entities flagged as missing/out of sync.
4. Open the Case Plan in Maestro and check for any remaining validation errors.
5. **Action App User Assignment** - From within the Case Plan, open the Properties of each Action App (Data Review & Verification, Evidence Validation, and Quality Review), navigate to Assignment, and assign a user to each app. This must be completed before publishing; otherwise, human tasks will have no assigned user.

### Import Data Fabric Schema, Sample Data & Context Grounding
 
1. Import the Data Fabric schema using the Vigilance Q DataFabric Schema.json file provided in the Prerequisites section.
2. Import the sample data for batch PTC-006 using the Vigilance Q_EntityData.xlsx file provided in the Prerequisites section. Ensure the data is loaded into the corresponding Data Fabric entities.
3. Upload the Context Grounding resource files provided in the Prerequisites section to a Storage Bucket. These files contain the rule and reference documents used by the different agents.
4. Create an Index for the Storage Bucket containing the context grounding files. This enables the agents to perform context grounding and retrieve relevant documents during execution.
 
### Publish & Deploy
 
1. Once all validation errors are resolved and Action App users are assigned, publish the package.
2. Deploy the solution to your target Orchestrator folder.
3. Allocate the required license(s) to that folder.
4. Configure the Data Fabric trigger to automatically start a Case whenever a new batch record is added in the LIMS entity.

### Run the Demo
 
1. Add a new batch record to the LIMS entity in Data Fabric representing batch `PTC-006`.
2. A new Maestro Batch Quality Case is automatically created.
3. Follow the case through its stages in the Maestro instance.


> **⚠️ Disclaimer:** This is a hackathon demo prototype. The pharma data, rules, thresholds, and integrations are representative demo assets and are not intended for production GMP use without validation.
