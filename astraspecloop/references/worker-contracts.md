# Worker contracts

The lead sends the common contract and only the worker's role instructions.

## Common

Each brief includes working directory, intent, constraints, scope, report path, completion criterion, and any available verified pointers/state. Missing root, code locations, or check commands are research inputs for Luna to discover, never a reason for lead code inspection. Read applicable instructions; preserve user changes; do not delegate or expand authorization.

Use a short Markdown report with five headings: **Result** (task/role/status/outcome), **Facts** (scope, state, requirement mapping), **Checks** (commands/cwd, exit codes, coverage or not-run reasons), **Risks** (findings, uncertainty, blockers), **Decision needed** (next action or none). Normally 2–4 KB, not a minimum. No source, diffs, raw logs, or repeated task history sent to the lead.

Follow-ups report only changed findings, new checks, current state, and remaining work; link previous evidence. Fresh review still covers the entire task, even with delta reporting. Never edit shared state.json.

Luna computes fingerprints: baseline plus relevant tracked/untracked inputs and covered paths, not HEAD alone. Exclude workflow records. Check evidence identifies tested state, command, environment/dependency identity, exit status, and evidence pointer. Record missing observations honestly. Avoid speculative measurements; command duration is not full agent duration.

Before accepting a report, the lead checks required headings/IDs, observed versus inferred claims, state coverage, and check outcomes for completeness and contradictions. Request a focused correction when needed. Markdown and manually authored fingerprints are evidence summaries, not tamper-proof execution receipts; link available runner/CI receipts without requiring a new service.

## Research / builder

Research writes only its report. Locate the handler, callers, relevant tests, constraints, current behavior, and gaps sufficient for the assigned objective. Stop at the criterion; further searches need a named question.

Reuse confirmed locations, runtime, and commands unless changed or contradicted. Derive local equivalents from substantive test coverage and relevant environment conditions, not command names alone. Never retry billing-blocked CI or treat it as a code failure.

Builders edit assigned areas, report conflicts or scope expansion, and provide requirement-to-change/check mapping. Complete required verification on stable combined state. Reuse valid results; a new review phase alone does not justify repeating tests.

## Reviewer / repairer

Start fresh. Verify spec completeness against the original request, then inspect ALL task changes relative to the recorded starting state and their relevant interactions. Supplied navigation aids do not replace independent code assessment or prove prior verdicts.

Review concrete risks selected for the task (e.g. duplicate submission, atomic debit, timeout recovery). Other evidenced defects still count. Each finding has stable ID, severity, trigger/preconditions, consequence, code pointers plus reasoning/reproduction, remedy, and closing check. Unsupported suspicions remain questions pending focused verification.

Mark requirements PASS/FAIL/BLOCKED. Initially write only the report. After assignment, repair confirmed findings and run checks, then stop. A NEW reviewer assesses the whole updated task. Before its final verdict, it confirms relevant fingerprints still match reviewed/tested state; include this confirmation in the same report, without another agent. Changes require affected revalidation; known changes after that report invalidate acceptance. Keep writers stopped through lead acceptance. Clean review states scoped readiness and uncertainty, not guaranteed production safety.
