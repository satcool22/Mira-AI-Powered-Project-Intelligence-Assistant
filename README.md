# Mira: AI-Powered Project Intelligence Assistant

Capstone project, Applied Agentic AI for PMs/TPMs.

## What Mira is
(Write your 3-sentence problem summary here: the problem, the user, and the biggest risk.)

## Architecture
(Diagram goes here: docs/architecture.png)

Pattern: Router / Dispatcher.
Agents: Orchestrator, Planner, Risk Assessor, Status Reporter, Milestone Tracker, Stakeholder Updater.

## Tools used
| Category | Tool | Why |
|---|---|---|
| Workflow | n8n | |
| LLM | OpenAI gpt-4o-mini | |
| Observability | Langfuse | |
| Delivery | Zapier | |

## How to run
1. Import the workflow JSON from /workflow into n8n.
2. Add your OpenAI and Langfuse credentials (never commit keys).
3. Set config values: as_of_date, project_start, current_sprint.
4. Load the files from /data and send a request.

## Repo structure
- /data: project input files
- /workflow: n8n workflow JSON exports
- /docs: writeup, diagram, charter, reflection, project notes
- /results: baseline test results and screenshots

## Baseline test results
(Link to results/baseline_results.md once T1 to T12 are run.)

## Known limitations
(See docs/notes.md for data discrepancies found in the source files.)
