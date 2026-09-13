---
name: astraspecloop
description: "AstraSpecLoop: bounded DISCOVER → SPEC → BUILD → REVIEW → REPAIR with Astra Low as lead and Luna Max research, implementation, and review workers. Use when the user requests AstraSpecLoop or this division of agent roles."
metadata:
  short-description: "Astra Low leads; Luna Max builds and reviews"
---

# AstraSpecLoop

Deliver verified changes using the SpecLoop workflow, with inexpensive worker discovery and implementation and focused lead reasoning.

## Roles and model contract

- Lead: `gpt-6-astra`, reasoning `low`. Own scope, targeted code inspection, specification, task decomposition, dispatch, finding triage, and final acceptance. The lead may write the spec and loop records, but must not write or patch implementation code, tests, migrations, or project configuration; delegate those edits to Luna.
- Every research, build, repair, and review worker: `gpt-5.6-luna`, reasoning `max`.
- A skill is instructions, not a model switch. The calling task must already use Astra Low. Verify settings when exposed; never claim they were changed by reading this file. If the host reports a mismatch, ask the user to select Astra Low before executing the workflow. If settings are not exposed, disclose that they cannot be verified instead of inventing confirmation.
- Use the host's subagent API, not user-visible new tasks. With `collaboration.spawn_agent`, explicitly set `model: "gpt-5.6-luna"`, `reasoning_effort: "max"`, and `fork_turns: "none"`. Full-history forks cannot apply model overrides and defeat the context-saving design.
- Give each worker a self-contained brief: repo root, user objective, applicable constraints, owned scope, spec/record paths, relevant evidence pointers, expected output, and validation obligations. Workers must read applicable repository instructions. Do not forward the whole lead conversation.
- If exact models, effort settings, or subagent execution are unavailable, report BLOCKED with the required setup; do not silently substitute models or implement as the lead. Workers must not spawn additional workers; the lead controls the budget and ownership.

## Scope and persistence

User, repository, host, and safety instructions override this skill. Preserve unrelated changes. Never weaken the spec, tests, or required checks to obtain PASS. Existing user authorization persists; do not ask for it again. Do not commit, push, merge, deploy, publish, or perform destructive/external actions without authorization.

Default record: `specs/<task-slug>.md`, containing `SPEC`, `EVIDENCE`, `REVIEW`, and `LOOP`. Store machine-readable worker reports beside it under `specs/<task-slug>/`; use another repository-approved location when required. Records contain the baseline revision, relevant project-state and spec fingerprints, check plan, phase, cumulative repair count, per-finding attempts/outcomes, and result paths. Avoid unrelated generated artifacts.

On resume, compare spec, relevant working-tree state, and required-check plan against the record. Invalidate stale evidence and reviews. Repair limits remain cumulative across sessions. Strict mode uses separate spec/evidence/review/loop files and fingerprints covering the baseline, spec, relevant diff, and required-check plan; independent reviewer context is mandatory.

## 1. DISCOVER — Luna maps the code

The lead only locates the root, reads applicable top-level instructions, and establishes task scope before dispatch. Delegate primary repository exploration to one read-only Luna worker. Do not first load broad code trees, full manifests, logs, or large diffs into the lead context.

Ask Luna to locate implementation entry points, relevant symbols and callers, tests, manifests/CI check commands, existing changes, and uncertainty. Luna saves a valid JSON report and returns its path plus a short summary. The structure below is a starting point: omit irrelevant fields and add task-specific fields when useful.

```json
{
  "objective": "requested behavior",
  "baseline": {"revision": "...", "existing_changes": []},
  "instructions": [{"path": "AGENTS.md", "constraints": ["..."]}],
  "locations": [
    {"path": "src/example.ts", "symbol": "example", "lines": [10, 45],
     "role": "entry point", "relevance": "why inspect", "related": []}
  ],
  "behavior": {"current": "...", "desired": "..."},
  "checks": [{"command": "...", "cwd": ".", "source": "package.json:scripts.test", "coverage": "...", "required": true}],
  "risks": [],
  "unknowns": [{"question": "...", "next_search": "..."}],
  "suggested_scope": [],
  "searched": ["directories/symbols inspected"],
  "not_searched": ["relevant coverage gaps"]
}
```

Target a compact report, normally 2–4 KB; this is guidance, not a truncation rule for critical facts. Include pointers and findings, not copied files or raw logs. Separate observed facts from inference. A missing item is unknown, not proof of absence.

The lead reads the JSON, then inspects only the code slices and instruction files necessary to validate the design. Confirm important claims against actual code and check adjacent callers/contracts when warranted. Treat the report as a search map, not exhaustive truth. If a concrete gap appears, send a focused follow-up to Luna; do not repeat broad discovery or launch speculative completeness audits.

## 2. SPEC — Astra plans

Based on targeted code inspection, reuse a matching spec or save the objective, current/desired behavior, scope and non-goals, assumptions, stable requirement IDs, edge/failure behavior, acceptance criteria, verification commands, and done criteria before BUILD.

Make conservative non-material assumptions autonomously. Ask only for missing material decisions or authorization. Pause before BUILD if the user requested plan approval. Material changes to agreed behavior require approval and invalidate review.

Decompose into coherent tasks with IDs, dependencies, owned files/areas, required behavior, and acceptance checks. Choose one Luna per task, or batch tightly related small tasks into one worker. Parallelize only independent tasks with disjoint write ownership, bounded by available slots. Serialize edits to shared files and integration work. Do not invent task granularity merely to create more agents.

## 3. BUILD — Luna implements

Dispatch Luna with the saved spec and precise task brief. Require minimal coherent edits, preservation of user changes, meaningful tests when warranted, and affected checks. Workers must flag scope changes or conflicting edits instead of overriding them.

Worker reports are compact JSON with task ID, status, changed paths/symbols, requirement-to-change mapping, checks (command, exit status, result/log path), risks, and blockers. Large logs remain on disk. Report blocked or incomplete work honestly. The lead inspects relevant diff slices and results, updates records, and dispatches any integration edits to Luna.

During development run affected checks only. Do not duplicate a running command or repeat successful checks without new changes or unresolved concerns. Run the complete required gate once implementation is ready for review; delegate command execution and report collection to Luna when helpful.

## 4. REVIEW — fresh Luna, Astra acceptance

Use a fresh read-only Luna reviewer with `fork_turns: "none"`, the exact worker model settings, the spec, baseline/current-state pointers, and check evidence. Do not use the implementer's context as independent review. Reviewer inspects actual code/diff from disk, verifies evidence against current state, and marks each requirement and criterion PASS, FAIL, or BLOCKED.

Return compact JSON containing requirement statuses and findings with stable ID, severity, path/symbol/line, evidence, concrete remedy, and closing verification. Never output whole source files. The lead validates actionable findings with focused inspection, rejects unsupported claims with recorded reasons, and makes the final acceptance decision. Independent review is required for this workflow; if unavailable, report BLOCKED.

## 5. REPAIR and stopping

Assign confirmed findings and their direct consequences to Luna workers, preserving file ownership. The lead never fixes code itself. Add useful regression coverage; run affected checks during repair. Rerun the complete required gate whenever edits make its evidence stale, then obtain a fresh review of the final state. Close findings only with verification.

Keep at most three cumulative repair rounds; stop earlier for oscillation or a finding unresolved after two attempts without measurable progress. Also stop for missing material decisions, authorization, required infrastructure/checks, model/subagent capability, or conflicting requirements. Persist unresolved findings and an exact next action; never portray partial work as PASS.

The final response starts with `ASTRASPECLOOP PASS` only when every requirement, criterion, and required check passes on the current state, with complete evidence and no known regression, unresolved correctness/safety finding, blocker, or unapproved scope change. Otherwise start with `ASTRASPECLOOP BLOCKED`.

Keep the final report concise: outcome, record path, cumulative repair count, reviewer isolation, actual/verified model settings (or verification limitation), required-check results, and any exact next action. Follow the host's communication requirements; avoid narrating routine tool calls.
