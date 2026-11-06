# Baseline Test Results

As-of date: 2026-04-22 | Project start: 2026-04-06 | Model: gpt-4o-mini

| Test | Input | Output (summary) | Hallucination | Result |
|---|---|---|---|---|
| T1 | Plan for ABCDE project | 8 phases, weeks and dates from the timeline; only the 3 stated goals; budget and team size marked not specified. Prompt fixed once: added the Week 1 start-date assumption and removed a stray "Not specified" bullet. | No | Pass |
| T2 | Plan for: "We want to build a chatbot." | Asks for scope, goals, timeline, team; no plan produced | No | Pass |
| T3 | Risk assessment, ABCDE | All 10 register risks (R01-R10) with register categories; 3 evidence links labeled as inference; no new risks. Prompt fixed once: added "answer only what was asked" after an unrequested top-3 list appeared. See T03_risk_assessment.md | No | Pass |
| T4 | Risk assessment for: "New project starting soon." | Says the project is unknown; asks for project details, scope, timeline, team | No | Pass |
| T5 | Weekly status, Sprint 3 | Pending | | |
| T6 | Status for: "Things are going fine." | States no task data is available; no invented statuses | No | Pass |
| T7 | Top 3 risks | R09 (T024 Blocked), R01 (T006 Overdue), R05 (T025 due soon), from the register only; notes that no severity scores exist. Prompt fixed twice: answer only what was asked, then rank by task evidence (first run gave R01, R02, R04 with generic reasons). See T07_top3_risks.md | No | Pass |
| T8 | Blocked or at-risk tasks | Pending | | |
| T9 | Plan for a 2-week project, no details | Asks for scope, goals, timeline, team; no plan produced | No | Pass |
| T10 | Status summary counts | Pending (expected from data: 5 Done, 3 In Progress, 1 Blocked, 16 To Do) | | |
| T11 | Milestones in next 2 weeks | Pending | | |
| T12 | Stakeholder email, Sprint 2 | Pending | | |

Screenshots: see the .png files in this folder.
