# Allo Innoware Operations Automation System
## System Architecture, Enterprise Decoupling & SLA Framework

---

## 1. Architectural Philosophy

The Allo Innoware Operations Automation System is an event-driven, spreadsheet-native operations management engine designed to eliminate operational blind spots, automate recurring workflows, and enforce escalation discipline across cross-functional teams.

```
                          ┌───────────────────────────┐
                          │   Recurring Task Master   │
                          │      (Cadence Engine)     │
                          └─────────────┬─────────────┘
                                        │
                         Synchronous Cycle Generation
                                        │
                                        ▼
    ┌───────────────────────────────────────────────────────────────────────┐
    │              EMPLOYEE SINGLE-POINT ACTION WORK DESK                   │
    │                      ('My_Pending_Tasks')                             │
    │  • Dynamic Team Filter   • External Proof Input   • Instant Submit   │
    └───────────────────────────────────┬───────────────────────────────────┘
                                        │
                               Task Marked Complete
                                        │
           ┌────────────────────────────┴────────────────────────────┐
           ▼                                                         ▼
┌─────────────────────────────┐                           ┌─────────────────────┐
│    Append-Only Audit Log    │                           │  Next Cycle Rollover│
│   ('Task_Completion_Log')   │                           │    ('Task_Master')  │
│ • Captures Who, When, Proof │                           │ • New Due Date      │
│ • Preserves Ack Timestamps  │                           │ • Alert Flags Reset │
└─────────────────────────────┘                           └─────────────────────┘
```

---

## 2. Enterprise Production Decoupling Note

In this evaluation prototype, all sheets reside within a unified Google Spreadsheet workbook utilizing sheet protections (`setWarningOnly(true)`).

In an enterprise production deployment:
1. **Front-Office Employee Desk (Sheet A):** Hosted in a separate, isolated spreadsheet file accessible to frontline operational staff.
2. **Back-Office Executive Control Tower (Sheet B):** Hosted in a restricted leadership spreadsheet file containing managerial KPIs, SLA compliance scorecards, and escalation circuit breakers.
3. **Data Synchronization Engine:** Synchronized programmatically via Google Apps Script using `SpreadsheetApp.openById()` over Google's secure private cloud fabric. Frontline staff have zero read or write access to managerial scorecards, completely eliminating data leakage risks.

---

## 3. Three-Tier SLA Governance & Escalation Timelines

```
Cutoff Time Passed (e.g. 19:00 IST)
               │
               ▼
   [Tier 1: Operations Manager Alert]
   • High-Priority HTML Email dispatched
   • Flags Col J = TRUE, timestamps Col L
               │
               ├─────────────────────────────────────────┐
               ▼                                         ▼
   Manager Acknowledges [x]                  Manager Does NOT Acknowledge
   • Pauses escalation for up to 24h         • 4-Hour Grace Window expires
               │                                         │
               ├─────────────────────────┐               │
               ▼                         ▼               │
   Task Completed in < 24h    Task Unresolved > 24h     │
   • Clean cycle rollover     • Hard Backstop Triggers  │
   • Alert flags reset                   │               │
                                         ▼               ▼
                              [Tier 2: Founder / Leadership Escalation]
                              • Urgent Red-Header Executive Breach Alert
                              • Direct Sheet Link with Follow-up Action
```

### Rationale for SLA Timings:
- **Same-Day Alerting via Per-Task Cutoffs:** Unlike generic end-of-day cron jobs, per-task cutoffs (e.g. 12:00 for pricing, 18:00 for RTOs, 19:00 for logistics) enable prompt, same-day detection.
- **4-Hour Grace Window:** Gives operations managers adequate time to contact staff, troubleshoot vendor/courier API delays, and resolve bottlenecks without triggering executive alarm.
- **24-Hour Hard Backstop Cap:** Ensures that even acknowledged tasks cannot remain unresolved indefinitely without executive visibility.
