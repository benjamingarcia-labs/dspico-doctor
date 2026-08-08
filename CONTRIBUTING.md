# Contributing to DSpico Doctor

Thank you for your interest in contributing to DSpico Doctor.

DSpico Doctor is a learning-first, open-source software-development project focused on researching, developing, testing, documenting, and preparing a useful beginner appropriate contribution to the DSpico community.

This repository is an independent project. Work developed here should not be presented as an official DSpico feature unless it has been accepted by the appropriate upstream maintainers.

## Before Contributing

Before opening an issue or beginning work:

1. Read the project `README.md`.
2. Review the documentation in `docs/`.
3. Search existing issues and pull requests for related work.
4. Confirm that the proposed change fits the current project phase and scope.
5. Open an issue before starting a substantial code, firmware, architecture, or workflow change.

Small corrections to spelling, formatting, or broken documentation links may not require a separate issue.

## Contribution Priorities

Contributions should support one or more of the following:

- documented DSpico user needs
- repository or architecture research
- build and flashing documentation
- requirements and acceptance criteria
- focused firmware or software improvements
- testing and validation
- troubleshooting guidance
- user documentation
- maintainability and compatibility

The project favors one small, complete, well-tested contribution over several incomplete or speculative changes.

## Scope Expectations

Keep contributions narrow and reviewable.

A pull request should normally address one issue or one coherent purpose. Do not combine unrelated documentation, refactoring, feature, and formatting changes in the same pull request.

Ideas that are useful but outside the current project scope should be recorded as future work rather than added silently.

## Development Workflow

Use the following workflow unless a maintainer requests otherwise:

1. Synchronize your local `main` branch.
2. Create a focused branch.
3. Make one logical change at a time.
4. Review the changed files.
5. Build and test when applicable.
6. Update relevant documentation.
7. Commit only the intended files.
8. Push the branch.
9. Open a pull request.
10. Respond to review feedback.

Example branch names:

```text
docs/add-build-instructions
feat/add-display-timeout
fix/reject-invalid-setting
test/add-settings-validation
```

## Commit Messages

Use concise commit messages that describe the purpose of the change.

Examples:

```text
docs: add firmware build instructions
feat: add configurable display timeout
fix: reject invalid settings value
test: add settings validation coverage
refactor: isolate button input handling
```

Avoid vague messages such as:

```text
update files
changes
fix stuff
work in progress
```

## Code Expectations

Code changes should:

* follow the existing repository conventions
* use readable control flow
* use descriptive names
* keep functions and modules focused
* avoid unnecessary dependencies
* handle invalid input defensively
* preserve compatibility where required
* include comments only when they explain intent, constraints, or non-obvious reasoning
* avoid unrelated refactoring
* remove temporary debug code before submission

Do not introduce a new framework, dependency, abstraction, or architecture pattern without explaining why it is necessary.

## Documentation Expectations

Documentation is part of the contribution, not an optional follow-up.

Follow the [Documentation Lifecycle](docs/documentation-lifecycle.md) when reviewing, updating, superseding, archiving, or removing durable documentation. Reviews are event-driven; a review does not require a modification when the document remains accurate.

Update documentation whenever a change affects:

* setup
* building
* flashing
* configuration
* behavior
* architecture
* testing
* troubleshooting
* limitations
* maintenance

Instructions should include enough detail for another person to repeat and verify the process.

## Testing and Validation

Testing should be tied to the requirement or behavior being changed.

Depending on the contribution, testing may include:

* build or compilation testing
* unit testing
* functional testing
* regression testing
* invalid-input testing
* compatibility testing
* hardware testing
* flashing verification
* recovery testing
* user acceptance testing

A successful build does not prove that a feature works. A successful feature test does not prove that existing behavior remains unaffected.

Record:

* the environment or hardware used
* the steps performed
* the expected result
* the actual result
* whether the test passed or failed
* any limitation or unverified behavior

Do not claim that code builds, tests pass, hardware works, or a feature is supported unless it has been verified.

## Evidence and Validation Boundaries

Clearly distinguish among:

* source-code findings
* command output
* repository state
* build results
* runtime behavior
* hardware observations
* inference
* assumptions
* unverified behavior

Static source inspection should not be presented as runtime proof.

Examples:

```text
Source inspection shows that the function sends the command.
```

```text
Runtime timing has not yet been measured.
```

## Pull Requests

A pull request should include:

* a clear summary
* the problem being addressed
* a linked issue when applicable
* the files or behavior changed
* testing performed
* documentation updated
* known limitations
* anything that remains unverified

Before submitting, review the complete diff and confirm that no unrelated files, credentials, editor settings, build artifacts, or personal files are included.

## Reporting Bugs

A useful bug report should include:

* a clear title
* the observed behavior
* the expected behavior
* steps to reproduce the problem
* hardware or software environment
* firmware or repository version
* exact error messages
* relevant logs or screenshots
* whether the problem is repeatable
* any attempted recovery steps

Do not include passwords, access tokens, private keys, or other sensitive information.

## Proposing Features

A feature proposal should explain:

* the user problem
* who experiences the problem
* the expected benefit
* evidence that the need exists
* the smallest useful behavior
* compatibility concerns
* testing considerations
* likely documentation requirements
* known risks or maintenance costs

Feature proposals may be declined or deferred when they are too broad, difficult to test, incompatible with the project, unsupported by user evidence, or likely to create excessive maintenance.

## Licensing and Attribution

Contributions must be compatible with the repository license.

Do not copy code, documentation, images, or other material unless its license permits inclusion and any required attribution is preserved.

Identify outside sources when a contribution is based on existing work.

## Respecting Upstream Projects

Before proposing work related to an existing DSpico repository:

* inspect its license
* inspect its contribution guidelines
* search existing issues and pull requests
* avoid duplicating active maintainer work
* preserve attribution
* follow the maintainer’s established conventions
* communicate respectfully
* do not present unmerged work as an official feature

## Questions

Open an issue when a contribution requires clarification about project scope, requirements, architecture, testing, or expected behavior.

For substantial work, resolve material uncertainty before beginning implementation.
