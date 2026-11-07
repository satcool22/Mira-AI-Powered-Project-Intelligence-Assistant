# Baseline Test Results

As-of date: 2026-04-22 | Project start: 2026-04-06 | Model: gpt-4o-mini

| Test | Input | Output (summary) | Hallucination | Result |
|---|---|---|---|---|
| T1 | Plan for ABCDE project | 8 phases, weeks and dates from the timeline; only the 3 stated goals; budget and team size marked not specified. Prompt fixed once: added the Week 1 start-date assumption and removed a stray "Not specified" bullet. | No | Pass |
| T2 | Plan for: "We want to build a chatbot." | Asks for scope, goals, timeline, team; no plan produced | No | Pass |
| T3 | Risk assessment, ABCDE | All 10 register risks (R01-R10) with register categories; 3 evidence links labeled as inference; no new risks. Prompt fixed once: added "answer only what was asked" after an unrequested top-3 list appeared. See T03_risk_assessment.md | No | Pass |
| T4 | Risk assessment for: "New project starting soon." | Says the project is unknown; asks for project details, scope, timeline, team | No | Pass |
| T5 | Weekly status, Sprint 3 | Sprint 3 only: Done 0, In Progress 2, Blocked 0, To Do 2, Total 4; tasks T007, T025, T008, T009 with names from the CSV; health GREEN; nothing invented. See T05_status_sprint3.md | No | Pass |
| T6 | Status for: "Things are going fine." | States no task data is available; no invented statuses | No | Pass |
| T7 | Top 3 risks | R09 (T024 Blocked), R01 (T006 Overdue), R05 (T025 due soon), from the register only; notes that no severity scores exist. Prompt fixed twice: answer only what was asked, then rank by task evidence (first run gave R01, R02, R04 with generic reasons). See T07_top3_risks.md | No | Pass |
| T8 | Blocked or at-risk tasks | Blocked: T024 (security team availability); overdue: T006 (1 day past due); due within 7 days: T007, T025; suggestions labeled; nothing invented. Dates were checked in code, not by the model. See T08_blocked_at_risk.md | No | Pass |
| T9 | Plan for a 2-week project, no details | Asks for scope, goals, timeline, team; no plan produced | No | Pass |
| T10 | Status summary counts | Done 5, In Progress 3, Blocked 1, To Do 16, Total 25, matching the CSV (problem statement says 3 Done; CSV has 5). Fixed twice: blocker details were missing (description field now passed through), then the To Do list was omitted (prompt rule 7). See T10_status_summary.md | No | Pass |
| T11 | Milestones in next 2 weeks | Window 2026-04-22 to 2026-05-06: Phase 2 deliverables (audit report, gap analysis) end in window; Phase 3 starts in window; nothing invented. See T11_upcoming_milestones.md | No | Pass |
| T12 | Stakeholder email, Sprint 2 | Professional email on Sprint 2 only (T004, T005 done; T006 in progress and past due); counts match; no Sprint 3+ tasks. Prompt fixed once: overdue task must appear under Risks. See T12_stakeholder_email.md | No | Pass |
Screenshots: see the .png files in this folder.
