# 🚀 Allo Innoware — Operations & Recurring Task Automation System

> **Production Prototype (V8.0 Master Architecture)**  
> **Candidate / Systems Lead:** Vivek Debnath ([Asccvivek](https://github.com/Asccvivek))  
> **Submission Destination:** `hr.ops.allo@gmail.com`  
> **Live Operating Sheet:** [Allo Innoware Operations Automation System](https://docs.google.com/spreadsheets/d/14MwdPdHUglsY4Bh1cwtU4zWj6IHqL8_uArUmeMQ5qW4/edit)  
> **Target Role:** Operations & Automation Manager (Founder's Office)  

[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-100%25%20Operational-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)](https://docs.google.com/spreadsheets/d/14MwdPdHUglsY4Bh1cwtU4zWj6IHqL8_uArUmeMQ5qW4/edit)
[![Google Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-V8%20Runtime-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://script.google.com)
[![QA Status](https://img.shields.io/badge/QA%20Status-16%2F16%20Checks%20Passed-10B981?style=for-the-badge)](docs/QA_AUDIT_REPORT.md)
[![Visibility](https://img.shields.io/badge/Repo-Public-10B981?style=for-the-badge)](https://github.com/Asccvivek/allo-innoware-automation)

---

## 📌 Executive Overview

Allo Innoware manages complex recurring operations across Logistics, Customer Experience, E-Commerce, and Supply Chain. Different tasks repeat at different frequencies (**Daily**, **7 Days**, **15 Days**, and **30 Days**), often involving external portal tickets or operational proofs (such as WhatsApp group updates or courier manifests).

This repository contains the complete production-grade automation system developed for the **Allo Innoware Founder's Office Assignment**, demonstrating the complete operational lifecycle:
$$\text{Task Cadence} \longrightarrow \text{Single-Point Action} \longrightarrow \text{Anti-Cheat Validation} \longrightarrow \text{Audit Preservation} \longrightarrow \text{Same-Day Alert} \longrightarrow \text{Leadership Escalation}$$

---

## 🏛️ System Architecture Flow Diagram

![Allo Innoware System Architecture Flow Diagram](Allo_Innoware_System_Architecture_Diagram.png)

---

## 📊 Assessment Scope vs. Production Implementation (What Was Asked vs. What We Built)

| Core Requirement | Baseline Brief Scope | Production-Grade System Delivered |
| :--- | :--- | :--- |
| **1. Recurring Cycle Logic** | Recalculate next due date for 4 static dummy tasks upon completion. | **Dynamic Cadence Engine:** Supports Daily (Mon–Sat), Weekly (7d), Fortnightly (15d), Monthly (30d), and custom cadences with zero formula drift. |
| **2. Single-Point Action Desk** | Staff updates completion from a pending tasks sheet without sheet-hopping. | **Multi-User Isolated Desk (`My_Pending_Tasks`):** Configured with native private Filter Views so multi-department staff see only their tasks without colliding. |
| **3. Non-Ticketing Proof Protocol** | Propose a practical method to capture daily tasks lacking a Jira ticket ID. | **Anti-Fraud Proof Gateway (`validateProof_`):** Accepts WhatsApp snippets, carrier manifests, or AWBs; strictly rejects blank entries, sub-3 char strings ("ok"), and self-referential IDs. |
| **4. Same-Day Alert & Cut-off** | Alert manager on the same day when a task is not finished on time. | **IST Cut-Off Cron Engine:** Hourly time-driven trigger evaluates per-task cut-offs (e.g. 19:00 IST for Logistics, 18:00 IST for CX), turning status OVERDUE and dispatching alerts. |
| **5. Manager Review & Escalation** | Capture manager review; escalate unresolved items to leadership. | **Manager SLA Circuit Breaker:** Checking `[x] Manager Acknowledged` freezes Level 2 Founder escalation up to 24h while logging review timestamps, preventing founder alert fatigue. |
| **6. Audit Trail & Idempotency** | Track who completed what; prevent duplicate cycles or duplicate emails. | **Immutable Audit Log (`Task_Completion_Log`) & Concurrency Lock:** Script enforces `LockService` to prevent race conditions during simultaneous edits. |

---

## 🛡️ Beyond-the-Brief Enterprise Hardening (Production Additions)

1. **Centralized Dynamic Employee Hub (`System_Overview_&_SLA` Section 8):** Zero hardcoded emails or assignees in script code. Adding an employee in Section 8 dynamically cascades across all workbook dropdowns, auto-populates departments, and routes alert notifications.
2. **Automated Task Creator Wizard & IST Creation Audit:** Managers can assign new tasks either directly in `Task_Master` Row 7+ or via the custom UI dialog, auto-generating sequential IDs (`ALLO-OPS-105`), auto-mapping departments, and stamping creation timestamps and assigner email.
3. **Historical Chain of Custody Preservation:** Active dashboard inputs reset cleanly upon cycle completion, but manager review timestamps and historical follow-up notes are permanently locked in `Task_Completion_Log`.
4. **Built-in QA Verification Harness (`Step_by_Step_Test_Lab` & `Automated_Code_Checks`):** Full 8-test interactive QA lab (100% Pass Rate) and 16 automated unit/integration assertions verifying zero formula errors.
5. **Self-Healing Dropdowns & Universal Typography:** Standardized dropdown validations, text wrapping, and row heights across Google Sheets' HTML5 canvas.

---

## 🎛️ Live System Visual Walkthrough

### 1. Dedicated Step-by-Step Test Lab (`🧪 Step_by_Step_Test_Lab`)
Interactive testing workbench mapping Tests 1 to 8 from Section 6 of the brief, featuring dynamic pass rate formulas and editable feedback cells:
![Test Lab](assets/screenshots/06_step_by_step_test_lab.png)

### 2. Task Master Repository (`Task_Master`)
Central cadence repository with live IST cut-off engine and formula-driven urgency states:
![Task Master](assets/screenshots/02_task_master.png)

### 3. Employee Single-Point Work Desk (`My_Pending_Tasks`)
Staff interacts strictly with this single interface. Entering proof and ticking complete triggers synchronous rollover without tab hopping:
![My Pending Tasks](assets/screenshots/03_my_pending_tasks.png)

### 4. Append-Only Audit Trail (`Task_Completion_Log`)
Immutable digital paper trail preserving who completed the task, when, proof submitted, and historical manager follow-up notes:
![Task Completion Log](assets/screenshots/04_task_completion_log.png)

### 5. Executive Control Tower (`Manager_Dashboard`)
Real-time KPI scorecards and Manager Circuit Breaker for acknowledging overdue tasks and pausing leadership escalations:
![Manager Dashboard](assets/screenshots/05_manager_dashboard.png)

### 6. Automated Code & Structural Checks (`⚙️ Automated_Code_Checks`)
12 of 12 unit and integration assertions verified with 100% emerald green verdict:
![Automated Checks](assets/screenshots/09_automated_code_checks.png)

---

## 🏗️ Repository Structure

```text
allo-innoware-automation/
├── README.md                                 # Master system overview and architecture
├── appsscript.json                           # Apps Script project manifest (Asia/Kolkata, V8)
├── src/
│   └── Code.gs                               # Production V8.0 Master script (88KB, 7 sheets, borders, wrap, toast, lock)
├── docs/
│   ├── ASSIGNMENT_BRIEF.md                   # Official assignment requirements (Sections 1 to 7)
│   ├── MANUAL_TESTING_GUIDE.md               # Step-by-step verification guide for Ramananda Kairi
│   ├── MULTI_EMPLOYEE_DESK_ARCHITECTURE.md   # Multi-user isolation, Filter Views & Hub-and-Spoke architecture
│   ├── SYSTEM_ARCHITECTURE_AND_SLA.md       # Enterprise decoupling & SLA framework
│   └── QA_AUDIT_REPORT.md                    # 6-Vector QA audit report (16/16 passed assertion checks)
└── assets/
    └── screenshots/                          # Live UI captures across all 7 sheets + custom menu
```

---

## ⚡ Key Highlights & Innovations

1. **Anti-Fraud Proof Validation Engine:**
   - Centralized `validateProof_()` function rejects empty submissions, sub-3 character inputs (`"ok"`), and self-referencing Task IDs (`"ALLO-OPS-101"`).
2. **Same-Day Cutoff Detection Engine:**
   - Evaluates per-task IST cutoffs (e.g. 19:00 IST for logistics, 18:00 for CX). Breaches trigger same-day alerts to the Operations Manager rather than waiting for next-day rollovers.
3. **Manager Circuit Breaker:**
   - Manager checking `[x] Manager Acknowledged` freezes Level 2 escalation to the Founder for up to 24 hours, preventing alert fatigue while enforcing a hard 24-hour backstop cap.
4. **Historical Chain of Custody Preservation:**
   - When a task completes and rolls over into a new cycle, active dashboard inputs reset, but the manager's review timestamp and follow-up notes are permanently preserved in `Task_Completion_Log`.
5. **Universal Layout & SaaS Styling:**
   - Universal text wrapping (`setWrap(true)`), crisp slate borders (`#CBD5E1`), 34-56px row heights, and vertical middle alignment eliminate cell clipping across Google Sheets' HTML5 Canvas.

---

## 🧪 Testing Instructions for Ramananda Kairi

Please refer to the dedicated [Manual Testing Protocol & Verification Guide](docs/MANUAL_TESTING_GUIDE.md) for step-by-step instructions on executing Tests 1 through 8.

---

## 📬 Contact & Credentials
* **Architect:** Vivek Debnath
* **Email:** `vivekdvnath@gmail.com`
* **Phone:** +91 9394242828
* **Location:** Bhopal, MP, India
* **GitHub:** [@Asccvivek](https://github.com/Asccvivek)
