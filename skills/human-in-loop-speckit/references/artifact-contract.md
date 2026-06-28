# Artifact Contract

Use this reference when drafting or reviewing human-in-the-loop Speckit artifacts.

## Front Matter

Each artifact should start with:

```yaml
---
status: draft
owner: human
last_reviewed: null
---
```

Allowed status values:

- `draft`: Codex may edit; not execution authority.
- `needs-user-review`: ready for user review.
- `approved`: can be used by later artifacts or implementation.
- `executing`: implementation is in progress.
- `verified`: required evidence has been collected.

## goal.md

Required sections:

- `# Goal`
- `## One-line Objective`
- `## Why This Matters`
- `## Success Criteria`
- `## Assumptions`
- `## Open Questions`

## spec.md

Required sections:

- `# Specification`
- `## Users or Actors`
- `## User Stories`
- `## Functional Requirements`
- `## Non-Goals`
- `## Constraints`
- `## Acceptance Criteria`
- `## Open Questions`

## plan.md

Required sections:

- `# Implementation Plan`
- `## Existing Reuse`
- `## Proposed Approach`
- `## Files and Boundaries`
- `## Risks and Mitigations`
- `## Conflicts or Conventions`
- `## Decisions Needing Approval`

Before drafting `plan.md`, read the relevant exports, callers, shared utilities, scripts, and tests.

## tasks.md

Required format:

```markdown
- [ ] T001 [P?] [artifact/story?] Action with target file or command
```

Required sections:

- `# Tasks`
- `## Dependency Order`
- `## Parallel Candidates`
- `## Task List`
- `## Checkpoints`

Use `[P]` only when tasks touch different files or have no unresolved dependency.

## verification.md

Required sections:

- `# Verification`
- `## Required Automated Checks`
- `## Required Manual Checks`
- `## Evidence to Report`
- `## Cannot Claim Done If`
- `## Known Gaps`

Every check should explain why it matters, not only what command to run.

## handoff.md

Required sections:

- `# Handoff`
- `## Approved Inputs`
- `## Execution Boundaries`
- `## Agent Roles`
- `## Parallel Workstreams`
- `## Merge or Integration Order`
- `## Stop Conditions`

Use this file only after `tasks.md` and `verification.md` are approved.
