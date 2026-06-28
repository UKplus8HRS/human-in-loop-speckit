---
name: human-in-loop-speckit
description: Use when the user wants collaborative Spec Kit style planning, harness engineering, interviewed requirements, Markdown specs/plans/tasks/verification, or human review gates before Codex or subagents implement work.
---

# Human-in-loop Speckit

## Overview

Use this skill to turn a rough user idea into approved Markdown artifacts before implementation. Treat Codex as an interviewer and editor first, implementer second.

Core rule: unapproved Markdown is a draft. Do not use drafts as execution authority until the user confirms them.

## Workflow

1. State assumptions, ambiguities, and success criteria.
2. Interview the user for the next artifact only.
3. Expand the user's rough answer into a concise Markdown draft.
4. Show the material change and ask for confirmation or edits.
5. After approval, write or update the artifact.
6. Checkpoint: summarize what changed, what was verified, and what remains.
7. Move to the next artifact.
8. Dispatch implementation only after `tasks.md` and `verification.md` are approved.

Ask at most three focused questions at a time. If a reasonable default is low-risk, propose it as an assumption instead of blocking.

## Artifact Set

Prefer this minimum set:

| Artifact | Purpose | Approval needed before |
| --- | --- | --- |
| `goal.md` | What the task is, why it matters, and success criteria | Writing plans |
| `spec.md` | User stories, scope, non-goals, constraints, acceptance criteria | Writing implementation plan |
| `plan.md` | Technical approach, reuse targets, risks, file boundaries | Generating tasks |
| `tasks.md` | Ordered work items, dependencies, parallel candidates, checkpoints | Implementation |
| `verification.md` | Required tests, evidence, manual checks, cannot-claim-done rules | Implementation |
| `handoff.md` | What subagents or future Codex turns need to execute safely | Dispatch |

If the repository is already a Spec Kit project, align with `specs/<feature>/spec.md`, `plan.md`, and `tasks.md`. Otherwise create a task folder such as `work/harness/<short-slug>/` unless the user specifies another location.

For artifact details, read `references/artifact-contract.md` when drafting or reviewing files.

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

## Dispatch Rules

Only dispatch Codex work, Superpowers, or subagents after:

- `goal.md` says what success means.
- `spec.md` has scope and non-goals.
- `plan.md` identifies reuse and file boundaries.
- `tasks.md` has dependency order and parallel markers.
- `verification.md` lists evidence required before completion.

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
