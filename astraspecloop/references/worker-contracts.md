# Worker contracts

The lead reads this before first dispatch. Workers receive only the common contract and their role's requirements, not this entire file or the lead's conversation.

## Common

Provide repository root, task intent and relevant accepted clarifications, constraints, owned scope, spec/evidence pointers, expected checks, and a unique report path. Workers read applicable repository instructions, preserve existing changes, and cannot delegate or expand authorization.

All research, build, review, and repair reports use compact Markdown in an assigned `.md` file, normally 2–4 KB. Never send code, diffs, raw logs, or repeated task instructions to Astra. Keep detailed evidence in worker-owned files; return concise conclusions and pointers for other Luna workers. If more evidence is needed, provide a focused follow-up. Return the saved path and a short summary, or direct Markdown if report writing is unavailable. Workers do not write the shared cycle `state.json`.

Use these fixed headings, with short bullets or a small table where useful:
- **Result:** task ID, role, status, and one-sentence outcome.
- **Facts:** scope, observed behavior or changes, requirement mapping and relevant code pointers; label inference explicitly.
- **Checks:** state evidence, commands/cwd, exit statuses and coverage, or explicitly not run with reason.
- **Risks:** findings with stable IDs, uncertainty, blockers, and unsearched areas; state none when applicable.
- **Decision needed:** the concrete question or recommended next action for Astra, or none.

Preserve critical facts when the size target is insufficient, without embedding source or raw logs.

Every report includes task/role, status, scope, observed state, results, and blockers. State evidence identifies baseline revision plus a reproducible digest and covered paths for relevant tracked and untracked inputs; a revision alone is insufficient for a dirty tree. Exclude workflow reports from code digests. Record before/after state for edits and checks, including command, working directory, exit status, coverage, and log pointers. Concurrent changes make affected evidence provisional until integrated validation.

Luna performs all project searches, reads, commands, code changes, and digest computation. Astra must be able to decide using reports alone. Reuse confirmed runtime/check information unless contradicted by new evidence. Do not invent token counts or report tool execution time as full agent elapsed time.

## Research

Project files are read-only; writing is allowed only to the assigned report. Locate entry points, symbols/callers, current behavior, instructions, existing changes, verification commands and their source, dependencies, risks, and unknowns. Include searched/not-searched coverage and actionable next searches. Stop once planning is supported; avoid speculative exhaustive searches.

Within the common headings, include relevant instructions, locations (path, symbol, lines, related contracts), searched coverage, and remaining gaps. Do not add a parallel JSON version of the report.

## Builder

Edit only assigned areas. Flag conflicting changes or required scope expansion. Return requirement-to-change mapping, changed paths/symbols, state evidence, check outcomes, risks, and unresolved work. Builder follow-ups finish assigned implementation; review-discovered repairs belong to the reviewer that found the issue.

## Reviewer
 
For concurrent writers, apply the worktree contract below before dispatch. Ordinary sequential work stays in the existing checkout.

Start with a fresh context, never the implementer's. Inspect ALL active changes and adjacent contracts, the original request, spec completeness, and evidence on disk. Follow the lead's task-specific risk focus without inventing requirements. Initially project files are read-only; only the report may be written. After Astra assigns confirmed findings back to this same reviewer, it becomes the repairer and may modify the assigned code/tests and run checks. Preserve unrelated user changes; flag repairs outside authorization.

Mark each requirement/criterion PASS, FAIL, or BLOCKED. Return stable finding IDs, severity, evidence with code pointers, remedy, closing verification, and reviewed-state evidence. Assess closure independently. Flag stale checks, integration gaps, and unjustified spec exclusions. Do not duplicate valid checks; request missing verification through the lead.

After fixing, return a repair report and stop. A NEW reviewer must assess all active changes; the repairing reviewer cannot certify its own fixes as the final independent verdict. Final clean review explicitly states no actionable findings and readiness within the agreed scope, with check evidence and residual uncertainty; it is not a guarantee of zero production defects.

## Parallel-write worktrees

Use separate Git worktrees only when tasks justify concurrent project edits, including concurrent repairs. Luna inspects repository state and prepares one unique worktree and branch per writer (default branch prefix `codex/`), with a common verified starting state. Preserve dirty and untracked user changes; do not silently start from HEAD when required input exists only in the working tree. If reproducing the intended baseline is unsafe, Git is unavailable, or worktree setup is unavailable, use sequential execution.

Each brief names the assigned absolute worktree path, baseline, owned files, shared interfaces, and unique report path accessible to Astra. Workers write only in their assigned checkout and report location; never switch another worker's branch. Keep one lead-owned authoritative cycle state. Worktrees are separate checkouts, not security sandboxes: shared databases, ports, generated outputs outside the checkout, and services still require coordination or serialized commands.

After writers finish, assign a Luna to integrate their changes into the designated result checkout, preserve unrelated changes, resolve conflicts, and run the complete required gate on the combined state. Integration can use scoped patches; do not create commits or merges without authorization. Independent review must cover all active integrated changes, not isolated worker branches. Repairs target that integrated state; repeat isolation only if further concurrent writes are justified. Report result checkout and fingerprints to Astra without source or diffs.

Do not remove worktrees with unintegrated or uncommitted work. Retain them and report their paths unless cleanup is authorized and Luna verifies all intended changes are preserved.
