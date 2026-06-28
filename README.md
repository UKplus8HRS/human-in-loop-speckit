# Human-in-loop Speckit 🧭

Languages: **English** | [简体中文](README.zh-CN.md)

Human-in-loop Speckit is a Codex skill for collaborative, review-gated Spec Kit style planning.

It is for people who do not want an AI coding agent to turn one vague prompt into a full implementation plan and start editing files immediately. Instead, the skill makes Codex act as an interviewer and editor first: it asks focused questions, expands rough answers into Markdown artifacts, waits for human confirmation, and only then uses those approved artifacts as execution authority.

This project is inspired by GitHub Spec Kit and harness engineering patterns, but it is not a fork of Spec Kit and does not modify upstream Spec Kit. It now supports two workflow depths:

- **Basic mode**: smaller harness for quick, low-risk work.
- **Full mode**: closer to Spec Kit parity, including constitution, clarification, research, data model, contracts, quickstart, checklists, analysis, verification, handoff, and convergence.

## Why this exists ✨

AI coding agents are strongest when the goal, boundaries, and verification evidence are explicit. They are weakest when they have to infer all of those from a vague instruction.

Human-in-loop Speckit adds a collaboration layer around that problem:

- define the goal before implementation;
- write specs with the human, not just for the human;
- mark every unreviewed artifact as a draft;
- require approval before a draft can drive work;
- separate implementation, review, and verification responsibilities;
- make subagent or Superpowers dispatch happen after planning, not before it.

The core rule is simple:

```text
Unapproved Markdown is a draft.
Drafts are not execution authority.
```

## Workflow depth ⚙️

Use `basic` only when the task is small and the user explicitly wants speed.

Use `full` for normal feature work, multi-agent work, unclear requirements, or anything that needs strong verification. Full mode follows this shape:

```text
constitution -> goal -> spec -> clarify -> plan -> research
-> data-model/contracts/quickstart -> tasks -> checklist
-> analysis -> verification -> handoff -> implement -> converge
```

If the repo is already a native Spec Kit project, use the upstream `speckit-*` skills directly where appropriate and use this skill as the human approval layer around them.

## What it produces 📄

The skill guides Codex through either the basic set or the full Spec Kit-style set.

Basic mode:

| Artifact | Purpose |
| --- | --- |
| `goal.md` | What the task is, why it matters, and what success means |
| `spec.md` | Users, user stories, requirements, non-goals, constraints, and acceptance criteria |
| `plan.md` | Technical approach, reuse targets, file boundaries, risks, and decisions needing approval |
| `tasks.md` | Ordered work items, dependencies, parallel candidates, and checkpoints |
| `verification.md` | Automated checks, manual checks, evidence to report, and cannot-claim-done rules |
| `handoff.md` | Approved inputs and boundaries for Codex, Superpowers, or subagents |

Full mode adds:

| Artifact | Purpose |
| --- | --- |
| `constitution.md` | Principles, governance, and non-negotiable project rules |
| `clarifications.md` | Up to 5 high-impact questions integrated back into the spec |
| `research.md` | Decisions, rationales, and alternatives for unknowns and dependencies |
| `data-model.md` | Entities, fields, relationships, lifecycles, and validation rules |
| `contracts/` | API, CLI, event, parser, UI, or integration contracts |
| `quickstart.md` | End-to-end validation scenarios and expected outcomes |
| `checklists/requirements.md` | Requirements quality checklist |
| `analysis.md` | Read-only consistency and coverage report |
| `convergence.md` | Post-implementation gap assessment and remaining work |

If the repository is already a Spec Kit project, the skill aligns with `specs/<feature>/spec.md`, `plan.md`, and `tasks.md`. Otherwise it creates a lightweight task folder such as `work/harness/<short-slug>/`.

## How it differs from Spec Kit 🔍

Spec Kit is a full spec-driven development toolkit. It can bootstrap a project, create standard specs, plans, tasks, and agent integrations.

Human-in-loop Speckit is a human-controlled wrapper around the same lifecycle shape:

| Area | Spec Kit | Human-in-loop Speckit |
| --- | --- | --- |
| Primary mode | Structured commands and generated artifacts | Interview, draft, confirm, then write |
| Human role | Provide requirements and prompts | Co-edit every artifact before execution |
| Lifecycle depth | Constitution, clarify, plan, tasks, analyze, implement, converge | Same concepts with explicit human approval gates |
| Execution gate | Plan and tasks drive implementation | Approved tasks, verification, analysis, and handoff are required |
| Verification | Checklists and analyze workflows | Checklists, analysis, evidence, cannot-claim-done rules, convergence |
| Subagents | Can be used through agent tooling | Dispatch only after approved handoff |

Use Spec Kit when you want the native toolkit. Use this skill when you want the same level of structure but with the human explicitly co-editing and approving each gate.

## Installation 🚀

Clone this repository and copy the skill directory into your Codex skills folder:

```powershell
git clone https://github.com/UKplus8HRS/human-in-loop-speckit.git
Copy-Item -Recurse .\human-in-loop-speckit\skills\human-in-loop-speckit $env:USERPROFILE\.codex\skills\
```

Then start a new Codex session so the skill list refreshes.

The installed skill path should look like:

```text
%USERPROFILE%\.codex\skills\human-in-loop-speckit\SKILL.md
%USERPROFILE%\.codex\skills\human-in-loop-speckit\agents\openai.yaml
%USERPROFILE%\.codex\skills\human-in-loop-speckit\references\artifact-contract.md
```

## Optional: install GitHub Spec Kit 🛠️

This skill can work without the Spec Kit CLI. If you also want the upstream Spec Kit CLI, install `uv` and `specify-cli`:

```powershell
py -m pip install --user uv
& "$env:APPDATA\Python\Python314\Scripts\uv.exe" tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

Make sure the installed tool path is on `PATH`:

```powershell
$env:Path = "$env:USERPROFILE\.local\bin;$env:APPDATA\Python\Python314\Scripts;$env:Path"
specify --version
```

To initialize a Codex-oriented Spec Kit project:

```powershell
specify init --here --force --integration codex --integration-options="--skills" --ignore-agent-tools --script ps
```

Depending on the Spec Kit version and integration, generated skills may be placed under `.agents/skills`.

## Usage 💬

Invoke the skill in Codex:

```text
$human-in-loop-speckit Help me turn this rough idea into goal/spec/plan/tasks/verification. Ask me questions first and do not implement until I approve the artifacts.
```

For full mode:

```text
$human-in-loop-speckit Use full mode. Walk me through constitution, spec, clarification, plan, research, tasks, checklist, analysis, verification, and handoff before implementation.
```

Codex should then work in short loops:

```text
I am drafting goal.md.
Current assumption: this task should improve the planning workflow before implementation.

Questions:
1. What outcome should count as success?
2. What should be explicitly out of scope?
3. What evidence should Codex report before claiming the work is done?
```

After the human answers, Codex drafts the artifact, asks for edits or approval, and only writes it after confirmation.

## Example workflow 🧪

```text
Human: I want a harness for building a small app with Codex.

Codex with this skill:
1. Drafts goal.md from an interview.
2. Expands the goal into spec.md.
3. Reads the relevant codebase before drafting plan.md.
4. Breaks approved work into tasks.md.
5. Writes verification.md with evidence requirements.
6. Creates handoff.md for subagents only after approval.
7. Implements or dispatches work.
8. Reports checkpoints after each step.
```

Checkpoint format:

```text
Done: <what changed>
Verified: <what was checked>
Remaining: <next artifact or decision>
Blocked: <only if blocked, with exact missing input>
```

## Artifact status model ✅

Each generated artifact should start with:

```yaml
---
status: draft
owner: human
last_reviewed: null
---
```

Allowed status values:

- `draft`: Codex may edit; not execution authority.
- `needs-user-review`: ready for human review.
- `approved`: can be used by later artifacts or implementation.
- `executing`: implementation is in progress.
- `verified`: required evidence has been collected.

## Repository layout 🗂️

```text
.
|-- README.md
|-- LICENSE
|-- skills/
|   `-- human-in-loop-speckit/
|       |-- SKILL.md
|       |-- agents/
|       |   `-- openai.yaml
|       `-- references/
|           |-- artifact-contract.md
|           `-- speckit-parity.md
```

## Validation 🔎

If you have the Codex system `skill-creator` skill available locally, validate the skill folder:

```powershell
py "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" .\skills\human-in-loop-speckit
```

Expected result:

```text
Skill is valid!
```

## Design principles 🧩

- Ask before guessing when a decision changes scope or architecture.
- Keep changes surgical and tied to approved artifacts.
- Use deterministic code for routing, retries, status handling, and checks.
- Use model judgment for interviewing, summarizing, classifying, extracting, reviewing, and drafting.
- Make verification explain why each check matters.
- Surface conflicts instead of blending incompatible patterns.
- Keep the human in control of approval gates.

## Relationship to upstream projects 🤝

- GitHub Spec Kit: https://github.com/github/spec-kit
- Claude Code Superpowers: used as an optional downstream dispatch model, not bundled here.

This repository is independent and is not affiliated with GitHub, OpenAI, or the upstream Spec Kit project.

## License 📜

MIT. See `LICENSE`.
