# Cost analysis and production metrics

## Cost analysis (gpt-4o-mini, $0.15 per 1M input tokens, $0.60 per 1M output tokens)

| Call | Input tokens | Output tokens | Cost per call | Source |
|---|---|---|---|---|
| Orchestrator | 393 | 29 | $0.0000764 | Measured (n8n output, usage field) |
| Status Reporter (all 25 tasks) | 2,480 | 973 | $0.0009558 | Measured |
| Planner | ~1,500 | ~900 | ~$0.00077 | Estimated from prompt size |
| Risk Assessor | ~2,200 | ~1,100 | ~$0.00099 | Estimated |
| Milestone Tracker | ~1,200 | ~300 | ~$0.00036 | Estimated |
| Stakeholder Updater | ~1,300 | ~250 | ~$0.00035 | Estimated |

Every request pays for the Orchestrator plus one specialist. Vague requests (clarify route) pay only for the Orchestrator: $0.0000764.

| Cost item | Estimate |
|---|---|
| Heaviest request (status report, all sprints) | $0.00103 |
| Typical mix, 15 requests per user per day (3 clarify, 2 plan, 2 risk, 3 status, 3 milestone, 2 comms) | ~$0.0093 per user per day |
| Per user per month (22 working days) | ~$0.20 |
| Worst case (15 heaviest requests per day) | ~$0.016 per day, ~$0.34 per month |
| Data sources (GitHub raw files) | Free |
| Observability (Langfuse Hobby tier) | Free tier |
| Workflow platform (n8n Cloud) | Plan cost not included |

Prices should be re-checked against OpenAI's current price list before relying on these figures.

## Production metrics

| Metric | What it measures | How to measure | Baseline (12-test run) |
|---|---|---|---|
| Plan groundedness | Share of plan phases and milestones that appear in the timeline file | Compare plan rows against project_timeline.csv | 8 of 8 phases, 100% |
| Status accuracy | Share of task counts that match the task board | Counts are computed in code and compared with the CSV | T5 and T10 counts correct, 100% |
| Risk relevance | Share of listed risks that come from the risk register, with evidence links where tasks support them | Compare risks against project_risks.csv | 10 of 10 from the register, 100% |
| Hallucination rate (trust) | Share of runs mentioning a task, risk or milestone not in the source files | Review of traced runs in Langfuse | 0 of 12 runs |
| Time saved (usefulness) | Minutes saved per PM per week against the manual process | Survey: plans 3-4 hrs, status reports 1-2 hrs manually | To be measured in the pilot |
