# DSpico Doctor Project Charter

## Purpose

DSpico Doctor exists to develop a small, useful, safe, and well-documented contribution to the DSpico community through a disciplined open-source software-development process.

The project will identify a real user problem, evaluate possible contributions, select a narrow and achievable first release, and develop it through requirements, design, implementation, testing, documentation, and community review.

## Initial Problem Evidence

During an initial DSpico setup, a beginner user experienced three significant sources of confusion:

1. The available instructions did not clearly distinguish between accessing the DSpico through the bootloader and accessing the card file system directly. Although the procedures were described, the user did not know what each mode should look like or how to confirm which mode was active.

2. The instructions did not make the complete required card structure sufficiently clear. The user copied `wrfu.srl`, `biosnds7.rom`, and `biosndsi7.rom` to the card without realizing that the required directory structure and bootloader also needed to be installed.

3. The available setup instructions appeared to assume a Windows-based computer. A user working on macOS had to interpret or translate Windows-specific instructions without clear platform-specific guidance.

These issues prolonged the setup process and suggest that beginner users may need clearer explanations, expected-state examples, file-structure diagrams, platform-specific guidance, or validation assistance.

## Initial Problem Hypothesis

Beginner DSpico users may have difficulty completing setup because the existing instructions do not always make the device modes, required files, directory structure, bootloader requirements, and operating-system differences sufficiently clear.

This hypothesis must be validated through documentation review, repository inspection, community discussions, existing issues, and maintainer feedback before the first contribution is selected.

## Initial Target User

The initial target user is a generally computer-capable person who is new to DSpico and may be using either macOS or Windows.

This user is comfortable copying files and following written instructions but has limited experience with bootloaders, firmware, directory structures, device modes, flashing workflows, or troubleshooting hardware-related software setup.

The user may struggle when instructions:

- assume Windows-specific menus, drive behavior, utilities, or file paths
- do not explain the macOS equivalent
- do not identify when a step is platform-specific
- do not clearly distinguish between bootloader access and direct card-file-system access
- do not show the complete required file and directory structure
- do not explain how to verify that the card has been prepared correctly

## Initial Scope

The initial project scope is to investigate and validate beginner setup problems related to:

- distinguishing DSpico bootloader access from direct card-file-system access
- understanding the expected appearance and behavior of each access mode
- identifying the complete required bootloader, file, and directory structure
- explaining platform-specific differences between macOS and Windows
- determining how a beginner can verify that a DSpico card has been prepared correctly
- reviewing existing documentation, repository files, issues, pull requests, and community discussions for evidence of similar problems
- evaluating small contribution options such as improved documentation, setup checklists, file-structure diagrams, or read-only validation assistance

The first contribution will be selected only after the problem has been validated and the existing DSpico project has been inspected.

## Initial Non-Goals

The project will not initially:

- modify or redesign the DSpico firmware
- add multiple unrelated features
- create an automatic firmware flasher
- write to or reformat the user's card
- distribute copyrighted BIOS, firmware, or game files
- claim full macOS support before tool and workflow compatibility are verified
- replace the existing DSpico documentation without maintainer agreement
- support every flash cartridge or retro-gaming device
- build a graphical application before a simpler contribution has been validated
- present assumptions or unmerged work as official DSpico capability

## Constraints

The project must operate within the following constraints:

- Benjamin is rebuilding his programming skills and has limited recent experience with modern software-development workflows.
- The first contribution must be small enough to understand, test, document, and maintain responsibly.
- Work must remain compatible with the existing DSpico hardware, firmware, repository conventions, and maintainer expectations.
- The project must not distribute copyrighted BIOS, firmware, game files, or other restricted content.
- Hardware access and testing may depend on the available DSpico cartridge, compatible systems, cables, storage media, and host computer.
- Some current instructions, tools, or workflows may assume Windows and may not have verified macOS equivalents.
- Firmware flashing, storage modification, or reformatting must not be introduced until safe recovery procedures are understood and documented.
- The project should use a small, practical toolset and avoid unnecessary frameworks, dependencies, or project-management systems.
- Claims about supported behavior must be based on verified evidence rather than assumptions or AI-generated output.
- Public project documentation must remain separate from Benjamin's private Learning Log.

## Initial Risks

### Selecting the Wrong Problem

The project may focus on a difficulty that affected one user but is not common or valuable enough to justify a community contribution.

Mitigation:
Validate the problem through documentation review, repository inspection, issues, pull requests, community discussions, and maintainer feedback.

### Duplicating Existing Work

The project may propose documentation or tooling that already exists or overlaps with active maintainer work.

Mitigation:
Search the upstream repository, documentation, issue tracker, pull requests, and community resources before selecting the first contribution.

### Expanding Scope Too Early

The project may grow from a narrow setup problem into firmware changes, graphical tools, flashing support, or multiple unrelated features.

Mitigation:
Keep the first release narrow, maintain a future backlog, and require each active task to support the current milestone.

### Misunderstanding the Existing System

Incomplete knowledge of the DSpico hardware, bootloader, firmware, file structure, or setup process could lead to incorrect requirements or unsafe recommendations.

Mitigation:
Document assumptions, inspect primary sources, reproduce workflows carefully, and verify findings before implementation.

### Hardware or Data Damage

Flashing, reformatting, or writing to the cartridge could corrupt files, prevent normal operation, or temporarily disable the device.

Mitigation:
Begin with documentation and read-only inspection. Require backups, recovery instructions, and controlled tests before any write operation is considered.

### Platform Compatibility Errors

A process that works on Windows may behave differently or fail on macOS, or vice versa.

Mitigation:
Identify platform-specific steps, test each supported workflow separately, and avoid claiming support that has not been verified.

### Overreliance on AI

AI-generated code, commands, or explanations may appear reasonable while containing incorrect assumptions or unsafe behavior.

Mitigation:
Review every change, understand the intent, inspect the diff, test the result, and avoid accepting code Benjamin cannot explain.

### Weak Testing Evidence

A successful build or one successful setup attempt may be mistaken for proof that the contribution works reliably.

Mitigation:
Define acceptance criteria, record expected and actual results, test failure conditions, and check for regressions.

### Maintainer Rejection

Even a technically correct contribution may not align with the upstream project's priorities, conventions, or maintenance capacity.

Mitigation:
Keep the contribution narrow, communicate early, follow contribution guidelines, and remain willing to revise or reduce scope.

### Documentation Drift

Instructions may become inaccurate as code, firmware, tools, or workflows change.

Mitigation:
Update documentation alongside implementation and include documentation review in the definition of done.

## Phase 1 Success Criteria

Phase 1 will be complete when:

- the public GitHub repository is established and accessible
- the project vision and charter are documented
- the initial beginner-user problem is clearly described
- confirmed observations are separated from unverified assumptions
- the initial target user is defined
- the project scope and non-goals are documented
- major constraints and risks are recorded
- the upstream DSpico project, license, contribution guidance, documentation, issues, and pull requests have been identified for review
- the initial research questions are documented
- the development environment, Git workflow, and documentation process are established
- public project documentation remains separate from the private Learning Log
- no implementation work has begun before the problem and contribution opportunity are validated

## Open Research Questions

### Existing Documentation

- What official DSpico setup documentation currently exists?
- Does the documentation clearly distinguish bootloader access from direct card-file-system access?
- Does it show the complete required file and directory structure?
- Does it include expected-state examples, screenshots, or troubleshooting guidance?
- Which instructions are platform-specific?

### macOS and Windows Compatibility

- Which DSpico setup and flashing workflows are supported on Windows?
- Which workflows are supported on macOS?
- Are different tools, drivers, commands, or file-management steps required?
- Are there macOS-specific risks involving hidden files, drive formatting, mounting, or safe ejection?
- Have macOS users reported similar setup difficulties?

### Community Need

- Have other users reported confusion about bootloader mode, file-system access, required files, directory structure, or platform assumptions?
- How frequently do these problems appear?
- Are maintainers already working on related documentation or tooling?
- Which beginner problems appear most important to users and maintainers?

### Technical System

- What role does the bootloader perform in the DSpico setup?
- What is the expected final card file and directory structure?
- Which files are mandatory, optional, user-supplied, or legally restricted?
- How can the device state and card preparation be verified safely?
- Can validation be performed without modifying the cartridge?

### Contribution Path

- What license governs the upstream project?
- What contribution guidelines and repository conventions must be followed?
- Does the maintainer prefer documentation changes, scripts, utilities, or issue proposals?
- What is the smallest contribution that would provide verified beginner value?
- How can the contribution be tested repeatably on both macOS and Windows?
