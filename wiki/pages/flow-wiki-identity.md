---
title: Flow Wiki identity
type: decision
sources:
  - S006
  - S007
  - S008
updated: 2026-09-23
---

# Flow Wiki identity

Flow Wiki is the wiki extension in the Spec Kit Flow project family. Its repository is `pegagio/spec-kit-flow-wiki`, its extension ID is `flow-wiki`, and the published release is `v2.0.0`. (S006)

The five actions retain their distinct responsibilities under `speckit.flow-wiki.{init,ingest,query,lint,status}`. (S006, S008)

Specify generates corresponding `speckit-flow-wiki-<action>` skill IDs; its generated human-readable skill names are accepted. (S007, S008)

The published `v2.0.0` archive requires Specify CLI 1.0.1 or newer; the current checkout requires 1.0.10.dev0 or newer. Its installed configuration is `flow-wiki-config.yml`, with `SPECKIT_FLOW_WIKI_*` environment overrides. Old command and skill IDs are not aliases for the new ones. (S006)

The [wiki command lifecycle](./wiki-command-lifecycle.md) describes each action, and [configuration resolution](./configuration-resolution.md) records the current override namespace and precedence. (S006)

Existing consumers follow the [consumer identity migration](./consumer-identity-migration.md) to transfer active references and configuration without changing accumulated wiki knowledge. (S007)

The identity change does not add actions or change source IDs, citations, conflict handling, schema authority, configured limits, or the existing read and write boundaries. Historical specs, source records, and changelog entries retain accurate old names as evidence, while active instructions use the new identity. (S008)
