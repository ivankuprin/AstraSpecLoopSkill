---
name: astraspecloop
description: "Astra Low leads a bounded SpecLoop with Luna Max research, build, and review workers. Use for AstraSpecLoop or this requested division of agent roles."
metadata:
  version: "1.1.0"
  short-description: "Astra Low leads; Luna Max builds and reviews"
---

# AstraSpecLoop

Deliver DISCOVER → SPEC → BUILD → REVIEW → REPAIR until verified or blocked.

## Roles and scope

- Lead: `gpt-6-astra`, reasoning `low`; scope, targeted inspection, specification, decomposition, dispatch, triage, acceptance. Only Luna edits implementation, tests, migrations, and project configuration; Astra may write workflow records.
- All workers: `gpt-5.6-luna`, reasoning `max`. For `collaboration.spawn_agent`, set `model`, `reasoning_effort`, and `fork_turns: "none"` explicitly. Do not create user-visible tasks or let workers spawn agents.
- The skill cannot switch the calling model. Verify exposed settings; on mismatch request Astra Low. If settings are hidden, disclose the verification limitation. Unavailable required models or delegation means BLOCKED, without silent substitution.
- Higher-priority instructions prevail. Preserve user changes and agreed scope; never weaken requirements or checks for PASS. Existing authorization persists; external/destructive actions, commits, pushes, merges, and publishing require authorization.

## DISCOVER

Astra locates the root and applicable top-level instructions, then delegates primary exploration to one Luna. Before first dispatch, read [worker-contracts.md](references/worker-contracts.md); reuse it without rereading. Send only the worker's role-specific contract and task context, never the full conversation or all workflow instructions.

Luna returns a compact JSON map of symbols, callers, instructions, checks, risks, and unknowns. Stop discovery when entry points, affected contracts, and verification options support a plan; continue only for a named gap. Astra validates important claims with targeted inspection. Missing evidence is unknown, not proof of absence; delegate focused follow-up searches instead of repeating broad research.

## SPEC

Save `specs/<task-slug>.md` with SPEC, EVIDENCE, REVIEW, and LOOP sections, and reports in `specs/<task-slug>/`, unless the repository requires another location. Record the original request and accepted clarifications, current/desired behavior, scope, assumptions, stable requirement IDs, failure cases, acceptance criteria, and required/optional checks derived from project evidence.

Use conservative non-material assumptions; ask for material missing decisions. Pause before BUILD when requested or necessary. Changes to agreed behavior require approval and invalidate affected evidence.

Assign tasks with dependencies, file ownership, acceptance checks, and shared interface contracts. Batch small related work. Parallelize only independent changes with compatible contracts and disjoint write ownership; serialize shared edits and delegate integration checks.

## BUILD

Luna makes minimal changes and runs affected checks with meaningful tests when warranted. Reuse the same worker for related follow-ups and repairs while its context remains useful; give changed scope/state explicitly. Replace it when unavailable or unsuitable. Reports map requirements to changes and observed checks. Astra inspects relevant diff slices and delegates integration edits.

Do not duplicate running commands or repeat successful checks without stale evidence or a concrete concern. Once all writers finish, run the complete required gate against stable project state; keep writers stopped through final review. Subsequent changes invalidate affected checks and review.

## REVIEW → REPAIR

Always use a fresh read-only Luna reviewer. Supply the original request, accepted clarifications, spec, actual state, and check evidence. Review both spec completeness and implementation, including relevant adjacent contracts; fresh context is not proof against shared model blind spots. Astra validates findings and records reasons for rejecting unsupported ones.

For confirmed findings, read [state-and-repair.md](references/state-and-repair.md) before repairs. Delegate fixes, rerun stale required checks, and obtain fresh independent review. Maximum three cumulative repair rounds; stop earlier for oscillation or two unsuccessful attempts on a finding without progress. Evaluate PASS before the limit, including after round three.

## State and verdict

After every phase and round, save phase, baseline, spec/project/check-plan fingerprints, results, report paths, repair count, and pending work. Update compact current state; keep detailed history in reports. Read the state reference on resume or for strict mode; invalidate stale evidence before continuing.

Return `ASTRASPECLOOP PASS` only when all requirements, criteria, and required checks pass on the current state, with complete evidence and no unresolved blocker, correctness/safety finding, known regression, or unapproved scope change. Otherwise return `ASTRASPECLOOP BLOCKED` for exhausted repairs or unavailable decisions, authorization, capabilities, infrastructure, checks, or conflicting requirements.

Final report: outcome, record path, repair count, reviewer isolation, model verification limitations, required-check results, and exact next action if blocked.
