# Documentation Lifecycle

## Purpose

This document defines the lightweight lifecycle used to keep DSpico Doctor documentation accurate without turning documentation maintenance into a calendar-driven process.

Documentation is reviewed when meaningful project evidence changes or when a document is about to be relied upon for consequential work. Age alone does not require an update.

## Document Classes

### Stable

Stable documents record durable project direction, structure, decisions, or constraints.

Examples include the project vision, charter, roadmap, approved decisions, and requirements.

Review a stable document when the governing decision, durable structure, scope, requirement, or authority boundary changes.

### Living

Living documents describe workflows, setup, behavior, dependencies, supported environments, maintenance procedures, or user-facing instructions.

Examples include setup guides, build instructions, flashing guidance, troubleshooting, and contributor guidance.

Review a living document when the behavior, dependency, environment, limitation, workflow, or supported procedure it describes changes.

### Time-bounded evidence

Time-bounded evidence records what was inspected or observed at a specific point in time.

Examples include research records, release inspections, issue or pull-request state, build results, hardware observations, and external-source reviews.

Preserve the original inspection boundary. Reverify affected evidence before it is used for a new consequential decision, implementation, compatibility claim, or user-facing update.

Do not silently rewrite historical evidence to look current.

### Operational state

Operational state includes active, paused, blocked, deferred, resumed, superseded, and completed work.

Store operational state in GitHub Issues, pull requests, branches, test records, and other owning lifecycle systems rather than static documentation.

Do not copy temporary task status into durable documents merely to make them appear current.

## Review Triggers

Review the affected documentation when any of the following occurs:

- documented behavior, setup, build, flashing, recovery, troubleshooting, or maintenance changes;
- a dependency, upstream component, release, supported environment, or compatibility boundary changes materially;
- a requirement, decision, architecture boundary, safety control, legal constraint, or project scope changes;
- new validation evidence contradicts, narrows, expands, or materially qualifies an existing claim;
- a time-sensitive source will be relied upon for consequential work;
- a contributor or user reports reproducible evidence that conflicts with current documentation;
- files or directories are added, removed, renamed, moved, or reorganized in a way that affects documented navigation or structure;
- a document is being prepared for supersession, archival, or removal.

Do not create recurring review schedules unless a specific document has a real time-based requirement.

## Review Versus Modification

A review does not automatically require an edit.

After review:

- modify the document when its content is no longer accurate, complete enough for its responsibility, or consistent with current supported evidence;
- make no change when the document remains accurate and its responsibility has not changed;
- record the no-change decision in the related Issue or pull request when the review was required by material work, a structural change, or an acceptance criterion.

Do not touch files merely to refresh dates or create cosmetic evidence of maintenance.

## README Impact

Whenever repository files or directories are added, removed, renamed, moved, reorganized, or materially changed:

1. review the root `README.md`;
2. review README files in affected directories;
3. update them only when structure, navigation, authority, setup, usage, dependencies, workflow, supported behavior, or limitations changed;
4. record a no-change decision when review found that modification was unnecessary.

README review is mandatory. README modification is conditional.

## Supersession, Archival, and Removal

Prefer updating a document in place when it still has the same responsibility.

Supersede a document when a new durable record replaces its responsibility and preserving the older record helps maintain history or traceability. Link the old and new records where practical.

Archive a document only when it remains useful as historical evidence but should no longer be treated as current guidance.

Remove a document when it is obsolete, misleading, duplicated without historical value, or no longer has a legitimate repository responsibility.

Before superseding, archiving, or removing documentation:

- identify references and links that depend on it;
- preserve evidence, attribution, and decision history that still matter;
- update affected navigation;
- record the rationale in the related Issue or pull request;
- review README impact.

## Evidence Reverification

When time-bounded evidence is reused for consequential work:

1. inspect the authoritative or identified source again;
2. preserve the prior inspection boundary when historically relevant;
3. record the new revision, item, version, or inspection date where appropriate;
4. distinguish changed evidence from changed interpretation;
5. update only the records and claims actually affected;
6. keep unresolved questions unresolved unless evidence resolves them.

Automated checks can confirm links, formatting, or other mechanical properties. They do not prove that time-sensitive research remains factually current.

## Work-State Routing

Use the owning GitHub Issue or pull request for changing work state, including:

- current task and priority;
- active branch or pull request;
- blockers and pauses;
- deferred work and review triggers;
- temporary risks and implementation notes;
- completion evidence and final disposition.

Use durable documentation for stable guidance, decisions, requirements, research evidence, validated procedures, and maintained user information.

## Maintenance Principle

Update documentation because the evidence or responsibility changed, not because time passed.

The goal is a small, accurate, traceable documentation system—not a second project-management system.