# State, repair, and resume

Read when starting repairs, resuming work, or using strict mode. The main file defines ordinary persistence and acceptance.

## Repair accounting

One round addresses a review's confirmed finding set, performs verification, and ends with the next independent review. Initial BUILD and its first review count as zero. Multiple workers or subtasks in the same cycle do not add rounds.

At round start increment the cumulative count once; persist its ID, finding IDs, and in-progress status before dispatch. Resume unfinished rounds under the same ID. Track each finding's attempted remedy and verification outcome separately from agent launches. A follow-up message alone is not a new attempt; a remedy tested against closing criteria is.

After review, record the outcome and check PASS first. Three rounds permit three repair-and-review cycles; successful round three passes. Otherwise block at the limit, on oscillation, or after two attempts without measurable progress on a finding. Preserve unresolved work and the exact next action. Do not reset counters by renaming findings or restarting the task.

## Evidence and resume

Store compact current state and pointers to detailed reports, not repeated history. Fingerprints cover the spec, baseline, relevant tracked/untracked inputs, and required-check plan. Exclude generated workflow reports from code fingerprints to avoid invalidating checks merely by recording them. Record covered paths and digest method; include shared dependencies affecting verification.

Compare saved and current state on resume. Invalidate affected evidence/reviews with reasons; expand validation when impact is uncertain. Stop writers before the final gate and review. Confirm covered state remains unchanged before acceptance; changes require revalidation. Missing, interrupted, or unobserved check outcomes never count as PASS.

Strict mode stores spec, evidence, review, and loop state in separate files with the same fingerprints, repair limits, and independent review requirement. Record pending actions before pauses or handoffs.
