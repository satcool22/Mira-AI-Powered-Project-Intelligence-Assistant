# T10 Output: Project status summary (all sprints, as of 2026-04-22)

Input: Summarize the current project status: how many tasks are done, in progress, and to do?

| Status | Count |
|---|---|
| Done | 5 (T001 to T005) |
| In Progress | 3 (T006, T007, T025) |
| Blocked | 1 (T024) |
| To Do | 16 (T008 to T023) |
| Total | 25 |

Counts match the task board CSV. Note: the problem statement says "3 Done", but the CSV has 5 Done.

Blocked: T024, Security review of AI infrastructure, James Wong, due 2026-05-26 (waiting for security team availability).
Overdue: T006 (due 2026-04-21). Due within 7 days: T007, T025 (both 2026-04-28).
Overall health: RED (T024 blocked).

## Observation log
- Run 1: counts correct, but Blocked items showed only "T024" with "Blocker: None", because the task description was not passed to the model. Fix: added the description field in Assemble Context, plus prompt rule 6 requiring ID, name, assignee, due date and blocker.
- Run 2: blocker details shown and counts correct, but the To Do task list was omitted. Fix: prompt rule 7, every status must be listed.
- Run 3 (after rule 7): all statuses listed, including all 16 To Do tasks (T008 to T023).
