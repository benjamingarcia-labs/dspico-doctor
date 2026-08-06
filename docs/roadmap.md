# DSpico Doctor Roadmap

## Purpose

This roadmap defines the durable progression DSpico Doctor should follow from project establishment and foundational research through deliverable selection, implementation, validation, documentation, and release preparation.

It establishes phase purposes, entry criteria, expected durable outputs, exit criteria, dependencies, and stop conditions.

This document is not a project-status dashboard.

Active, paused, blocked, deferred, resumed, and completed work is tracked through GitHub Issues. Branches, pull requests, temporary priorities, current tasks, and short-term sequencing must not be copied into this roadmap.

A phase is complete only when durable repository evidence satisfies its exit criteria. The roadmap itself does not prove completion.

## Roadmap Principles

DSpico Doctor should progress according to the following principles:

- solve a verified user or project problem;
- keep the first owned deliverable narrow and maintainable;
- preserve compatibility with the existing DSpico ecosystem;
- distinguish DSpico Doctor work from official upstream behavior;
- prefer safe, reversible, and read-only approaches when possible;
- define requirements and acceptance criteria before implementation;
- treat builds, source inspection, runtime tests, and hardware tests as different forms of evidence;
- document limitations and unresolved behavior honestly;
- avoid unnecessary frameworks, dependencies, and project structure;
- create durable documents only when real work justifies them.

The project vision and guiding principles remain defined in the [Project Vision](vision.md). The original project authorization, user problem, scope, non-goals, risks, and initial research questions remain defined in the [Project Charter](project-charter.md).

## Work-State Authority

GitHub Issues are authoritative for changing project state, including:

- active work;
- priorities;
- sequencing;
- pauses and resumptions;
- blockers;
- deferred work;
- current dependencies;
- completion evidence;
- superseded work;
- final dispositions.

This roadmap defines the stable development path and phase gates. It must not duplicate changing issue state.

---

## Phase 1 — Establish the Project and Validate the Initial Problem

### Purpose

Establish the project’s identity, intended users, initial problem hypothesis, scope, safety boundaries, and development discipline.

This phase ensures that the project begins from a documented problem and defined constraints rather than from an assumed technical solution.

### Entry Criteria

- A credible DSpico user problem or project opportunity has been observed.
- There is sufficient interest to investigate whether the problem warrants a maintained open-source contribution.
- The project can be separated from official upstream repositories and private learning records.

### Expected Durable Outputs

- project repository;
- root project overview;
- product vision;
- project charter;
- initial user-problem evidence;
- initial target-user definition;
- scope and non-goals;
- constraints and major risks;
- initial research questions;
- contribution and review practices;
- clear separation between public project records and private apprenticeship records.

### Exit Criteria

Phase 1 may be considered complete when:

- the project purpose and intended user value are documented;
- the initial user problem is described without presenting the hypothesis as universal fact;
- scope, non-goals, constraints, and major risks are explicit;
- the project’s independent relationship to upstream DSpico is clear;
- public project documentation and private learning records are separated;
- implementation has not begun before the problem and contribution opportunity are investigated;
- repository evidence is sufficient to begin structured upstream and technical research.

### Important Dependencies

- access to the DSpico ecosystem and relevant documentation;
- a repository structure capable of preserving durable project evidence;
- contribution practices that require narrow, reviewable changes.

### Major Risks and Stop Conditions

Stop or revise the project direction when:

- the user problem cannot be supported beyond one unexamined assumption;
- the project would require distributing copyrighted or restricted files;
- the proposed scope is too broad to understand, test, document, or maintain;
- the project cannot distinguish its work from official upstream functionality;
- safe investigation is not possible with the available hardware, software, or recovery knowledge.

### Work-State Ownership

Changing work associated with this phase must be tracked through GitHub Issues.

---

## Phase 2 — Understand the Ecosystem and Contribution Environment

### Purpose

Develop enough technical, repository, legal, workflow, and community understanding to evaluate contribution opportunities responsibly.

This phase reduces the risk of duplicating existing work, misunderstanding component ownership, overstating evidence, or selecting a deliverable that cannot be safely validated.

### Entry Criteria

- Phase 1 exit criteria are supported by durable repository evidence.
- The relevant upstream repositories and their ownership boundaries can be identified.
- The project has defined research questions that can be investigated through primary repository evidence and controlled observation.

### Expected Durable Outputs

As justified by the investigation:

- upstream repository and component mapping;
- source-path and architecture research;
- setup and workflow findings;
- platform-compatibility research;
- contribution-guidance and license findings;
- issue, pull-request, release, and maintainer-evidence reviews;
- records of community-reported behavior;
- evidence classifications;
- source revisions and inspection dates for time-sensitive findings;
- documented assumptions, inferences, limitations, and unresolved questions;
- candidate contribution paths.

Research records should distinguish among:

- official upstream evidence;
- community reports;
- direct user observations;
- source inspection;
- command output;
- build evidence;
- runtime or hardware evidence;
- inference;
- assumptions;
- unverified behavior.

### Exit Criteria

Phase 2 may be considered complete enough to enter deliverable selection when:

- the relevant DSpico component repositories and authority boundaries are understood;
- applicable licenses, contribution guidance, issues, pull requests, and active upstream work have been inspected;
- major user-problem claims can be traced to identified evidence;
- time-sensitive findings include an inspection boundary where appropriate;
- technical dependencies and unresolved questions are visible;
- candidate deliverables can be compared using current evidence;
- duplication risk, maintenance burden, safety, testing feasibility, and current capability can be assessed;
- no selected deliverable depends primarily on unsupported AI assumptions.

Phase 2 does not require every DSpico technical question to be resolved. It requires enough reliable evidence to make a deliberate, bounded deliverable decision.

### Important Dependencies

- access to current upstream repositories and records;
- accurate identification of which upstream component owns each behavior;
- available host platforms, hardware, or community evidence where required;
- source traceability sufficient to reproduce major conclusions;
- revalidation of paused or time-sensitive research before it is relied upon.

### Major Risks and Stop Conditions

Stop deliverable selection when:

- current upstream state has not been inspected;
- the apparent opportunity duplicates active or completed upstream work;
- evidence cannot distinguish reported behavior from independently verified behavior;
- platform, firmware, storage, or hardware claims exceed available evidence;
- validation would require unsafe writes, flashing, or reformatting without understood recovery procedures;
- research records are too incomplete or stale to support a responsible choice.

### Work-State Ownership

Research tasks, pauses, open questions, evidence updates, and candidate investigations must be tracked through GitHub Issues.

---

## Phase 3 — Select and Specify the First DSpico Doctor-Owned Deliverable

### Purpose

Select one narrow deliverable that DSpico Doctor itself will own, maintain, validate, and document.

This phase converts research into an approved commitment without beginning implementation prematurely.

### Entry Criteria

- Phase 2 has produced enough current evidence to compare realistic candidates.
- Applicable upstream work has been inspected for duplication and compatibility.
- Candidate scope, user value, maintenance burden, legal constraints, safety, and testing feasibility can be evaluated.
- The project can distinguish an upstream contribution from a DSpico Doctor-owned deliverable.

### Expected Durable Outputs

- a formal deliverable-selection decision record;
- documented alternatives considered;
- selected and deferred options;
- decision consequences and maintenance ownership;
- reconsideration conditions;
- requirements for one approved deliverable;
- explicit scope and non-goals;
- user and intended outcomes;
- functional and non-functional requirements;
- supported and unsupported environments;
- safety, legal, compatibility, storage, firmware, and hardware boundaries;
- acceptance criteria;
- required test categories;
- documentation requirements;
- known limitations and unresolved questions;
- traceability to research evidence and related Issues.

The following document categories become justified only when the selected deliverable requires them:

- `docs/decisions/`;
- `docs/requirements/`;
- risk records;
- testing strategy;
- recovery requirements;
- design constraints.

Directories should be created only when the first real artifact is ready to be committed.

### Exit Criteria

Phase 3 is complete when:

- one deliverable has been deliberately selected and approved;
- the decision record explains the evidence and tradeoffs;
- the deliverable addresses a supported user or project need;
- duplication with active upstream work is avoided or justified;
- the scope is narrow enough to understand, test, document, and maintain;
- legal, safety, platform, compatibility, and recovery concerns are identified;
- requirements are testable, inspectable, or explicitly classified as constraints;
- acceptance criteria define how implementation success will be evaluated;
- supported and unsupported environments are explicit;
- implementation can begin without relying on unresolved foundational assumptions.

### Important Dependencies

- current research evidence;
- a completed deliverable-selection decision;
- requirements approved before implementation;
- access to the environments needed for planned validation;
- a clear maintenance owner.

### Major Risks and Stop Conditions

Do not approve a deliverable when:

- the user need remains unsupported;
- upstream state has not been re-inspected;
- safe validation is not feasible;
- the work depends on distributing restricted files;
- the deliverable cannot be maintained responsibly;
- requirements cannot be made testable;
- implementation would begin before scope, acceptance criteria, and safety boundaries are understood;
- the decision relies primarily on AI-generated recommendations rather than reviewed evidence.

### Work-State Ownership

Candidate comparison, approval, rejection, deferral, dependencies, and requirements work must be tracked through GitHub Issues.

---

## Phase 4 — Design, Implement, and Validate the First Deliverable

### Purpose

Develop the approved deliverable and produce evidence showing whether it satisfies its requirements and acceptance criteria.

This phase must remain tied to the selected deliverable rather than expanding into unrelated project features.

### Entry Criteria

- Phase 3 exit criteria are satisfied.
- The selected deliverable has approved requirements and acceptance criteria.
- Required environments, dependencies, tools, hardware, and recovery boundaries are understood.
- Verification, recovery, and stop conditions are defined before consequential changes begin.

### Expected Durable Outputs

As required by the approved deliverable:

- focused design documentation;
- implementation;
- source code or maintained documentation;
- configuration;
- dependency records;
- test plans;
- automated or manual tests;
- build instructions;
- expected and actual test results;
- invalid-input and failure-condition evidence;
- compatibility evidence;
- hardware or firmware evidence where applicable;
- risk updates;
- recovery or rollback procedures;
- implementation documentation;
- issue and pull-request history connecting changes to requirements.

The following categories become justified only when actual work requires them:

- `docs/design/`;
- `docs/testing/`;
- `docs/build/`;
- `docs/flashing/`;
- `docs/recovery/`;
- `docs/risks/`.

### Exit Criteria

Phase 4 is complete when:

- the approved requirements are implemented or receive a documented disposition;
- acceptance criteria map to recorded validation evidence;
- relevant tests have been performed in the intended environment;
- expected and actual results are recorded;
- failure and invalid-input behavior have been considered;
- regressions have been evaluated proportionately;
- safety and recovery procedures have been tested where applicable;
- unsupported environments and limitations are explicit;
- the diff contains only intended changes;
- affected documentation is updated;
- root and affected-directory README impact has been reviewed;
- the deliverable can be explained and maintained by the project owner.

A successful build proves compilation only. It does not, by itself, satisfy this phase gate.

One successful runtime or hardware test does not prove broad compatibility.

### Important Dependencies

- approved requirements;
- design decisions proportionate to the work;
- controlled implementation branches;
- appropriate test environments;
- required hardware and software versions;
- backups and recovery procedures for consequential operations;
- upstream compatibility and attribution where applicable.

### Major Risks and Stop Conditions

Stop implementation or validation when:

- the work expands beyond the approved requirements without a reviewed scope decision;
- required recovery procedures are absent;
- tests cannot distinguish expected behavior from accidental success;
- hardware, firmware, or storage operations introduce unacceptable risk;
- dependencies or environments cannot be reproduced;
- restricted files would be committed or distributed;
- implementation cannot be understood or maintained;
- generated code or documentation has not been reviewed and validated;
- evidence does not support claimed compatibility or correctness.

### Work-State Ownership

Implementation tasks, test failures, blocks, scope changes, defects, and validation evidence must be tracked through GitHub Issues and pull requests.

---

## Phase 5 — Prepare User Documentation and Release Evidence

### Purpose

Prepare the validated deliverable for responsible use, maintenance, and release.

This phase ensures that users can understand what the deliverable does, use it within supported boundaries, recognize limitations, and recover safely when applicable.

### Entry Criteria

- Phase 4 exit criteria are satisfied.
- The deliverable has recorded validation evidence.
- Known limitations and supported environments are understood.
- The project has sufficient confidence to prepare user-facing guidance without overstating capability.

### Expected Durable Outputs

As applicable:

- user documentation;
- setup instructions;
- usage instructions;
- platform-specific guidance;
- troubleshooting documentation;
- recovery instructions;
- supported and unsupported environment statements;
- known limitations;
- release notes;
- installation or distribution guidance;
- license and attribution records;
- release artifacts;
- checksums or artifact-verification guidance;
- maintenance ownership;
- post-release issue routes.

The following categories become justified only when real user-facing content exists:

- `docs/user/`;
- `docs/troubleshooting/`;
- `docs/recovery/`;
- release records and artifacts.

### Exit Criteria

Phase 5 is complete when:

- a target user can identify the deliverable’s purpose and supported use;
- setup and usage instructions are repeatable within documented environments;
- failure conditions and troubleshooting routes are documented;
- recovery procedures are available where the deliverable can affect hardware, firmware, or storage;
- limitations and unverified behavior are visible;
- release evidence matches the published claims;
- legal, licensing, and attribution requirements are satisfied;
- release artifacts correspond to reviewed source and documented versions;
- maintenance responsibilities and issue-reporting routes are clear;
- repository documentation accurately reflects the released behavior.

### Important Dependencies

- completed validation evidence;
- stable release scope;
- verified installation and usage procedures;
- platform-specific testing where support is claimed;
- recovery guidance where consequential behavior exists;
- maintainership capacity.

### Major Risks and Stop Conditions

Do not release when:

- claims exceed recorded evidence;
- user instructions cannot be reproduced;
- release artifacts cannot be tied to reviewed source;
- recovery behavior is required but undocumented or untested;
- known limitations create unacceptable user risk;
- restricted or improperly licensed content is included;
- supported platforms or hardware have not been validated;
- maintenance ownership is unclear;
- unresolved defects materially undermine the intended use.

### Work-State Ownership

Release preparation, blockers, release candidates, defects, support limitations, and post-release work must be tracked through GitHub Issues, pull requests, and release records.

---

## Phase Transitions

Movement between phases is evidence-based rather than calendar-based.

A later phase may begin only when:

- the prior phase’s relevant exit criteria are supported;
- unresolved items do not invalidate the next phase;
- dependencies and stop conditions have been reviewed;
- the transition is represented by appropriate durable records and GitHub Issues.

Some research may continue after Phase 2, and documentation may be updated throughout the project. The phase model defines the primary decision gates; it does not prohibit necessary feedback loops.

When evidence invalidates an earlier assumption, the project should return to the applicable phase, update durable records, and revise downstream work as needed.

## Documentation Creation Rule

Canonical documentation paths identify where records belong when they are justified. They are not a checklist of directories that must exist.

Create a directory or document only when:

- a real artifact is required;
- its responsibility is clear;
- it will preserve durable evidence or guidance;
- it does not duplicate another source of truth.

Do not create empty directories or placeholder documents solely to represent future phases.

## Roadmap Maintenance

Update this roadmap only when project evidence materially changes the durable phase structure, phase gates, expected outputs, dependencies, or stop conditions.

Do not update the roadmap for routine discoveries, active tasks, issue sequencing, temporary priorities, branches, pull requests, pauses, blockers, or normal progress within an existing phase. Record those changes in the applicable Issues, pull requests, research records, decisions, requirements, risks, testing records, or other owning documentation.

When a roadmap update is necessary:

- document the reason, evidence, scope, and validation in the related Issue or pull request;
- modify only the sections directly affected by the change;
- preserve unrelated valid content;
- review the root README and any affected documentation;
- avoid adding historical commentary, change logs, or duplicated work-state information to the roadmap.

## Validation Boundary

This roadmap defines the intended DSpico Doctor progression and the evidence required to pass each phase gate.

It does not establish that any phase is complete.

Phase completion, active work, pauses, blocks, implementation behavior, test results, hardware behavior, release readiness, and current project status must be verified through the applicable repository records and GitHub Issues.
