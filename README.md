# AstraSpecLoopSkill

Current version: **1.1.0**.

The core skill is 692 words, down from 1,501 in v1.0.1. Worker contracts and resume/repair details live in references loaded when needed. This reduces entrypoint instructions, not a measured percentage of task token costs.

Version 1.1.0 reuses implementation workers for related repairs, gives independent reviewers the original request to check specification completeness, defines shared contracts before parallel work, and ties reports to project state. Final verification and review run after writers finish.

`AstraSpecLoop` is a bounded `DISCOVER → SPEC → BUILD → REVIEW → REPAIR` workflow for Codex. Astra Low acts as the technical lead, while Luna Max handles repository research, implementation, repairs, and independent review.

The lead receives a compact JSON map from the research worker, studies only the relevant code, writes the specification, and decomposes the work. Luna workers then make the code and test changes. Astra evaluates the evidence, routes repairs, and accepts the final result only when the required checks pass.

## Roles

- **Astra Low (`gpt-6-astra`, low reasoning):** scope, targeted inspection, specification, task decomposition, dispatch, triage, and final acceptance.
- **Luna Max (`gpt-5.6-luna`, max reasoning):** discovery, implementation, tests, repairs, and fresh read-only review.

The skill does not silently change the model of the calling task. Select Astra Low for the main Codex task. The skill explicitly requests Luna Max for worker agents; if the host cannot provide those settings, the workflow reports `ASTRASPECLOOP BLOCKED` instead of substituting another model.

## Install in Codex

Use the `astraspecloop` folder as the skill path:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo ivankuprin/AstraSpecLoopSkill \
  --path astraspecloop
```

Restart Codex after installation so the skill is discovered.

## Use

Choose Astra Low in the task and invoke the skill explicitly:

```text
Use $astraspecloop to implement this feature through verified review and repair.
```

Or:

```text
Use $astraspecloop to fix this bug until every required check passes or progress is blocked.
```

## Workflow

1. **DISCOVER:** a read-only Luna worker searches the repository and writes a focused JSON report with relevant files, symbols, checks, risks, and unknowns.
2. **SPEC:** Astra validates the report against the code, records the specification, and splits the work into coherent subtasks.
3. **BUILD:** Luna workers implement the assigned subtasks and run affected checks.
4. **REVIEW:** a fresh Luna worker inspects the actual diff and evidence independently.
5. **REPAIR:** Astra triages findings and dispatches Luna workers for bounded repair rounds.

The workflow preserves a task record under `specs/<task-slug>.md`, keeps repair limits cumulative across resumed sessions, and avoids copying large source files or logs into the lead context.

The lead saves state after every phase and repair round. Discovery and review workers may write only their assigned report; with a fully read-only filesystem, they return JSON for the lead to save. One repair round includes the finding set, its fixes, verification, and the next independent review, regardless of worker count. Interrupted rounds resume without consuming another round, and a successful third round still returns PASS.

## Verdicts

The final response starts with exactly one of:

- `ASTRASPECLOOP PASS` — all requirements, acceptance criteria, and required checks pass on the current state.
- `ASTRASPECLOOP BLOCKED` — a material decision, capability, authorization, required check, or unresolved finding prevents verified completion.

The skill never commits, pushes, merges, deploys, publishes, or performs destructive actions without authorization.
