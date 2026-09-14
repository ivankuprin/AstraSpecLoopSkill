---
name: astraspecloop
description: "The user-selected main model coordinates from Luna xHigh reports; Luna researches, implements, reviews, and repairs code in a bounded SpecLoop."
metadata:
  version: "1.4.2"
  short-description: "Your selected model leads; Luna xHigh works"
---

# AstraSpecLoop

Deliver DISCOVER → SPEC → BUILD → REVIEW → REPAIR until verified or blocked.

## Roles

The lead keeps the user's selected model and reasoning effort, including when it is Luna. It decides from user requirements and compact worker reports only. It must not search, read, or write project code, tests, diffs, configuration, or raw logs. Luna performs project inspection, commands, changes, and fingerprints. The lead may read required host/skill instructions and maintain its plans and workflow records.

Every worker uses `gpt-5.6-luna`, `reasoning_effort: "xhigh"`, `fork_turns: "none"`. Do not create user-visible tasks or allow worker delegation. Missing worker capabilities means BLOCKED; never block merely because the main model differs or is hidden.

Preserve user changes, accepted scope, and authorization boundaries. Never weaken checks for PASS. Higher-priority instructions prevail; commits, pushes, merges, publishing, and destructive/external actions require authorization.

## DISCOVER → SPEC

Read [worker-contracts.md](references/worker-contracts.md) before first dispatch. Send the working directory, relevant task context, and only the applicable worker contract. Luna locates instructions and implementation; the lead performs no preliminary repository search.

Require an observable stopping criterion: enough entry points, behavior, contracts, constraints, and verification options to plan. Unknowns trigger focused follow-ups, not repeated broad discovery.

Save `specs/<task>.md` with the request, accepted clarifications, requirement IDs, scope, assumptions, acceptance criteria, and check plan. Keep worker Markdown reports and authoritative `state.json` in `specs/<task>/`, or the repository-approved location. Plan-only requests stop with a plan, without implementation PASS.

Honor requested approval checkpoints before dependent work. Material changes to agreed behavior or scope require a user decision and invalidate affected acceptance/review evidence. Make conservative non-material assumptions without adding approval steps or asking again for authorization already given.

Luna records the starting state, pre-existing changes, and task-owned changes. Review scope is ALL changes produced for this task, including integrated work and repairs. Inspect surrounding or pre-existing code only for relevant interactions; do not audit or repair unrelated changes. If attribution is unclear, Luna resolves it before review.

Assign coherent tasks, file ownership, shared interfaces, and completion checks; batch small related work. Default to sequential writes. Only when parallel writes are worthwhile, read [parallel-worktrees.md](references/parallel-worktrees.md).

## BUILD → REVIEW → REPAIR

Luna implements and runs meaningful affected checks. Reuse builders for unfinished work. Stop writers before final validation. Existing full-gate results count if Luna confirms identical relevant code, dependencies, environment, and check plan; do not rerun them just because the phase changed. Revalidate affected evidence after changes or concrete uncertainty.

Validation is local-first: green GitHub CI is not required. Luna maps required behavior and relevant environment conditions to equivalent local checks. Billing or missing hosted runs alone cannot block implementation PASS. Record hosted checks as unavailable/not run. Essential uncovered behavior still blocks; explicit hosted-CI requests and branch protection remain separate constraints, never permission to bypass them.

Launch a NEW Luna reviewer with task requirements, verified navigation/check pointers, and concrete risk scenarios. It independently reads all task changes and relevant contracts. Findings need a trigger, consequence, code-linked reasoning or reproduction, and closing verification; speculation is a question, not a confirmed defect.

Assign confirmed repairs to the reviewer that found them, never the original builder. After fixes, a NEW context-free reviewer verifies the whole task result. The repairer cannot independently accept its own fixes. Read [state-and-repair.md](references/state-and-repair.md) before repair, resume, or strict mode.

## Limits and acceptance

Before each assignment, record its completion criterion and attempt. Progress means evidence resolves a named gap/hypothesis, satisfies a criterion, or verifies a fix; rewording reports or repeating searches is not progress. Two consecutive attempts at the same objective without progress trigger BLOCKED in any phase. Independent subtasks are not retries. Stop oscillation. Allow three cumulative repair rounds; check PASS before the limit.

After each phase/attempt, save compact state and evidence pointers. Bundle fingerprints with existing Luna work; never launch an agent solely for bookkeeping. Reuse unchanged fingerprints; mark unverified updates pending until the next substantive worker verifies them, before acceptance. Incomplete reports require focused correction, not speculative acceptance.

Return `ASTRASPECLOOP PASS` only with complete current evidence, all requirements/checks passed, fresh clean review, and no unresolved task defect or blocker. Otherwise report `ASTRASPECLOOP BLOCKED` with the specific reason. Final: outcome, record path, repair count, reviewer isolation, check results, limitations, and next action if blocked.
