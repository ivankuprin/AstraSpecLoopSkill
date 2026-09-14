# State, repair, and resume

Read when starting repairs, resuming work, or using strict mode. The main file defines ordinary persistence and acceptance.

## Repair accounting

One round addresses a review's confirmed finding set, performs verification, and ends with the next independent review. Initial BUILD and its first review count as zero. Multiple workers or subtasks in the same cycle do not add rounds.

The finding reviewer owns its repairs after lead dispatch. Never route them back to the original builder. If that reviewer is unavailable, start a fresh Luna to independently re-establish the findings before owning their fixes. Each repair cycle ends with a different fresh Luna reviewing all active changes.

At round start increment the cumulative count once; persist its ID, finding IDs, and in-progress status before dispatch. Resume unfinished rounds under the same ID. Track each finding's attempted remedy and verification outcome separately from agent launches. A follow-up message alone is not a new attempt; a remedy tested against closing criteria is.

After review, record the outcome and check PASS first. Three rounds permit three repair-and-review cycles; successful round three passes. Otherwise block at the limit, on oscillation, or after two attempts without measurable progress on a finding. Preserve unresolved work and the exact next action. Do not reset counters by renaming findings or restarting the task.

## Evidence and resume

Keep one authoritative `state.json`, maintained by the lead from Luna's Markdown reports. It holds phase, baseline, fingerprints, check statuses/results, report paths, cumulative repair count, active round ID/status, per-finding attempts/outcomes, and pending actions. Plans and all worker reports remain Markdown; do not duplicate their prose in JSON. Fingerprints cover the spec, baseline, relevant tracked/untracked inputs, and required-check plan. Exclude generated reports and cycle state from code fingerprints. Record covered paths and digest method; include shared dependencies affecting verification.

When resuming an older record, preserve its counters, findings, and evidence pointers while moving current cycle state to `state.json`. Do not rerun work or rewrite historical reports merely to change formats.

Luna compares saved/current state on resume and returns a compact report; the lead never reads project files or computes code fingerprints. Invalidate affected evidence/reviews with reasons; delegate expanded validation when impact is uncertain. Stop writers before the final gate and review. Luna confirms unchanged state before acceptance; changes require revalidation. Missing, interrupted, or unobserved outcomes never count as PASS.

Strict mode stores spec, evidence, and review in separate Markdown files and loop state in `state.json`, with the same fingerprints, repair limits, and independent review requirement. Record pending actions before pauses or handoffs.
