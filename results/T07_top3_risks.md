# T7 Output (after prompt fixes): Top 3 risks for the ABCDE Ltd project

Input: What are the top 3 risks for the ABCDE Ltd project based on the risk data?

1. **R09 - Security Vulnerabilities.** Linked to Blocked task T024 ("Security review of AI infrastructure"). The security review is critical to mitigating data breaches and operational risks from adversarial attacks on AI models.
2. **R01 - Data Quality & Availability.** Linked to Overdue task T006 ("Identify data gaps and quality issues"). Addressing incomplete or unstructured data is urgent to avoid poor model performance.
3. **R05 - Ethical & Legal Concerns.** Linked to task T025 ("Legal review of data usage"), due within 7 days. The ongoing legal review addresses bias and data privacy risks before implementation.

Notes: The risk register has no probability or severity scores, so any ranking is a judgment based on the evidence above.

## Observation log
- Run 1 (before fixes): the T3 request also produced an unrequested top-3 list. Fix: rule 6, "answer only what was asked".
- Run 2: the top 3 were R01, R02 and R04. Only R01 cited a task, R09 (Blocked T024) was ignored, and the choice differed from the T3 run. Fix: rule 7, rank by task evidence (Blocked, then Overdue, then Due soon).
- Run 3 (after fixes): R09, R01, R05, each with task evidence.
