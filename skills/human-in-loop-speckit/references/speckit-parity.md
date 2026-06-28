# Spec Kit Parity Guide

Use this reference when the user expects Human-in-loop Speckit to be comparable to upstream Spec Kit rather than a lightweight harness.

## Upstream Concepts to Preserve

| Upstream Spec Kit concept | Human-in-loop equivalent |
| --- | --- |
| `speckit-constitution` | constitution interview and approval gate |
| `speckit-specify` | spec interview and draft approval |
| `speckit-clarify` | up to 5 high-impact questions integrated into spec |
| `speckit-plan` | plan plus research, data model, contracts, quickstart |
| `speckit-tasks` | story-ordered tasks with IDs, file paths, dependencies, parallel markers |
| `speckit-checklist` | requirements quality checklist |
| `speckit-analyze` | read-only cross-artifact consistency report |
| `speckit-implement` | implementation from approved tasks only |
| `speckit-converge` | post-implementation gap assessment and remaining tasks |
| `speckit-taskstoissues` | optional GitHub issue export from approved tasks |

## Full Mode Order

1. Constitution
2. Goal
3. Specification
4. Clarification
5. Plan
6. Research
7. Data model
8. Contracts
9. Quickstart
10. Tasks
11. Requirements checklist
12. Analysis
13. Verification
14. Handoff
15. Implementation
16. Convergence

Do not force every artifact when it is genuinely not applicable. If skipped, record why.

## Clarification Rules

- Ask one question at a time.
- Ask at most 5 questions per pass.
- Prefer questions that change architecture, data modeling, task breakdown, testing, UX behavior, operational readiness, or compliance.
- Integrate answers into the relevant artifact; do not leave answers only in the chat transcript.
- Keep deferred questions visible.

## Task Rules

Every task must follow:

```text
- [ ] T001 [P?] [Story?] Description with file path or command
```

Use `[P]` only when tasks can truly run in parallel without touching the same files or depending on unresolved work.

## Analysis Rules

Run analysis before implementation. It should report:

- duplicated or conflicting requirements
- ambiguous terms
- missing task coverage
- unmapped tasks
- constitution violations
- plan/spec/task inconsistencies
- missing verification evidence

Analysis is read-only. Fixes require a separate approved edit pass.

## Convergence Rules

After implementation, inspect current code against approved intent. If gaps remain, append new tasks rather than rewriting existing tasks. If everything matches, report converged without changing task files.
