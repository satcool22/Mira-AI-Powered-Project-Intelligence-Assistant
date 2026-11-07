# Mira Program Charter (Q3)

Prepared by: Sathish Doraiswamy | Nexora Pvt. Ltd. | Version 1.0

## 1. Vision
Mira gives every Nexora PM and TPM an accurate first draft of their project plan, risk assessment, status report, milestone alert or stakeholder update in seconds instead of hours, grounded in the project's own files, so PMs spend their time on decisions and stakeholders instead of paperwork.

## 2. Scope

**In scope**
- Project plan generation from a project description and timeline (core)
- Risk assessment and top-N risk ranking from a risk register, linked to task evidence (core)
- Weekly status report from task board data (core)
- Milestone and at-risk task alerts (extended)
- Stakeholder update emails for one sprint (extended)
- Safe handling of vague input: Mira asks for more detail instead of inventing it
- Request tracing and cost tracking in Langfuse

**Out of scope (version 1)**
- Sending emails or posting messages automatically (a PM reviews and sends)
- Live Trello or Jira connection (files are loaded from a repository for now)
- Editing project data or task statuses
- Resource allocation advice and competitive analysis
- Fine-tuning a custom model

## 3. Success criteria
| Criterion | Target | Evidence today |
|---|---|---|
| Baseline tests passed | 12 of 12 | 12 of 12 |
| Hallucinated tasks, risks, milestones or numbers | 0 | 0 in 12 traced runs |
| Task counts match the task board | 100% | 100% (counts computed in code) |
| Plan phases traceable to the timeline | 100% | 8 of 8 |
| Risks come from the register | 100% | 10 of 10 |
| Cost per request | Under $0.005 | About $0.001 (heaviest request) |
| PM time saved | At least 50% on plans and status reports | To be measured in the pilot |
| PM satisfaction in the pilot | 4 out of 5 or higher | To be measured in the pilot |

## 4. Timeline
| Phase | Weeks | Activities | Exit gate |
|---|---|---|---|
| 1. Build and test (complete) | Done | Workflow, 12 baseline tests, tracing, cost analysis | 12 of 12 tests pass |
| 2. Pilot | Weeks 1 to 4 | 3 PMs use Mira on 2 real projects; review every output before sharing | No fabricated item reaches a stakeholder; satisfaction at least 4 of 5 |
| 3. Expand | Weeks 5 to 8 | 10 PMs; add live task board connection; add human approval step for emails | Status accuracy stays 100%; hallucination rate stays at 0 |
| 4. Operate | Week 9 onward | Monthly metric review; prompt updates through change control | Metrics reported to the CTO each month |

## 5. Risks
| Risk | Impact | Mitigation |
|---|---|---|
| Mira invents a milestone, risk or task status | PM shares wrong information with a client | Counts, dates and filters are computed in code; prompts forbid inventing; vague input goes to a fixed Clarify reply; PM reviews before sharing |
| Source files are incomplete or inconsistent (for example, 3 vs 5 Done tasks, week vs date timeline) | Wrong plan or status | Mira states assumptions and flags gaps; data issues logged and fixed at the source |
| Privacy: project data sent to an external model | Client confidentiality breach | Use only approved projects; remove personal data; confirm the vendor's data handling terms before wider rollout |
| Prompt change breaks a working behavior | Quality drops without notice | Re-run all 12 baseline tests after every change |
| Low adoption | No time saved | Pilot with volunteers; collect feedback; show quick wins |
| Cost grows with usage | Budget overrun | Track tokens per request in Langfuse; alert above a daily limit |
| Platform or vendor change (for example, the Langfuse endpoint deprecation) | Observability breaks | Monitor vendor notices; keep the tracing node isolated |

## 6. Stakeholders
| Stakeholder | Interest | Role in the program |
|---|---|---|
| CTO | Productivity and risk control | Sponsor; approves rollout gates |
| PMs and TPMs | Time savings and accuracy | Primary users; give feedback |
| Data and ML engineers | Quality of prompts and data | Maintain the workflow; review traces |
| Client project teams (for example, ABCDE Ltd.) | Accurate reporting | Receive updates reviewed by PMs |
| Security and legal | Data handling and compliance | Review privacy and vendor terms |
| Project owner (Sathish Doraiswamy) | Delivery of the program | Runs the pilot and reports metrics |

## 7. Decision-making and governance
- **Human in the loop:** Mira drafts; the PM reviews and sends. Mira never sends messages by itself in version 1.
- **Change control:** any prompt or workflow change needs a re-run of the 12 baseline tests and a short note in the observation log before it is used.
- **Approvals:** the project owner approves prompt changes; the CTO approves moving to the next rollout phase.
- **Escalation:** any fabricated item found in an output is logged as an incident, traced in Langfuse, and fixed within 2 working days.
- **Review cadence:** weekly check-in during the pilot; monthly metric review afterward.

## 8. Rollout plan
1. **Pilot (Weeks 1 to 4):** 3 PMs, 2 real projects. Every output reviewed before sharing. Metrics: hallucination count, status accuracy, time saved, satisfaction.
2. **Expand (Weeks 5 to 8):** 10 PMs. Add the live task board connection and an approval step for emails. Short training session and a one-page user guide.
3. **Operate (Week 9 onward):** monthly review of the three production metrics, cost per user, and incidents. Decide on fine-tuning only if traces show repeated failures that prompts cannot fix.
