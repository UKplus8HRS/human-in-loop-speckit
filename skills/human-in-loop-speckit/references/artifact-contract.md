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

## constitution.md

Required sections:

- `# Constitution`
- `## Principles`
- `## Governance`
- `## Versioning`
- `## Compliance Review`
- `## Sync Impact Report`

Principles should be testable and use MUST/SHOULD intentionally. If this is a native Spec Kit project, use `.specify/memory/constitution.md` instead of a separate local file.

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

## clarifications.md

Required sections:

- `# Clarifications`
- `## Coverage Map`
- `## Questions Asked`
- `## Integrated Answers`
- `## Deferred Questions`

Ask no more than 5 high-impact questions per pass. Each answer must be integrated back into `spec.md`, not only stored here.

## plan.md

Required sections:

- `# Implementation Plan`
- `## Technical Context`
- `## Constitution Check`
- `## Existing Reuse`
- `## Proposed Approach`
- `## Files and Boundaries`
- `## Risks and Mitigations`
- `## Conflicts or Conventions`
- `## Decisions Needing Approval`

Before drafting `plan.md`, read the relevant exports, callers, shared utilities, scripts, and tests.

## research.md

Required sections:

- `# Research`
- `## Unknowns Resolved`
- `## Decisions`
- `## Alternatives Considered`
- `## Open Technical Risks`

Each decision should state `Decision`, `Rationale`, and `Alternatives considered`.

## data-model.md

Required sections:

- `# Data Model`
- `## Entities`
- `## Fields`
- `## Relationships`
- `## Lifecycle and State`
- `## Validation Rules`

Mark `Not applicable` with rationale when the feature has no durable data model.

## contracts/

Use this folder for public or internal interfaces that affect implementation or tests:

- API endpoints
- CLI commands
- event schemas
- parser grammars
- UI interaction contracts
- external integration payloads

If there are no contracts, record that in `plan.md` or `research.md` with rationale.

## quickstart.md

Required sections:

- `# Quickstart`
- `## Prerequisites`
- `## Setup`
- `## Scenarios`
- `## Expected Outcomes`
- `## Troubleshooting`

This is a validation/run guide, not an implementation guide.

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

## checklists/requirements.md

Required sections:

- `# Requirements Checklist`
- `## Completeness`
- `## Clarity`
- `## Consistency`
- `## Measurability`
- `## Edge Cases`
- `## Non-Functional Requirements`

Checklist items test the quality of the requirements, not whether the implementation works.

## analysis.md

Required sections:

- `# Specification Analysis Report`
- `## Findings`
- `## Coverage Summary`
- `## Constitution Alignment`
- `## Unmapped Tasks`
- `## Metrics`
- `## Recommended Next Actions`

This report is read-only. Do not modify artifacts while generating it.

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

## convergence.md

Required sections:

- `# Convergence`
- `## Intent Inventory`
- `## Codebase Assessment`
- `## Remaining Work`
- `## Appended Tasks`
- `## Converged Items`
- `## Follow-up Recommendation`

Convergence happens after implementation. It compares current code to approved spec, plan, tasks, and verification expectations.
