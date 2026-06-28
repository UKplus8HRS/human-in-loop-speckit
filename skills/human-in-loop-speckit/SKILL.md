---
name: human-in-loop-speckit
description: Use when the user wants collaborative Spec Kit style planning, harness engineering, interviewed requirements, Markdown specs/plans/tasks/verification, or human review gates before Codex or subagents implement work.
---

# Human-in-loop Speckit

## Overview

Use this skill to turn a rough user idea into approved Spec Kit-style Markdown artifacts before implementation. Treat Codex as an interviewer and editor first, implementer second.

Core rule: unapproved Markdown is a draft. Do not use drafts as execution authority until the user confirms them.

## Workflow

1. Choose workflow depth with the user: `basic`, `full`, or `existing-spec-kit`.
2. State assumptions, ambiguities, and success criteria.
3. Interview the user for the next artifact only.
4. Expand the user's rough answer into a concise Markdown draft.
5. Show the material change and ask for confirmation or edits.
6. After approval, write or update the artifact.
7. Checkpoint: summarize what changed, what was verified, and what remains.
8. Move to the next artifact or quality gate.
9. Dispatch implementation only after tasks and verification gates are approved.

Ask at most three focused questions at a time. If a reasonable default is low-risk, propose it as an assumption instead of blocking.

## Artifact Set

Use `basic` only for small, low-risk work. For normal feature development, prefer `full`.

### Basic mode

| Artifact | Purpose | Approval needed before |
| --- | --- | --- |
| `goal.md` | What the task is, why it matters, and success criteria | Writing plans |
| `spec.md` | User stories, scope, non-goals, constraints, acceptance criteria | Writing implementation plan |
| `plan.md` | Technical approach, reuse targets, risks, file boundaries | Generating tasks |
| `tasks.md` | Ordered work items, dependencies, parallel candidates, checkpoints | Implementation |
| `verification.md` | Required tests, evidence, manual checks, cannot-claim-done rules | Implementation |
| `handoff.md` | What subagents or future Codex turns need to execute safely | Dispatch |

### Full mode

| Artifact | Purpose | Approval needed before |
| --- | --- | --- |
| `constitution.md` | Project principles, non-negotiable rules, governance, versioning | Specs and plans |
| `goal.md` | Human-readable objective and success criteria | Specs |
| `spec.md` | User stories, functional requirements, non-goals, constraints, acceptance criteria | Clarification and planning |
| `clarifications.md` | Up to 5 high-impact decisions asked one at a time and integrated back into spec | Planning |
| `plan.md` | Technical context, architecture, file boundaries, constitution check | Research/design |
| `research.md` | Decisions, rationales, alternatives considered for unknowns and dependencies | Design artifacts |
| `data-model.md` | Entities, fields, relationships, lifecycle, validation rules | Contracts and tasks |
| `contracts/` | API, CLI, event, UI, parser, or integration contracts where applicable | Tasks |
| `quickstart.md` | End-to-end validation scenarios and expected outcomes | Tasks and verification |
| `tasks.md` | Story-ordered executable tasks with IDs, file paths, dependencies, parallel markers | Analyze/implementation |
| `checklists/requirements.md` | Requirements quality checklist, like unit tests for English | Analyze/implementation |
| `analysis.md` | Read-only cross-artifact consistency report before implementation | Implementation |
| `verification.md` | Automated checks, manual checks, evidence to report, cannot-claim-done rules | Completion claims |
| `handoff.md` | Agent roles, workstreams, boundaries, stop conditions, integration order | Dispatch |
| `convergence.md` | Post-implementation gap assessment and remaining work list | Follow-up implementation |

If the repository is already a Spec Kit project, align with `.specify/memory/constitution.md`, `specs/<feature>/spec.md`, `plan.md`, `tasks.md`, `research.md`, `data-model.md`, `contracts/`, and `quickstart.md`. Otherwise create a task folder such as `work/harness/<short-slug>/` unless the user specifies another location.

For artifact details, read `references/artifact-contract.md` when drafting or reviewing files.
For parity guidance against upstream Spec Kit, read `references/speckit-parity.md` before full-mode work.

## Interview Pattern

For each artifact:

```text
I am drafting <artifact>. Current assumption: <one sentence>.
Questions:
1. <highest-impact missing decision>
2. <boundary or constraint>
3. <verification or evidence need>
```

Then convert the user's natural language into structured Markdown. Preserve the user's intent; do not silently add speculative features.

## Spec Kit Integration

Use `specify` when it helps bootstrap a standard Spec Kit project or inspect Spec Kit behavior. Do not force `specify init` into an existing repository without explicit user confirmation, because it may add `.specify/`, prompts, templates, or agent files.

Useful commands:

```powershell
specify --version
specify init . --integration codex --integration-options="--skills"
specify init . --force --integration codex --integration-options="--skills"
```

Before running `specify init` in a non-empty directory, state what it will add and ask for confirmation.

If upstream `speckit-*` skills exist in the project and the user wants native Spec Kit behavior, use them directly rather than reimplementing them. Use this skill to add human review gates around those artifacts.

## Quality Gates

Full mode must run these gates before implementation:

1. **Constitution gate**: no plan or task may violate an approved MUST principle.
2. **Clarification gate**: ask up to 5 high-impact questions before planning when spec ambiguity would change architecture, data model, tasks, or tests.
3. **Design gate**: unknown technical decisions must be resolved in `research.md`; data and interface obligations must appear in `data-model.md`, `contracts/`, or be explicitly marked not applicable.
4. **Task gate**: every task must use `- [ ] T### [P?] [Story?] Description with file path or command`.
5. **Analysis gate**: before implementation, perform a read-only consistency check across spec, plan, tasks, constitution, and verification.
6. **Verification gate**: before claiming done, provide the evidence required by `verification.md`.
7. **Convergence gate**: after implementation, compare current code to approved intent and append or report remaining work.

## Dispatch Rules

Only dispatch Codex work, Superpowers, or subagents after:

- approved goal/spec/plan artifacts say what success means and what is out of scope;
- full mode has either completed or explicitly skipped constitution, clarification, research, data model, contracts, quickstart, checklist, and analysis gates;
- `tasks.md` has dependency order, task IDs, file paths, and parallel markers;
- `verification.md` lists evidence required before completion;
- `handoff.md` names agent roles, boundaries, stop conditions, and integration order.

When dispatching, keep model judgment limited to judgment tasks such as summarizing, classifying, extracting, reviewing, and implementing from approved specs. Deterministic behavior such as retry policy, routing, status-code handling, and test commands must live in code, scripts, or explicit task instructions.

## Checkpoints

After every step, report:

```text
Done: <what changed>
Verified: <what was checked>
Remaining: <next artifact or decision>
Blocked: <only if blocked, with exact missing input>
```

Do not say implementation is complete unless all required verification evidence exists or the missing checks are explicitly disclosed.

## Common Mistakes

| Mistake | Correction |
| --- | --- |
| Generating all documents from one vague prompt | Interview one artifact at a time |
| Treating AI-written drafts as approved | Ask for confirmation before execution |
| Letting the same agent implement and judge completion | Separate implementation, review, and verification responsibilities |
| Adding speculative features | Put them in `Open Questions` or `Out of Scope` |
| Skipping verification because the task "looks done" | Require evidence named in `verification.md` |
