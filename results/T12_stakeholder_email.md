# T12 Output (after prompt fix): Stakeholder update email, Sprint 2

Input: Generate a stakeholder update email summarizing Sprint 2 progress.

**Subject:** Sprint 2 Progress Update

Dear Team,

As of April 22, 2026, Sprint 2 has made considerable progress, with 3 tasks in total: 2 completed and 1 currently in progress.

**Completed Tasks**
- T004 - Audit existing data infrastructure
- T005 - Map current logistics workflows

**In Progress**
- T006 - Identify data gaps and quality issues (Owner: Priya Nair, due April 21, 2026, past due)

**Risks and Next Steps**
- T006 - Identify data gaps and quality issues (Owner: Priya Nair, due April 21, 2026). Suggestion: prioritize this task to accelerate completion and mitigate any impact on subsequent phases.

Thank you for your continued support.

[Sender name], Nexora Project Management

## Observation log
- Run 1: Sprint 2 only, counts correct, no Sprint 3+ tasks. But Risks and next steps said "None identified" although T006 was overdue, and the sign-off used "[Your Name]". Fix: prompt rule 5 (name overdue tasks in Risks) and rule 6 (placeholder sign-off).
- Run 2 (after fix): overdue task T006 named in Risks with a labeled suggestion; sign-off placeholder correct.
