# AI Agent Use Cases for Nexora PMs and TPMs (Q1)

Prepared by: Sathish Doraiswamy | Nexora Pvt. Ltd.

Nexora's PMs and TPMs run several projects at once with very little support. These three use cases target the work that costs them the most time and is easiest to get wrong.

| # | Use case | Status | Pattern |
|---|---|---|---|
| 1 | Mira: plans, risk assessments, status reports and updates from project files | Built and tested (12 of 12 baseline tests) | Router |
| 2 | Meeting notes to action items and decision log | Proposal | Sequential pipeline |
| 3 | Workload and dependency advisor | Proposal | Router with code-computed inputs |

## Use case 1: Mira, project intelligence assistant

**Pain point.** Building a plan from a description takes 3 to 4 hours, a weekly status report takes 1 to 2 hours, and risk assessments are done differently from project to project. Missed milestones are discovered only in retrospectives.

**Agentic solution.** An Orchestrator agent reads each request, checks that there is enough data, and routes it to a specialist: Planner, Risk Assessor, Status Reporter, Milestone Tracker or Stakeholder Updater. Code computes every count, date and filter, so the model only writes the narrative. Vague requests get a fixed "please share scope, goals, timeline and team" reply instead of invented content.

**Inputs and outputs.**
- Inputs: project description, timeline (CSV), risk register (CSV), task board (CSV), the user's request, and an as-of date.
- Outputs: a phase plan with dates, a risk matrix or top-N risks with task evidence, a status report with counts and health (RED, AMBER or GREEN), blocked and at-risk tasks, upcoming milestones, or a sprint email.

**Success metrics.**
| Metric | Target | Baseline |
|---|---|---|
| Groundedness: plan phases, risks and milestones traceable to the files | 100% | 8 of 8 phases; 10 of 10 risks |
| Status accuracy: task counts match the task board | 100% | 100% (computed in code) |
| Hallucination rate: runs mentioning something not in the files | 0% | 0 of 12 runs |

**Knowledge base needed.** The four project files above, the as-of date and project start date, and Nexora's standard report formats.

## Use case 2: Meeting notes to action items and decision log

**Pain point.** After every meeting, a PM spends 30 to 60 minutes turning notes or a transcript into action items, owners, due dates and decisions. Items are lost, owners are unclear, and nobody can later find why a decision was made.

**Agentic solution.** A fixed three-step pipeline, because every meeting goes through the same stages: (1) an Extractor agent lists actions, decisions and open questions, quoting the sentence each came from; (2) a Validator agent marks each item as stated, ambiguous or missing an owner or date, and never fills in a missing owner or date; (3) a Formatter agent produces the action list, the decision log and a follow-up message for the PM to review and send. Items that cannot be matched to a quote are dropped.

**Inputs and outputs.**
- Inputs: meeting transcript or notes, attendee list, the project's task board, and previous decision log.
- Outputs: action items with owner and due date (or "not stated"), a decision log entry, a list of open questions, and a draft follow-up email.

**Success metrics.**
| Metric | Target |
|---|---|
| Extraction completeness: share of real action items captured (checked against a PM's manual list) | At least 90% |
| Traceability: share of extracted items backed by a quote from the notes | 100% |
| PM time saved per meeting | At least 25 minutes |

**Knowledge base needed.** Meeting transcripts or notes, attendee and role list, the project's open task list, and the running decision log.

## Use case 3: Workload and dependency advisor

**Pain point.** Task boards show who is assigned what, but not whether someone is overloaded or whether one late task will push others. PMs find out when a deadline is missed. In the ABCDE board, for example, assignments range from 4 tasks (Ravi Kumar) to 6 tasks (Arjun Mehta), and a blocked security review (T024) sits in the same sprint as other work, but nobody sees the combined effect.

**Agentic solution.** Code computes tasks per person per sprint, overdue and due-soon counts, and which tasks share a sprint with a blocked task. A Router then sends a request such as "Is anyone overloaded in Sprint 5?" or "What is affected if T024 slips?" to an Advisor agent that explains the computed numbers and suggests options (rebalance, reorder, escalate), always labeled as suggestions. Dependencies that are not in the data are labeled as inferences, never as facts.

**Inputs and outputs.**
- Inputs: task board with assignee, sprint, due date and status; optional team capacity (days available per person); the user's question.
- Outputs: a workload table per person and sprint, an at-risk list, and two or three suggested actions with the numbers behind each.

**Success metrics.**
| Metric | Target |
|---|---|
| Accuracy: workload numbers match the task board | 100% (computed in code) |
| Early warning: at-risk tasks flagged at least 5 working days before the due date | At least 80% of tasks that later slip |
| Suggestion usefulness: share of suggestions a PM rates useful or acted on | At least 60% |

**Knowledge base needed.** The task board export, team capacity and leave calendar, the project timeline, and any known task dependencies.

## Why these three
All three share the same design lesson from Mira: compute facts in code, let the model write and explain, ground every output in supplied data, and ask for more information when the data is not enough. Use case 1 is built and measured; use cases 2 and 3 would reuse the same router, tracing and test approach, and each would start with its own baseline test set before use.
