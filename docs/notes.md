# Project Notes

## Context
- Company: Nexora Pvt. Ltd. (London, est. 2018, 120+ staff, Europe/Asia clients)
- Client project: ABCDE Ltd. AI Adoption (logistics), 6 months, 8 phases
- My role: AI PM/TPM; works with data scientists, ML engineers, developers, stakeholders
- Mira user: Nexora PM/TPM managing multiple projects simultaneously

## Config (used by the workflow)
- as_of_date: 2026-04-22
- project_start: 2026-04-06 (Week 1 = Mon 6 Apr 2026)
- current_sprint: Sprint 3

## Data discrepancies found
1. Problem statement says 3 Done tasks; CSV has 5 Done, 3 In Progress, 1 Blocked, 16 To Do (25 total).
2. Task board ends ~Week 18 (4 Aug); timeline runs to Week 24.
3. Risk CSV has no probability or severity scores, so "top 3" requires a stated ranking rationale.
4. Timeline is in weeks, not dates; a project start date must be assumed.
   
## Observability setup (Langfuse)
- Platform: n8n Cloud; tracing: Langfuse Cloud (US region, v4.72), project "Mira".
- n8n has no built-in Langfuse node, so traces are sent with an HTTP Request node using Basic Auth (public key / secret key).
- First attempt: legacy /api/public/ingestion endpoint. It returned 201 but the trace was not visible in the v4 Tracing table (v4 is observation-centric, and a bare trace-create event has no observation).
- The legacy endpoint is also deprecated; Langfuse stops accepting traces on it on 2026-11-16.
- Fix: switched to the OTLP endpoint /api/public/otel/v1/traces with a JSON body (resourceSpans) and langfuse.observation.input / langfuse.observation.output attributes. Trace appeared after about a minute.
- Lesson for Q4: check the ingestion API version when connecting observability; a 2xx response does not guarantee the data is visible.
- Evidence: results/langfuse_test_trace.png

## Tracing in the Mira workflow
- One "Trace" HTTP Request node (OTLP endpoint, Basic Auth) receives the output of all six branches (plan, risk, status, milestone, comms, clarify), so every request creates one trace in Langfuse.
- Each trace is named mira-INTENT, carries the request as input and Mira's answer as output, and is tagged with the route taken (for example route:clarify).
- Issue found: because Trace is the last node, the chat showed Langfuse's ingestion receipt instead of the answer. Fix: a "Final Reply" node after Trace returns the answer from whichever Reply node ran.
- Limitation: the model calls themselves are not traced as separate generations; token usage is read from the n8n execution view instead.
