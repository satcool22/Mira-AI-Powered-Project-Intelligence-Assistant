# Mira Reflection and Next Steps (Q4)

Prepared by: Sathish Doraiswamy | Nexora Pvt. Ltd.

## 1. What the traces and test runs showed
Every request is traced in Langfuse (one trace per request, named mira-INTENT, with the request as input, the answer as output and a route tag). Across the 12 baseline tests and the re-runs:

- **The router and guardrails worked.** All four vague inputs (T2, T4, T6, T9) went to the fixed Clarify reply, which cost only the Orchestrator call ($0.0000764) and invented nothing.
- **Code-computed facts were never wrong.** Status counts matched the task board in every run (5 Done, 3 In Progress, 1 Blocked, 16 To Do).
- **The problems were in the model's wording and ranking, and were small:** a stray "Not specified" bullet (T1), an unrequested top-3 list (T3), a generic top 3 that ignored the Blocked task (T7), a missing To Do list and blocker reason (T10), and an overdue task missing from the email's risks (T12). Each was fixed with one prompt rule or by passing a missing field through.
- **The same question gave different answers.** The model runs at the default temperature (1), so the T7 ranking varied between runs.
- **Observability needed debugging.** The first endpoint returned 201 but showed nothing in Langfuse v4. I fixed it with the OTLP endpoint and a Final Reply node.

## 2. Improvement plan
| Priority | Improvement | Why |
|---|---|---|
| 1 | Lower the temperature (for example 0.2) and rank the top-N risks in code from task evidence | Removes the run-to-run variation seen in T7 |
| 2 | Automate the 12 baseline tests as a regression run after every prompt change, with pass or fail recorded as Langfuse scores | The check is manual today |
| 3 | Add a human approval step before any email draft leaves Mira | Keeps a PM in control of what stakeholders read |
| 4 | Connect the live task board (Trello or Jira) instead of CSV files | Removes stale data and the fixed as-of date |

Later: trace each model call as its own generation, and validate source files on load.

## 3. Evaluation and fine-tuning
Evaluation comes first. The 12 baseline tests, the traced re-runs and the production metrics (plan groundedness, status accuracy, risk relevance and hallucination rate) give a baseline of 12 of 12 passing with 0 hallucinated items. Every fix followed one loop: evaluate, find the failure, change the prompt, re-run.

Fine-tuning is not justified yet, because every failure was fixed with a prompt rule or a data change, and the system is cheap (about $0.0006 per plan, about $0.001 for the heaviest request). It becomes worth trying only if traces show a repeated failure that prompts cannot fix, for example the Risk Assessor ranking differently on identical input. Then I would collect 15 reviewed input and output pairs from Langfuse (10 detailed, 5 vague), fine-tune the Risk Assessor, and compare before and after on the same 12 tests.

## 4. Privacy considerations
- **What leaves Nexora:** project descriptions, task names, assignee names and risk text go to the model provider and are stored in Langfuse traces (long outputs are cut to 4,000 characters).
- **Controls before wider use:** approved projects only, with the client's consent; remove unneeded personal data (roles instead of names); confirm data retention and training terms of the model provider and Langfuse with security and legal; keep API keys in the credential store (the exported workflow contains none); restrict who can open traces; delete client traces on the contract's schedule.

## 5. Rollout for 10 PMs and TPMs
1. **Weeks 1 to 4, pilot:** 3 PMs on 2 real projects, every output reviewed. Track hallucination count, status accuracy, time saved and satisfaction.
2. **Weeks 5 to 8, expand:** 10 PMs, the live task board connection, an approval step for emails, a 30-minute walkthrough and a one-page guide.
3. **Week 9 onward, operate:** monthly review of metrics, cost per user (about $0.20 per month at 15 requests a day) and incidents; run the 12 tests after every prompt change.

**Gate between phases:** 0 fabricated items reaching a stakeholder, status accuracy at 100%, and satisfaction of at least 4 of 5.

## 6. My own reflection

**What surprised me**
- Observability: the Langfuse call returned success (201) but nothing appeared, and I only found the problem by checking the tool itself.
- I fixed it with the OTLP (OpenTelemetry Protocol) endpoint and a Final Reply node, and I now treat a success message as a claim to verify, not proof.
- The same question (T7) gave different top 3 risks between runs, and the first answer ignored the Blocked task.

**What I would do differently**
- Write the 12 test inputs and expected results first.
- Check the source files for mismatches (like 3 vs 5 Done tasks) before building.
- Set up tracing on day one, and lower the temperature from the start.
- Create the GitHub folders before uploading files.

**What I learned as a TPM**
- The model should explain facts, not compute them. Once code did the counting, the numbers were never wrong.
- One run is not proof, so I re-run the whole test set after every prompt change.
- A human should review anything a stakeholder will read.
- The AI is only as good as the data, so data checks come first.
