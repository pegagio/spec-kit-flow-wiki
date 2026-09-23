# Tasks: Adopt Spec Kit Flow Wiki Identity

**Input**: Design documents from `specs/006-adopt-flow-wiki-identity/`

**Prerequisites**: [plan.md](./plan.md), [spec.md](./spec.md), [research.md](./research.md), [data-model.md](./data-model.md), [contracts/](./contracts/), [quickstart.md](./quickstart.md)

**Validation approach**: The specification requires clean and migrated consumer checks. This repository has no dedicated test framework, so the tasks use static contract review and disposable Specify consumers instead of introducing one.

**Organization**: Setup and foundation protect existing state. Each user story then delivers a separately testable slice. The published repository rename and release checks are final external gates.

## Phase 1: Setup

Establish current state before changing source or installed consumer artifacts.

- [X] T001 Capture the current branch, complete Git scope, `origin`, installed `wiki` registry and hooks, tracked `.specify/extensions/wiki/wiki-config.yml`, and `wiki/` content baseline using `specs/006-adopt-flow-wiki-identity/quickstart.md`; preserve the existing Diagram-removal changes as separate scope.
- [X] T002 Verify the pinned Specify version in `mise.toml`, the runtime allowlist in `.extensionignore`, and the availability of tag `v1.1.2` for the migration fixture described in `specs/006-adopt-flow-wiki-identity/quickstart.md`.

## Phase 2: Foundational

Resolve cross-artifact and historical-boundary issues before implementation.

- [X] T003 Run `$speckit-analyze` against `specs/006-adopt-flow-wiki-identity/spec.md`, `plan.md`, and `tasks.md`; reconcile any requirement, contract, or scope finding in those exact files before changing `extension.yml`.

**Checkpoint**: The active identity mapping, constitutional boundaries, and current checkout state are agreed and reviewable.

## Phase 3: User Story 1 - Recognize the Flow Wiki Extension (Priority: P1) 🎯 MVP

**Goal**: Produce a locally installable `flow-wiki` extension that identifies the `spec-kit-flow-wiki` project and exposes the same five actions.

**Independent Test**: Install the root source into a clean disposable Codex-skills consumer and inspect the manifest, registered commands, packaged payload, minimum CLI version, and project identity. Published URL verification completes in the final phase.

- [X] T004 [US1] Update root `extension.yml` to ID `flow-wiki`, name Flow Wiki, author `pegagio`, target repository metadata, five `speckit.flow-wiki.*` declarations, `flow-wiki-config.yml`, new hook targets, minimum Spec Kit 1.0.1, and target breaking version 2.0.0.
- [X] T005 [P] [US1] Rename `commands/speckit.wiki.init.md` to `commands/speckit.flow-wiki.init.md` and keep its initialization contract intact.
- [X] T006 [P] [US1] Rename `commands/speckit.wiki.ingest.md` to `commands/speckit.flow-wiki.ingest.md` and keep its source and write contract intact.
- [X] T007 [P] [US1] Rename `commands/speckit.wiki.query.md` to `commands/speckit.flow-wiki.query.md` and keep its read-only citation contract intact.
- [X] T008 [P] [US1] Rename `commands/speckit.wiki.lint.md` to `commands/speckit.flow-wiki.lint.md` and keep its bounded repair contract intact.
- [X] T009 [P] [US1] Rename `commands/speckit.wiki.status.md` to `commands/speckit.flow-wiki.status.md` and keep its read-only status contract intact.
- [X] T010 [P] [US1] Update `config-template.yml` examples to `.specify/extensions/flow-wiki/flow-wiki-config.yml` and `SPECKIT_FLOW_WIKI_*` without changing settings, defaults, or precedence.
- [X] T011 [US1] Update the current project identity, extension ID, local install example, and target published location in `README.md`; label unverified published links as pending until T031-T032.
- [X] T012 [US1] Run the clean-consumer install in `specs/006-adopt-flow-wiki-identity/quickstart.md` and verify `extension.yml`, five packaged `commands/speckit.flow-wiki.*.md` files, `.extensionignore` payload, CLI minimum, and absence of old command registrations.

**Checkpoint**: The renamed source installs locally and the project/extension identity is visible in a clean consumer. The published repository location remains a final release gate.

## Phase 4: User Story 2 - Use Clearly Named Wiki Skills (Priority: P1)

**Goal**: Make every generated skill and command lead to the correct wiki workflow with new internal references and unchanged safety boundaries.

**Independent Test**: In the clean consumer, inspect all five generated `speckit-flow-wiki-*` IDs and invoke each action against representative valid, malformed, untrusted, bounded, and failed-write fixtures.

- [X] T013 [US2] Replace old command, configuration-path, and override-prefix references in `commands/speckit.flow-wiki.init.md`, including the schema text it creates, while preserving idempotency and repository containment.
- [X] T014 [P] [US2] Replace old command references in `commands/speckit.flow-wiki.ingest.md` while preserving source IDs, citation requirements, page limits, conflicts, and coherent writes.
- [X] T015 [P] [US2] Replace old command references in `commands/speckit.flow-wiki.query.md` while preserving read-only bounded evidence and coverage verdicts.
- [X] T016 [P] [US2] Replace old command references in `commands/speckit.flow-wiki.lint.md` while preserving configured checks, semantic report-only findings, and mechanical repair limits.
- [X] T017 [P] [US2] Replace old command references in `commands/speckit.flow-wiki.status.md` while preserving structural-only reads and one evidence-backed next action.
- [X] T018 [P] [US2] Update the current action map and examples in `docs/concepts.md` to `speckit.flow-wiki.*` without changing the documented behavior.
- [X] T019 [US2] Reinstall the validated source in the clean fixture from `specs/006-adopt-flow-wiki-identity/quickstart.md` and verify five generated `.agents/skills/speckit-flow-wiki-*/SKILL.md` files, action-identifying generated names, new hook targets, and zero old skill or command aliases.
- [X] T020 [US2] Execute the action and failure matrix in `specs/006-adopt-flow-wiki-identity/quickstart.md`; compare init, ingest, query, lint, and status outcomes with `specs/006-adopt-flow-wiki-identity/contracts/extension-identity.md` and record any behavioral drift before proceeding.

**Checkpoint**: All five actions work under the new command and skill IDs in a clean consumer, with existing provenance, limits, read/write boundaries, and failure behavior.

## Phase 5: User Story 3 - Move an Existing Consumer Safely (Priority: P2)

**Goal**: Give existing `wiki` consumers an explicit cutover path that preserves knowledge and configuration values without aliases or duplicate active hooks.

**Independent Test**: Start from the `v1.1.2` fixture, install the new extension beside the old one without running actions, transfer values, remove the old extension, and compare the wiki bytes and effective configuration.

- [X] T021 [US3] Write `docs/migration.md` with the old-to-new extension, command, skill, config, environment, and hook mapping; document temporary coexistence, no aliases, value precedence, rollback evidence, and failed-state reporting from `contracts/consumer-migration.md`.
- [X] T022 [P] [US3] Update only active command examples in `wiki/SCHEMA.md` and the generated maintenance note in `wiki/INDEX.md` to `speckit.flow-wiki.*`; preserve user-authored scope, rules, source records, and cited wiki page history.
- [X] T023 [US3] Link `docs/migration.md` from the current install and upgrade guidance in `README.md`, including the exact old-ID cutover and unsupported-version message.
- [X] T024 [US3] Run the migrated-consumer fixture in `specs/006-adopt-flow-wiki-identity/quickstart.md`; verify transferred `flow-wiki-config.yml` values and `SPECKIT_FLOW_WIKI_*` precedence, one active hook per event, absent old aliases, and byte-identical `wiki/` state.
- [X] T025 [US3] After T024 passes, migrate this checkout's tracked `.specify/extensions/wiki/`, `.specify/extensions/flow-wiki/`, `.specify/extensions.yml`, `.specify/extensions/.registry`, and `.agents/skills/speckit-wiki-*/` through Specify's remove/add workflow; preserve the old `wiki-config.yml` values and verify the generated `speckit-flow-wiki-*` skills without recursive text replacement.
- [X] T026 [US3] Exercise partial install, invalid configuration, old-name override, duplicate-hook coexistence, and failed-write cases from `specs/006-adopt-flow-wiki-identity/quickstart.md`; record the actual active extension and confirm unchanged wiki data and honest failure reporting.

**Checkpoint**: A migrated disposable consumer and this checkout use only `flow-wiki`; durable wiki knowledge and intended configuration values are preserved.

## Phase 6: Polish & Cross-Cutting Concerns

Finish current documentation, validate the full tree, and complete external identity only after local evidence is reviewable.

- [X] T027 Update only the active project title in `.specify/memory/constitution.md` if needed for `spec-kit-flow-wiki`; preserve all normative text, version, dates, and historical governance meaning.
- [X] T028 [P] Append a breaking-identity migration entry in `CHANGELOG.md` for target version 2.0.0 without rewriting previous releases.
- [X] T029 Run the active-reference, privacy, historical-boundary, YAML/Markdown, payload, and `git diff --check` review in `specs/006-adopt-flow-wiki-identity/quickstart.md`; reconcile active references in `extension.yml`, `commands/`, `config-template.yml`, `README.md`, `docs/`, `.specify/extensions.yml`, `wiki/SCHEMA.md`, and `wiki/INDEX.md`. Preserve historically accurate cited claims in other wiki pages.
- [X] T030 Run the full disposable validation in `specs/006-adopt-flow-wiki-identity/quickstart.md`, then `$speckit-converge` against `specs/006-adopt-flow-wiki-identity/spec.md`, `plan.md`, and `tasks.md` until no implementation gaps remain.
- [X] T031 After local validation and review, rename the published GitHub repository to `pegagio/spec-kit-flow-wiki`; verify the target before updating `.git/config` `origin`, and record the verified location in `specs/006-adopt-flow-wiki-identity/quickstart.md`.
- [X] T032 After explicit publication authorization, publish the reviewed `extension.yml` and `README.md` at the verified repository location and validate installation from the published instructions in a fresh consumer using `specs/006-adopt-flow-wiki-identity/quickstart.md`.

**Checkpoint**: The source, installed consumers, migration guide, and verified published location agree. The v2.0.0 archive installs from the published README instructions in a fresh consumer.

## Dependencies & Execution Order

### Phase Dependencies

- Setup T001-T002 precedes the T003 consistency gate.
- T003 blocks all source changes.
- US1 source identity T004-T011 precedes clean installation T012.
- US2 command-content work T013-T018 follows the corresponding US1 file renames; T019-T020 follow all five content updates.
- US3 documentation T021-T023 follows the new identity contract; migrated fixture T024 follows validated source; this checkout migration T025 follows T024; failure checks T026 follow the migration path.
- Final local review T027-T030 follows all user stories. External steps T031-T032 follow reviewed local validation and verified publication authority.

### User Story Dependencies

- **US1 (P1)**: Starts after foundation and delivers the local identity MVP independently. Its published-link acceptance is completed at T031-T032.
- **US2 (P1)**: Uses the new command files from US1 but is independently testable in a clean consumer without migrating an old installation.
- **US3 (P2)**: Uses the validated new extension from US1-US2 but is independently testable in a disposable old-to-new consumer fixture.

### Parallel Opportunities

- After T004, T005-T010 can proceed in parallel because each changes a different source file.
- After the matching rename task, T014-T018 can proceed in parallel on separate command or documentation files; T013 owns the init file.
- T021 and T022 can proceed in parallel because the migration guide and current schema examples are separate files.
- T027 and T028 can proceed in parallel because constitution title and changelog are separate artifacts.

## Parallel Examples

- **US1**: Work on T005 `commands/speckit.flow-wiki.init.md`, T007 `commands/speckit.flow-wiki.query.md`, and T010 `config-template.yml` after T004, then run T012.
- **US2**: Work on T014 `commands/speckit.flow-wiki.ingest.md` and T018 `docs/concepts.md` after their prerequisites, then run T019-T020.
- **US3**: Work on T021 `docs/migration.md` and T022 `wiki/SCHEMA.md` independently, then run T024-T026 in order.

## Implementation Strategy

### MVP First

Complete T001-T012 and validate the renamed extension in a clean consumer. The remaining local stories can then be completed without making an unverified published-location claim.

### Incremental Delivery

Complete US1 source identity, then US2 action correctness, then US3 migration and this checkout's generated consumer state. Run the full local gate before the external repository rename and publication steps.

## Notes

- `[P]` means different files and no dependency on an incomplete task at that point.
- Existing historical specs, completed queue records, older changelog entries, and cited wiki claims are evidence, not rename targets.
- No task authorizes an incidental commit, push, release, or other unrelated change; external publication is an explicit final gate.

## Phase 7: Convergence

- [X] T033 Update `docs/migration.md` with the verified published `v2.0.0` archive installation command and remove the completed pre-publication contingency per FR-005 and plan: published repository (partial).

## Phase 8: Convergence

- [X] T034 Correct the `docs/migration.md` verification order so temporary duplicate hooks do not block removal of `wiki`, and verify exactly one active ingest hook only after removal per FR-006 and US3/AC1 (partial).
