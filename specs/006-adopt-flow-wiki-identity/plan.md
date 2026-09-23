# Implementation Plan: Adopt Spec Kit Flow Wiki Identity

**Branch**: `develop` (no feature branch created) | **Date**: 2026-09-23 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `specs/006-adopt-flow-wiki-identity/spec.md`

## Summary

Rename the extension's source identity to `flow-wiki`, its five commands to `speckit.flow-wiki.<action>`, and its generated skill IDs to `speckit-flow-wiki-<action>`. Update active configuration, hooks, examples, and migration guidance while preserving the existing wiki knowledge and command behavior. Validate the actual installed payload in disposable consumers, then complete and verify the published repository rename before publishing new installation links.

## Technical Context

**Language/Version**: Declarative Markdown and YAML compatible with the pinned Specify CLI 1.0.1

**Primary Dependencies**: Existing Specify CLI 1.0.1 and its Codex skills integration; no new runtime, library, framework, or build tool

**Storage**: Existing repository-contained `wiki/` Markdown and YAML; no schema or knowledge migration

**Testing**: Static identity and contract checks, `git diff --check`, and clean and migrated disposable Specify consumers

**Target Platform**: Spec Kit consumers; Codex skills mode is the reference consumer for generated skill IDs

**Project Type**: Declarative Spec Kit extension

**Performance Goals**: No change to existing page, word, query, context, or staleness limits; no extra wiki reads or writes during migration

**Constraints**: Five actions only; no old command or skill aliases; old configuration names are inactive; historical evidence and source provenance remain intact; no dependency addition

**Scale/Scope**: One extension source, five command files, one configuration identity, one published repository rename, and representative clean and migrated consumers

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle or boundary | Design response | Gate |
|---|---|---|
| Evidence before assertion; honest uncertainty | Preserve source IDs, citations, conflicting claims, and missing-evidence behavior. Treat unverified published links as targets until checked. | Pass |
| Bounded context and explicit scope | Keep current limits and command read scopes; migration reads only named configuration and identity artifacts. | Pass |
| Human authority and untrusted text | Rename operational references without rewriting claims or source history; wiki content never authorizes broader migration. | Pass |
| Repository-native, deterministic, atomic maintenance | Keep plain-text artifacts and deterministic names; preserve consumer wiki state if removal, install, or configuration transfer fails. | Pass |
| Portable, minimal architecture | Reuse Specify's declarative extension and generated skills; accept generated display names instead of adding metadata tooling. | Pass |
| Operational boundaries | Init, ingest, query, lint, and status retain their respective write and read contracts. | Pass |
| Merge-bounded flow-back | Keep this feature's spec, plan, contracts, tasks, and implementation consistent before merge; preserve merged historical feature meaning. | Pass |

**Pre-design gate**: PASS. No new dependency or constitutional exception is required.

**Post-design gate**: PASS. The contracts below preserve provenance, configured limits, mutation boundaries, and failure behavior. The generated display-name decision is reconciled in the spec.

## Phase 0: Research

[research.md](./research.md) records the pinned CLI's naming behavior, configuration resolution, source-versus-installed-snapshot boundary, repository rename sequencing, and validation strategy. All technical-context choices are resolved.

## Phase 1: Design

### Source identity

Update root `extension.yml` with ID `flow-wiki`, a Flow Wiki name, `speckit.flow-wiki.*` command declarations, `flow-wiki-config.yml`, new hook targets, current repository metadata, a verified minimum Spec Kit version of 1.0.1, and a breaking release version at publication. Rename the five root command files to match their declared names and update cross-command references, configuration paths, and override prefixes inside them. Keep their behavioral contracts, defaults, and safety rules unchanged. Keep `.extensionignore` narrowly scoped to the runtime payload.

### Consumer identity and migration

Specify generates `speckit-flow-wiki-*` skill IDs from the new commands; generated human-readable names are accepted. Update root `config-template.yml`, README, current conceptual documentation, and active installation examples. Add `docs/migration.md` for a value-preserving transfer from `wiki-config.yml` and `SPECKIT_WIKI_*` to `flow-wiki-config.yml` and `SPECKIT_FLOW_WIKI_*`. In disposable migration tests, install the new extension, transfer values, verify it, and then remove the old extension without running actions during temporary coexistence. Preserve `wiki/` bytes and avoid old-name aliases. Rebuild this checkout's tracked installed snapshot and generated skills through Specify's supported commands after the source is validated; do not recursively edit the old snapshot.

Review the active `.specify/extensions.yml` hooks and current operational examples in `wiki/SCHEMA.md` and the generated maintenance note in `wiki/INDEX.md` for identity references. Keep the schema's meaning, derived page index, and cited wiki page history unchanged except for those active command names. Preserve merged `specs/001-005/`, completed Time Machine queue records, and existing changelog history; append a new release entry rather than rewriting older entries. If the active constitution title is changed for project identity, treat it as editorial and preserve all normative text and versioned governance.

### Published repository

The target repository identity is `pegagio/spec-kit-flow-wiki`. Prepare source and migration guidance against that target, but do not assert that the new URL works until the published repository has been renamed and verified. After local validation, perform the repository rename as the final external identity step, update this checkout's `origin` URL, verify fetch and installation links, and then publish release instructions. Existing GitHub redirects do not replace updating active links or local remotes; see [GitHub's repository rename guidance](https://docs.github.com/en/repositories/creating-and-managing-repositories/renaming-a-repository).

### Contracts and validation

[data-model.md](./data-model.md) defines identity mappings and migration states. [contracts/extension-identity.md](./contracts/extension-identity.md) defines the manifest, command, skill, configuration, and hook contract. [contracts/consumer-migration.md](./contracts/consumer-migration.md) defines the transition and failure boundary. [quickstart.md](./quickstart.md) gives runnable disposable-consumer validation and published-location checks. The feature does not add executable application code.

## Project Structure

### Documentation for this feature

```text
specs/006-adopt-flow-wiki-identity/
├── spec.md
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   ├── extension-identity.md
│   └── consumer-migration.md
└── checklists/requirements.md
```

### Affected repository surfaces

```text
extension.yml
commands/speckit.flow-wiki.{init,ingest,query,lint,status}.md
config-template.yml
.extensionignore
README.md
docs/concepts.md
docs/migration.md
CHANGELOG.md
.specify/memory/constitution.md
.specify/extensions.yml
.specify/extensions/.registry                 # Specify-managed installed state
.specify/extensions/flow-wiki/                # Specify-managed installed snapshot
.agents/skills/speckit-flow-wiki-*/           # Specify-generated Codex skills
wiki/SCHEMA.md                                # only current operational examples if needed
```

**Structure Decision**: Keep the extension declarative in its existing root layout. Treat installed consumer files as generated outputs and historical specifications and wiki claims as evidence, not rename targets.

## Complexity Tracking

No constitutional violations or new abstractions are proposed.
