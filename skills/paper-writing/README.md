# Paper Writing

Chinese guide: [README.zh-CN.md](README.zh-CN.md)

`paper-writing` is an AI4Math Skill adapter for source-grounded mathematical
paper writing. It helps a coding agent draft, revise, and audit paper text from
provided notes, proofs, experiment logs, references, and reviewer feedback.
It also supports early-stage paper skeletons and final submission readiness
checks without splitting unsupported-claim review into a separate public Skill.

This package intentionally exposes one public writing workflow. Unsupported
claim checks are part of the writing review step; there is no separate public
unsupported-claim-review Skill package.

## When To Use It

Use this Skill when:

- drafting or revising abstracts, introductions, related work, theorem
  exposition, experiment narratives, conclusions, or response letters;
- turning mathematical notes, paper-reading outputs, proof artifacts, or
  experiment logs into paper text;
- building a rapid paper skeleton, contribution spine, section map, or result
  dependency map before drafting prose;
- checking a draft for unsupported claims, missing citations, overstated
  contribution language, or unclear assumptions.

Do not use this Skill to invent citations, theorems, experiments, numeric
results, proof status, or author claims that are not supported by the supplied
materials.

## What It Produces

The agent should produce the requested paper text or paper skeleton, plus
claim-evidence notes, proof-obligation notes, unsupported-claim warnings,
citation-needed markers, and recommended next source checks.

## Skill Entry Points

| File | Purpose |
| --- | --- |
| `SKILL.md` | top-level paper-writing workflow |
| `skills/registry.yaml` | nested subskill routing |
| `skills/paper-skeleton-and-logical-architecture/SKILL.md` | rapid paper skeleton, section map, and result dependency audit |
| `skills/claim-evidence-ledger/SKILL.md` | claim-to-source evidence audit |
| `skills/proof-obligation-and-assumption-audit/SKILL.md` | theorem assumptions, proof obligations, and external result fit |
| `skills/notation-and-variable-consistency/SKILL.md` | notation, symbol, and variable audit |
| `skills/formula-environment-and-readability/SKILL.md` | displayed formula and environment readability |
| `skills/latex-build-and-layout-audit/SKILL.md` | LaTeX build, reference, citation, and layout audit |
| `templates/` | reusable source packet, skeleton, ledger, and submission readiness templates |
| `README.md` | English package guide |
| `README.zh-CN.md` | Chinese package guide |

## Quick Start

```text
Use this repository's paper-writing workflow.

Read:
- SKILL.md
- templates/source-packet.md if the source packet is not already organized
- skills/registry.yaml if the task needs skeleton, proof, claim, notation, formula, or LaTeX checks

Goal:
Draft the introduction for this mathematical paper.

Target:
<notes, theorem statements, related papers, experiment logs, or draft file>

Constraints:
- inspect the source packet first;
- do not invent citations or results;
- mark unsupported claims explicitly;
- return the draft plus claim-evidence notes.
```

## How To Interact

The normal interface is conversation with a coding agent:

```text
human goal
  -> agent reads SKILL.md
  -> agent loads a focused subskill if needed
  -> agent inventories sources
  -> agent builds a skeleton if the paper structure is not stable
  -> agent proposes a writing plan
  -> human replies approve / revise / reject / skip
  -> agent drafts or revises
  -> agent reports evidence gaps and next checks
```

Use these checkpoint words:

| Decision | Meaning |
| --- | --- |
| `approve` | run the proposed next step |
| `revise` | update the plan before running |
| `reject` | stop this path |
| `skip` | skip this phase |
| `stop` | end the session and summarize state |

## Maintainer Checks

Run the root validation suite from the organizer repository:

```bash
python3 -m unittest discover -s tests -v
python3 scripts/validate_skill_repo.py
```
