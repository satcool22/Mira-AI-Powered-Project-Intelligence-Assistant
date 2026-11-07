# Mira Reflection and Next Steps (Q4)

Prepared by: Sathish Doraiswamy | Nexora Pvt. Ltd.

## 1. What the traces and test runs showed
Every request is traced in Langfuse (one trace per request, named mira-INTENT, with the request as input, the answer as output and a route tag). Reading outputs and traces across the 12 baseline tests and the re-runs showed five things:

- **The router and the guardrails worked.** All four vague inputs (T2, T4, T6, T9) went to the fixed Clarify reply, which cost only the Orchestrator call ($0.0000764) and invented nothing.
- **Code-computed facts were never wrong.** Task counts, overdue tasks and date windows came from code, so status counts matched the task board in every run (5 Done, 3 In Progress, 1 Blocked, 16 To Do).
- **The model was where the problems appeared, and they were small and fixable:** a stray "Not specified" bullet (T1), an unrequested top-3 list (T3), a generic and inconsistent top 3 that ignored the Blocked task (T7), an omitted To Do list and a missing blocker reason (T10), and an overdue task missing from the email's risk section (T12). Each was fixed with one prompt rule or by passing a missing field through.
- **The same question gave different answers.** T7 returned R01, R02, R04 in one run and a different ranking after the fix. The model runs at the default temperature (1), so wording and choices can vary between runs.
- **Observability itself needed debugging.** The first tracing endpoint returned 201 but showed nothing in Langfuse v4, and adding the Trace node made the chat show the ingestion receipt instead of the answer. Both were fixed (OTLP endpoint, Final Reply node).

## 2. Improvement plan
| Priority | Improvement | Why |
|---|---|---|
| 1 | Lower the model temperature (for example 0.2) and compute the top-N risk ranking in code from task evidence | Removes the run-to-run variation seen in T7 |
| 2 | Automate the 12 baseline tests as a regression run after every prompt change, and record pass or fail as Langfuse scores | Today the check is manual |
| 3 | Add a human approval step before any email draft leaves Mira | Keeps a PM in control of what stakeholders read |
| 4 | Connect the live task board (Trello or Jira) instead of CSV files | Removes stale data and the fixed as-of date |
| 5 | Trace each model call as its own generation to see tokens and latency per agent | Today token use is read from the n8n execution view |
| 6 | Validate source files on load (for example, dates in order, statuses from an allowed list) and report inconsistencies | The ABCDE data had 3 vs 5 Done tasks and a board shorter than the timeline |

## 3. Evaluation and fine-tuning
Evaluation comes first. The 12 baseline tests, the traced re-runs and the three production metrics (plan groundedness, status accuracy, risk relevance, plus hallucination rate) already give a measurable baseline of 12 of 12 passing with 0 hallucinated items. Every fix in this project followed the same loop: evaluate, find a failure, change the prompt, re-run, and confirm.

Fine-tuning is not justified yet, because every failure was fixed with a prompt rule or a data change, and the system is cheap (about $0.001 for the heaviest request). It becomes worth trying only if the traces show a repeated failure that prompts cannot fix, for example the Risk Assessor ranking differently on identical input. In that case the plan is: collect 15 reviewed input and output pairs from Langfuse (10 detailed and 5 vague ones where the correct answer is to ask for more information), fine-tune the Risk Assessor or the Orchestrator, and compare before and after on the same 12 tests.

## 4. Privacy considerations
- **What leaves Nexora:** project descriptions, task names, assignee names and risk text are sent to the model provider and stored in Langfuse traces (inputs and outputs, with long outputs cut to 4,000 characters).
- **Controls before wider use:** use only approved projects; remove personal data that is not needed (for example, use roles instead of names where possible); confirm the model provider's and Langfuse's data retention and training terms with Nexora's security and legal teams; keep API keys in the platform's credential store and never in the exported workflow (checked: the exported JSON contains no keys); restrict who can open traces.
- **Client confidentiality:** a project's files go through Mira only with the client's consent, and traces for client projects are deleted on the contract's schedule.

## 5. Rollout for 10 PMs and TPMs
1. **Weeks 1 to 4, pilot with 3 PMs on 2 real projects.** Every output is reviewed before sharing. Track hallucination count, status accuracy, time saved and satisfaction.
2. **Weeks 5 to 8, expand to 10 PMs.** Add the live task board connection and the approval step for emails. Give a 30-minute walkthrough and a one-page guide that explains what to ask, what Mira will refuse to do and why.
3. **Week 9 onward, operate.** Monthly review of the metrics, cost per user (about $0.20 per user per month at 15 requests per day) and incidents. Run the 12 baseline tests after every prompt change.

**Go or no-go gate between phases:** 0 fabricated items reaching a stakeholder, status accuracy at 100%, and satisfaction at least 4 of 5.
