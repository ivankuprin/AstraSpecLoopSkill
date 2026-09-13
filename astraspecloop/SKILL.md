---
name: astraspecloop
description: "Astra Low coordinates solely from Luna xHigh reports; Luna reads, implements, reviews, and repairs code in a bounded SpecLoop. Use for AstraSpecLoop or this division of roles."
metadata:
  version: "1.2.2"
  short-description: "Astra Low leads; Luna xHigh builds and reviews"
---

# AstraSpecLoop

Deliver DISCOVER → SPEC → BUILD → REVIEW → REPAIR until verified or blocked.

## Roles and scope

- Lead: `gpt-6-astra`, reasoning `low`; decisions, plans, decomposition, dispatch, and acceptance based ONLY on compact Luna reports and user requirements. Astra MUST NOT search, open, read, or write project source, tests, diffs, manifests, migrations, configuration, or raw logs, including through tools or code excerpts in reports. All code inspection, verification, commands, and fingerprints belong to Luna. Astra may read required host/skill instructions and read/write its own plans and workflow records; this exception never permits inspecting project implementation.
- All workers: `gpt-5.6-luna`, reasoning `xhigh`. For `collaboration.spawn_agent`, explicitly set `model: "gpt-5.6-luna"`, `reasoning_effort: "xhigh"`, and `fork_turns: "none"`. Do not create user-visible tasks or let workers spawn agents.
- The skill cannot switch the calling model. Verify exposed settings; on mismatch request Astra Low. If settings are hidden, disclose the verification limitation. Unavailable required models or delegation means BLOCKED, without silent substitution.
- Higher-priority instructions prevail. Preserve user changes and agreed scope; never weaken requirements or checks for PASS. Existing authorization persists; external/destructive actions, commits, pushes, merges, and publishing require authorization.

## DISCOVER

Astra passes the provided working directory to Luna, which locates the root and applicable repository instructions. Before first dispatch, read [worker-contracts.md](references/worker-contracts.md); reuse it without rereading. Send only the role-specific contract and task context, never the conversation or all workflow instructions. Do not run preliminary repository searches as lead.

Luna returns compact Markdown describing behavior, symbols, callers, constraints, checks, risks, and unknowns without code excerpts. Stop once planning is supported. Astra plans from that report; pointers are for subsequent Luna tasks, never for Astra to open. Resolve gaps or disputed claims through focused Luna follow-ups or an independent Luna assessment. Missing evidence is unknown, not proof of absence.

## SPEC

Save `specs/<task-slug>.md` with SPEC, EVIDENCE, REVIEW, and a LOOP state pointer. Store worker `.md` reports and authoritative `state.json` in `specs/<task-slug>/`, unless the repository requires another location. Record the original request and accepted clarifications, current/desired behavior, scope, assumptions, stable requirement IDs, failure cases, acceptance criteria, and required/optional checks derived from project evidence.

Use conservative non-material assumptions; ask for material missing decisions. Pause before BUILD when requested or necessary. Changes to agreed behavior require approval and invalidate affected evidence.

If the user requested only a plan, deliver that plan and stop; do not implement or claim an implementation PASS.

Assign tasks with dependencies, file ownership, acceptance checks, and shared interface contracts. Batch small related work. Parallelize only independent changes with compatible contracts and disjoint write ownership; serialize shared edits and delegate integration checks.

When concurrent project writes are actually worthwhile, delegate separate Git worktree setup to Luna for each writer; follow the worktree contract in the worker reference. Sequential work and read-only review need no extra worktree. If isolation or integration cannot be established safely, serialize the work instead.

## BUILD

Luna makes minimal changes and runs affected checks with meaningful tests. Reuse builders for unfinished assigned work, not fixes discovered by reviewers. Reports map requirements to observed changes/checks. Astra decides from reports and delegates every integration inspection or edit.

Luna must not duplicate running commands or repeat valid checks without cause. After writers finish, delegate the complete required gate on stable state; freeze writes during final review. Subsequent changes invalidate affected evidence.

## REVIEW → REPAIR

Start each review with a NEW Luna, `fork_turns: "none"`. Review ALL active changes, not just the last fix, against the original request and spec. Include relevant risk focus: races, transaction boundaries, validation, error handling, compatibility, or other implementation-specific concerns. Preserve unrelated user changes. Astra assesses compact findings; unresolved factual disagreements go to Luna, never lead code inspection.

Before repair, read [state-and-repair.md](references/state-and-repair.md). Assign confirmed fixes to THE SAME Luna that found them, never the original builder. After its fixes/checks, launch a NEW context-free reviewer of all active changes. A reviewer that edited code cannot provide final acceptance for those edits. Continue until a fresh reviewer reports no actionable findings and readiness within the agreed scope, or the bounded stopping conditions apply: three cumulative repair rounds, oscillation, or two attempts without progress. Evaluate PASS before the limit.

## State and verdict

After each phase/round, Astra updates `state.json` with phase, Luna-reported baseline/fingerprints, check statuses/results, report paths, repair count/round ID, finding attempts, and pending work. JSON is only for cycle state; plans and worker reports use Markdown. On resume/strict mode read the state reference and delegate state verification to Luna.

Return `ASTRASPECLOOP PASS` only when all requirements, criteria, and required checks pass on the current state, with complete evidence and no unresolved blocker, correctness/safety finding, known regression, or unapproved scope change. Otherwise return `ASTRASPECLOOP BLOCKED` for exhausted repairs or unavailable decisions, authorization, capabilities, infrastructure, checks, or conflicting requirements.

Final report: outcome, record path, repair count, reviewer isolation, model verification limitations, required-check results, and exact next action if blocked.
