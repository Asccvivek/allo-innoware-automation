# Allo Innoware Operations Automation System
## Manual Testing Protocol & Verification Guide

> **Prepared for:** Ramananda Kairi (`rama-kairi`)  
> **Candidate / Author:** Vivek Debnath (`Asccvivek`)  
> **Live System Sheet:** [Open Live Google Sheet](https://docs.google.com/spreadsheets/d/14MwdPdHUglsY4Bh1cwtU4zWj6IHqL8_uArUmeMQ5qW4/edit)  
> **Target Evaluation:** AI Operations & Automation Manager (Founder's Office) — Allo Innoware  

---

## 🎯 Testing Overview

This manual test suite covers the **8 Official Assessment Scenarios** outlined in Section 6 of the Allo Innoware brief. 

The entire system runs inside the Google Sheet using **Google Apps Script (V8 runtime)**, with 3 autonomous background triggers, live conditional formatting, dynamic `toast` UI alerts, and multi-channel HTML email delivery.

---

## 📊 Live Workbook Architecture (7 Sheets)

| # | Sheet Name | Scope / Operating Focus |
| :--- | :--- | :--- |
| **1** | `📌 System_Overview_&_SLA` | Architecture foundations, 3-tier SLA governance matrix, daily task tracking protocol, and evaluator guide. |
| **2** | `Task_Master` | Central cadence repository (Daily, Weekly, Fortnightly, Monthly) with IST cut-off engines and alert flags. |
| **3** | `My_Pending_Tasks` | **Employee Work Desk:** Single-point action interface for viewing pending items, entering proof, and completing tasks. |
| **4** | `Task_Completion_Log` | **Immutable Audit Trail:** Append-only completion log with warning protection, capturing full operational metadata. |
| **5** | `Manager_Dashboard` | **Executive Control Tower:** Real-time KPI scorecards, exception monitor, and Manager Circuit Breaker. |
| **6** | `🧪 Step_by_Step_Test_Lab` | **Interactive Test Workbench:** Interactive test desk with dynamic pass formulas, dropdown verdicts, and notes. |
| **7** | `⚙️ Automated_Code_Checks` | **Assertion Suite:** 12 automated unit & integration checks validating regex, ScriptLock, cutoffs, and channels. |

---

## 🛠️ Step-by-Step Test Scenarios (Tests 1 to 8)

### Test 1: Cadence-Based Recurring Task Rollover
* **Objective:** Demonstrate that completing a recurring task automatically closes the cycle, logs audit metadata, and calculates the next cycle due date using the configured cadence (Daily: +1d, Weekly: +7d, Fortnightly: +15d, Monthly: +30d).
* **Operating Sheet:** `My_Pending_Tasks`
* **Execution Steps:**
  1. Open tab `My_Pending_Tasks`.
  2. Locate task `ALLO-OPS-102` (Weekly 7d recurrence).
  3. In Column F (`Enter Ticket ID / WhatsApp Proof`), enter: `TICKET-CX-7729`.
  4. Tick Column H `[x] Mark Complete`.
* **Expected Verification:**
  - Checkbox unchecks automatically.
  - A green toast appears: *"Task ALLO-OPS-102 completed successfully!"*.
  - A new immutable row is appended to `Task_Completion_Log`.
  - In `Task_Master`, `ALLO-OPS-102`'s due date rolls over to **`Today + 7 Days`**.
  - Alert flags (`Manager Alert Sent` & `Leadership Escalated`) reset to `FALSE`.

---

### Test 2: Single-Point Action Desk (No Duplicate Updating)
* **Objective:** Verify staff never has to update across multiple sheets or tabs.
* **Operating Sheet:** `My_Pending_Tasks`
* **Execution Steps:**
  1. Stay on `My_Pending_Tasks`.
  2. Notice that staff views their pending tasks, inputs their proof, and submits completion directly from this single view.
* **Expected Verification:**
  - One-click workflow: all downstream tables (`Task_Master`, `Task_Completion_Log`, `Manager_Dashboard`) update synchronously via Apps Script without employee tab switching.

---

### Test 3: Daily Task Completion Method & Anti-Fraud Proof Validation
* **Objective:** Verify operational proof handling for daily tasks without tickets (WhatsApp/AWB/Batch ID) and anti-cheat validation.
* **Operating Sheet:** `My_Pending_Tasks`
* **Execution Steps:**
  1. On task `ALLO-OPS-101` (Daily), leave Column F blank (or enter `ok`). Tick `[x] Mark Complete`.
     - *Result:* Rejection toast: `⚠️ Validation Blocked: Proof too short / required`. Checkbox resets.
  2. Enter `ALLO-OPS-101` into Column F (self-referencing Task ID). Tick `[x] Mark Complete`.
     - *Result:* Rejection toast: `⚠️ Validation Blocked: Proof cannot be the Task ID`.
  3. Enter valid WhatsApp dispatch confirmation: `WA-GRP-OPS-UPDATE-20261122`. Tick `[x] Mark Complete`.
* **Expected Verification:**
  - Sub-3 character and self-referencing inputs are intercepted.
  - Real WhatsApp/AWB reference completes task cleanly and rolls over to tomorrow (+1 Day).

---

### Test 4: Same-Day Manager Alert & Email Dispatch
* **Objective:** Verify that past cutoff time triggers same-day managerial alerting via email.
* **Operating Sheet:** `Task_Master` & Custom Menu
* **Execution Steps:**
  1. In `Task_Master`, set `ALLO-OPS-101` Due Date to yesterday (or set Cut-off Time to an hour ago today).
  2. Click top menu: **`Allo Innoware Ops` > `▶️ 3. Run Overdue & Escalation Check Now`**.
* **Expected Verification:**
  - In `Task_Master`: Column I turns red **`OVERDUE`**, Col J (`Manager Alert Sent`) becomes **`TRUE`**, Col L records dispatch timestamp.
  - Toast: `⚠️ Overdue breach on 1 task(s)! Email alert sent to Operations Manager.`
  - Gmail: Operations Manager (`viknthdv@gmail.com`) receives high-priority HTML email with Task ID, Description, SLA breach time, and direct sheet link.

---

### Test 5: Manager Action & Acknowledgement Logging (Circuit Breaker)
* **Objective:** Verify manager acknowledgement freezes escalation to leadership for up to 24 hours.
* **Operating Sheet:** `Manager_Dashboard`
* **Execution Steps:**
  1. Switch to `Manager_Dashboard`.
  2. Locate the overdue task on the Exception Desk (Row 7).
  3. Check Column F **`[x] Manager Acknowledged`**.
  4. Select or type in Column H: `"Spoke with Rohan, dispatch underway"`.
* **Expected Verification:**
  - Column G (`Acknowledged At`) auto-populates exact live timestamp (`dd/MM/yyyy HH:mm:ss`).
  - Toast: `Acknowledged Task ALLO-OPS-XXX. Level 2 Leadership Escalation paused (24h resolution window).`
  - Audit log permanently records acknowledgement timestamp and notes.

---

### Test 6: Leadership Escalation (4-Hour Grace Expiry)
* **Objective:** Verify overdue tasks unacknowledged past 4 hours escalate directly to Founder.
* **Operating Sheet:** `Task_Master` & Custom Menu
* **Execution Steps:**
  1. On the overdue task in `Task_Master` (where Col J is `TRUE`), edit Column L (`Manager Alert Sent At`) to **5 hours ago** (`now - 5h`).
  2. Ensure `Manager_Dashboard` Col F is **unchecked**.
  3. Click menu: **`Allo Innoware Ops` > `▶️ 3. Run Overdue & Escalation Check Now`**.
* **Expected Verification:**
  - In `Task_Master`: Col K (`Leadership Escalated`) becomes **`TRUE`**. Urgency status escalates to **`CRITICAL / ESCALATED`**.
  - Toast: `🚨 Critical SLA breach on 1 task(s)! Escalated to Founder/Leadership.`
  - Gmail: Founder (`bidhudvnth@gmail.com`) receives urgent Red-Header HTML alert.

---

### Test 7: Escalation Timing Logic & 24-Hour Hard Backstop Cap
* **Objective:** Verify acknowledged tasks that remain unresolved beyond 24 hours escalate to Founder.
* **Operating Sheet:** `Task_Master` & `Manager_Dashboard`
* **Execution Steps:**
  1. On `Manager_Dashboard`, ensure `[x] Manager Acknowledged` is **checked** (pausing standard 4h grace).
  2. In `Task_Master`, edit Column L (`Manager Alert Sent At`) to **25 hours ago** (`now - 25h`).
  3. Click menu: **`Allo Innoware Ops` > `▶️ 3. Run Overdue & Escalation Check Now`**.
* **Expected Verification:**
  - 24-hour hard backstop rule fires: Col K becomes **`TRUE`**, Founder receives critical escalation email despite managerial acknowledgement.

---

### Test 8: Concurrency, ScriptLock & Anti-Spam Idempotency
* **Objective:** Verify rapid multi-clicks or repeated cron runs do not produce duplicate logs or spam emails.
* **Operating Sheet:** Custom Menu & `My_Pending_Tasks`
* **Execution Steps:**
  1. Click **`▶️ 3. Run Overdue & Escalation Check Now`** 3 times in rapid succession.
  2. Double-click `[x] Mark Complete` rapidly on a pending task.
* **Expected Verification:**
  - ScriptLock (`LockService.getScriptLock()`) cleanly serializes execution: exactly **1** row logged in `Task_Completion_Log`.
  - State lock flags in Col J & L prevent duplicate email dispatch. Toast confirms: `ℹ️ Zero duplicate emails sent (Idempotency Active)`.

---

## 📝 Recording Verdicts in `🧪 Step_by_Step_Test_Lab`

After verifying each test:
1. Switch to tab **`🧪 Step_by_Step_Test_Lab`**.
2. In Column G, change the dropdown from `⚪ Not Tested` to **`🟢 PASSED`**.
3. In Column H, enter any specific observations (browser version, toast timing, inbox delivery).
4. Watch the top scorecard dynamically hit **`Tests Passed: 8 / 8 (100.0% Compliance)`**!
