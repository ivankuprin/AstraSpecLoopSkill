# AstraSpecLoopSkill

Current version: **1.2.1**.

A bounded DISCOVER → SPEC → BUILD → REVIEW → REPAIR workflow for Codex. **Astra Low is exclusively the team lead; Luna xHigh performs all project code work.**

## Roles

- **Astra Low (`gpt-6-astra`, low reasoning):** reads compact worker reports, plans, divides tasks, dispatches workers, and decides from reported evidence. It does not search, read, or write project code, tests, diffs, configuration, or raw logs.
- **Luna xHigh (`gpt-5.6-luna`, xhigh reasoning):** researches, reads and writes code, runs checks, computes fingerprints, reviews changes, and repairs findings.

Astra may read required host/skill instructions and maintain its own plans and workflow records. Code pointers in reports are for subsequent Luna tasks, not for Astra to open.

Select Astra Low for the calling task; the skill cannot switch it automatically. Every new worker explicitly requests Luna xHigh with `fork_turns: "none"`. Missing required capabilities produce BLOCKED rather than silent model substitution.

## Workflow

1. **DISCOVER:** Luna explores the repository and reports behavior, locations, constraints, risks, and verification options.
2. **SPEC:** Astra plans exclusively from reports. Missing information triggers a focused Luna follow-up. A request for a plan only stops at the plan.
3. **BUILD:** Luna workers implement coherent subtasks and run relevant checks. Astra can batch small tasks or coordinate independent tasks with compatible interfaces and disjoint write ownership.
4. **REVIEW:** a fresh Luna reviews all active changes against the original request and specification, focusing on relevant risks such as races, transactions, validation, and error handling.
5. **REPAIR:** the reviewer that found the problem fixes it after Astra assigns the confirmed findings. A new Luna with no inherited context then reviews all active changes. The repairing reviewer cannot provide final independent acceptance of its own edits.

There are at most three cumulative repair rounds, with earlier stopping for oscillation or repeated attempts without progress. A successful third round still passes. Writers stop during final checks and review; stale evidence must be revalidated.

## Reports and state

Luna reports use compact Markdown, normally 2–4 KB, with five fixed sections: **Result, Facts, Checks, Risks, Decision needed**. Reports contain conclusions and evidence pointers, never code excerpts, diffs, or raw logs.

The plan lives in `specs/<task-slug>.md`; worker reports and the authoritative `state.json` live in `specs/<task-slug>/`, unless repository rules require another location. JSON is only for cycle state: phase, counters, findings, fingerprints, results, and report pointers. Astra updates it from Luna reports after each phase and round. Existing history and counters survive resume.

Detailed worker contracts and repair/resume rules are separate references loaded when needed. No measured token-saving percentage or guarantee of defect-free production behavior is claimed.

## Install in Codex

Use the `astraspecloop` folder as the skill path:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo ivankuprin/AstraSpecLoopSkill \
  --path astraspecloop
```

Restart Codex after installation so the skill is discovered.

## Use

Choose Astra Low and invoke:

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
