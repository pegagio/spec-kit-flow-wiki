# Data Model: Flow Wiki Identity

This feature changes identity metadata and consumer references. It does not introduce a database or change the wiki's knowledge schema.

## Project Identity

| Field | Value or rule |
|---|---|
| Family | Spec Kit Flow |
| Repository name | `spec-kit-flow-wiki` |
| Target published location | `pegagio/spec-kit-flow-wiki` |
| Publication state | Planned → renamed → location verified |

The target location is not presented as available until verification succeeds. The local checkout directory name does not define the published project identity.

## Extension Identity

| Field | Value or rule |
|---|---|
| Extension ID | `flow-wiki` |
| Human-readable extension name | Identifies Flow Wiki within Spec Kit Flow |
| Configuration file | `flow-wiki-config.yml` |
| Environment override prefix | `SPECKIT_FLOW_WIKI_*` |
| Actions | Exactly `init`, `ingest`, `query`, `lint`, `status` |
| Hook target | `speckit.flow-wiki.ingest` for existing optional hooks |
| Release version | Target `2.0.0` because old identifiers are removed |
| Minimum Spec Kit version | `1.0.1`, the version used to verify renamed command and skill generation |

The extension ID is unique within a consumer's installed extension registry. The installed snapshot lives under `.specify/extensions/flow-wiki/`; it is derived from the root extension source.

## Wiki Action

| Action | Command | Generated skill ID | Behavioral authority |
|---|---|---|---|
| Init | `speckit.flow-wiki.init` | `speckit-flow-wiki-init` | Existing initialization contract |
| Ingest | `speckit.flow-wiki.ingest` | `speckit-flow-wiki-ingest` | Existing ingestion contract |
| Query | `speckit.flow-wiki.query` | `speckit-flow-wiki-query` | Existing cited-query contract |
| Lint | `speckit.flow-wiki.lint` | `speckit-flow-wiki-lint` | Existing lint and repair contract |
| Status | `speckit.flow-wiki.status` | `speckit-flow-wiki-status` | Existing read-only status contract |

Specify generates human-readable skill names from command names. Exact `FlowKit <action>` display names are not required. No old command or skill ID is registered as an alias.

## Consumer Configuration

Configuration values retain their current fields, defaults, validation, and precedence: extension defaults, optional installed file, environment overrides, then explicit command arguments where supported. Only the file and environment prefix change. Old names are migration inputs, not active configuration after the new extension is installed.

The configured wiki directory remains `wiki/` by default. The directory contains the schema, source registry, index, pages, citations, and derived reports. Identity migration does not rewrite this state.

## Consumer Migration

| State | Meaning | Allowed next state |
|---|---|---|
| Inventoried | Old installation, overrides, hooks, and wiki state recorded | Prepared |
| Prepared | New values and rollback copy reviewed; wiki state unchanged | Coexisting or Failed |
| Coexisting | New extension installed beside old one; no wiki action or hook is run | Installed or Failed |
| Installed | New configuration and references applied; old extension removed | Verified or Failed |
| Verified | Five actions and effective values checked; wiki state unchanged | Complete |
| Failed | A step did not finish; report exact state and preserve wiki data | Prepared after user-directed recovery |
| Complete | New identity is active and old command, skill, and config names are inactive | None |

The migration cannot claim `Complete` solely because the new extension directory exists. A failed step must report whether the old installation, new installation, or neither is active. A real consumer migration is a separate execution decision; the feature validates this model in disposable consumers.
