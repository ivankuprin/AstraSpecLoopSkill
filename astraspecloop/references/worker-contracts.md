# Worker contracts

The lead reads this before first dispatch. Workers receive only the common contract and their role's requirements, not this entire file or the lead's conversation.

## Common

Provide repository root, task intent and relevant accepted clarifications, constraints, owned scope, spec/evidence pointers, expected checks, and a unique report path. Workers read applicable repository instructions, preserve existing changes, and cannot delegate or expand authorization.

Reports are valid JSON, normally 2–4 KB; retain critical evidence even when larger. Return the saved path and a short summary, or direct JSON when permissions prevent report writing. Use paths/symbols and observations, not copied code or logs. Extend the structure for task needs; distinguish facts, inference, and unknowns.

Every report includes task/role, status, scope, observed state, results, and blockers. State evidence identifies baseline revision plus a reproducible digest and covered paths for relevant tracked and untracked inputs; a revision alone is insufficient for a dirty tree. Exclude workflow reports from code digests. Record before/after state for edits and checks, including command, working directory, exit status, coverage, and log pointers. Concurrent changes make affected evidence provisional until integrated validation.

## Research

Project files are read-only; writing is allowed only to the assigned report. Locate entry points, symbols/callers, current behavior, instructions, existing changes, verification commands and their source, dependencies, risks, and unknowns. Include searched/not-searched coverage and actionable next searches. Stop once planning is supported; avoid speculative exhaustive searches.

Suggested fields: `objective`, `state`, `instructions`, `locations` (path, symbol, lines, relevance, related contracts), `checks`, `risks`, `unknowns`, `searched`, `not_searched`. Omit irrelevant optional fields.

## Builder / repairer

Edit only assigned areas. Flag conflicting changes or required scope expansion. Return requirement-to-change mapping, changed paths/symbols, state evidence, check outcomes, risks, and unresolved work. A follow-up brief supplies changed requirements, current state, findings, and closing verification; reuse prior context only after checking it remains applicable.

## Reviewer

Use a fresh context, never the implementer's. Project files and shared records are read-only; only the assigned report may be written. Compare the original request and accepted clarifications with the specification to detect omissions, then inspect actual code/diff, adjacent contracts, and evidence on disk. Do not trust implementer summaries as proof or invent new requirements.

Mark each requirement/criterion PASS, FAIL, or BLOCKED. Return stable finding IDs, severity, evidence with code pointers, remedy, closing verification, and reviewed-state evidence. Assess closure independently. Flag stale checks, integration gaps, and unjustified spec exclusions. Do not duplicate valid checks; request missing verification through the lead.
