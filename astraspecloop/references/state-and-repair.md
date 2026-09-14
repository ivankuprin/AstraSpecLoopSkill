# State and repair

Read before repair, resume, or strict mode.

Keep one lead-owned state.json: phase, starting state, pre-existing/task change scope, fingerprints, check plan/results, report pointers, assignment attempts, per-finding remedies/outcomes, repair count, active round, and pending work. Plans/reports remain Markdown; do not duplicate prose.

An attempt is work against an objective with an observed outcome, not each tool call or message. Record it before dispatch and its evidence afterward. Two consecutive attempts without progress block in any phase; renaming objectives cannot reset the history. An interrupted attempt resumes under its original ID. The main file defines progress.

One repair round starts with confirmed findings, includes their fixes/checks, and ends with fresh review. Initial build/review is round zero. Increment once before dispatch, preserve the round ID across interruptions, and allow at most three rounds. PASS after round three is valid. Stop earlier for oscillation or repeated no-progress attempts.

The finding reviewer owns repairs. If unavailable, a fresh Luna independently re-establishes findings before owning fixes. A different fresh reviewer accepts the combined task result.

Luna checks saved state on resume; the lead never reads code. Preserve counters and historical report pointers when migrating older records. Invalidate affected evidence when code, dependencies, environment, or check plan changes; reused evidence needs an explicit equivalence confirmation. Exclude reports/state from code digests; keep spec/check-plan fingerprints separately.

Collect fingerprints during research/build/review already needed for the task. A bookkeeping-only change does not invalidate code checks. Keep changed spec/check-plan fingerprints pending until verified during substantive work; do not invent them or accept stale evidence. Plan-only delivery needs no extra implementation-verification agent.

For older green-CI requirements, map substantive checks to local equivalents and preserve valid results. Billing alone is not a defect or repair round. Never remove explicit hosted-execution requirements or unmet essential coverage silently.

Strict mode stores spec, evidence, review, and loop state separately with the same gates and limits. Record pending actions before handoff. Missing check results never pass.
