# AstraSpecLoopSkill

Current version: **1.4.2**.

A bounded DISCOVER → SPEC → BUILD → REVIEW → REPAIR workflow for Codex. **Your selected main model acts exclusively as team lead; Luna xHigh performs all project code work.**

## Roles

- **Team lead (your selected main model and reasoning effort):** reads compact worker reports, plans, divides tasks, dispatches workers, and decides from reported evidence. It does not search, read, or write project code, tests, diffs, configuration, or raw logs.
- **Luna xHigh (`gpt-5.6-luna`, xhigh reasoning):** researches, reads and writes code, runs checks, computes fingerprints, reviews changes, and repairs findings.

The lead may read required host/skill instructions and maintain its own plans and workflow records. Code pointers in reports are for subsequent Luna tasks, not for the lead to open.

Select any available main model and reasoning effort for the calling task. The skill preserves that choice, including when the main model is Luna. Every new worker explicitly requests Luna xHigh with `fork_turns: "none"`. Missing worker capabilities produce BLOCKED rather than silent substitution.

## Workflow

1. **DISCOVER:** Luna explores the repository and reports behavior, locations, constraints, risks, and verification options.
2. **SPEC:** the lead plans exclusively from reports. Missing information triggers a focused Luna follow-up. A request for a plan only stops at the plan.
3. **BUILD:** Luna workers implement coherent subtasks and run relevant checks. The lead can batch small tasks or coordinate independent tasks with compatible interfaces and disjoint write ownership.
4. **REVIEW:** a fresh Luna reviews all changes produced for the task and relevant interactions, without auditing unrelated user changes. Findings need a trigger, consequence, evidence, and closing verification.
5. **REPAIR:** the reviewer that found the problem fixes it after assignment. A new Luna reviews the entire updated task result; the repairer cannot independently accept its own fixes.

There are at most three cumulative repair rounds, with earlier stopping for oscillation or repeated attempts without progress. A successful third round still passes. Writers stop during final checks and review; stale evidence must be revalidated.

Every assignment has a completion criterion. Two consecutive attempts at the same objective without evidence-based progress block in any phase. Interrupted attempts retain their IDs. Requested approval checkpoints and decisions on material scope changes are respected.

## Local verification

Green GitHub CI is not required by default. Luna runs equivalent substantive checks locally; billing outages or missing hosted runs alone do not block implementation PASS. Unavailable hosted checks remain not run, never falsely passed. Essential uncovered behavior still blocks; explicit hosted-CI requests and branch protection remain separate constraints.

Existing check results can be reused when relevant code, dependencies, environment, and check plan are confirmed unchanged. The final reviewer confirms that reviewed/tested fingerprints still match before its verdict. Known subsequent changes invalidate acceptance. Writers remain stopped through lead acceptance.

## Reports and state
 
When concurrent edits are worthwhile, Luna prepares a separate Git worktree and branch for each writer. Sequential work and read-only review do not require extra worktrees. Luna preserves the intended baseline, including relevant uncommitted changes; unsafe or unavailable isolation falls back to sequential work. Worktrees do not isolate shared databases or services.

After parallel work, a Luna integrates changes into the designated result checkout and runs the combined required gate. Fresh review covers the integrated changes. Commits, merges, and cleanup retain their authorization requirements; unintegrated work is preserved.

Luna reports use compact Markdown, normally 2–4 KB, with five fixed sections: **Result, Facts, Checks, Risks, Decision needed**. Reports contain conclusions and evidence pointers, never code excerpts, diffs, or raw logs.

The plan lives in `specs/<task-slug>.md`; worker reports and the authoritative `state.json` live in `specs/<task-slug>/`, unless repository rules require another location. JSON is only for cycle state: phase, counters, findings, fingerprints, results, and report pointers. The lead updates it from Luna reports. Existing history and counters survive resume.

Follow-ups report only new results and remaining questions, linking unchanged evidence. Unknown locations and commands are discovered by Luna, not the lead. Fingerprints are bundled with substantive worker tasks rather than separate bookkeeping agents.

Worker contracts, repair/resume rules, and conditional worktree instructions are separate references. Structural validation passed; this version has not been behaviorally benchmarked. No measured token-saving percentage or guarantee of defect-free production behavior is claimed.

## Install in Codex

Use the `astraspecloop` folder as the skill path:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo ivankuprin/AstraSpecLoopSkill \
  --path astraspecloop
```

Restart Codex after installation so the skill is discovered.

## Use

Choose your preferred main model and reasoning effort, then invoke:

```text
Use $astraspecloop to implement this feature through verified review and repair.
```

For a plan without implementation:

```text
Use $astraspecloop to research and write an implementation plan for this feature.
```

## Verdicts

Implementation runs finish with:

- `ASTRASPECLOOP PASS`: all requirements, criteria, and required checks pass on the current state, with a clean fresh review and no unresolved blocker.
- `ASTRASPECLOOP BLOCKED`: decisions, capabilities, authorization, checks, or unresolved findings prevent verified completion.

Plan-only requests deliver the plan without claiming implementation PASS. Commits, pushes, merges, deployment, publishing, and destructive/external actions require authorization.
