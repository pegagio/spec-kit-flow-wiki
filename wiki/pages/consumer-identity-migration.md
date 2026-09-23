---
title: Consumer identity migration
type: howto
sources:
  - S007
updated: 2026-09-23
---

# Consumer identity migration

Moving a consumer from `wiki` to `flow-wiki` is an explicit cutover: the new extension does not accept old command, skill, configuration-file, or environment-variable names as aliases. Inventory active references, hooks, configuration values, and the `wiki/` content baseline before installing the new extension. Preserve the old configuration outside its installed directory for rollback. (S007)

Use Specify CLI 1.0.1 or newer and install the published `v2.0.0` archive. The old and new extensions may coexist briefly, but neither wiki action nor ingest hook should run until the old registration is removed because both can offer the optional hooks. (S007)

Transfer each old `wiki-config.yml` value to `flow-wiki-config.yml` without changing it. Rename each `SPECKIT_WIKI_*` override to `SPECKIT_FLOW_WIKI_*`; the effective precedence remains extension defaults, file, environment, then per-invocation arguments. Replace active command, skill, and hook references while retaining historically accurate names in cited sources and older feature records. (S007)

Before removing `wiki`, verify the new registration, five skill IDs, effective configuration, new hook targets, and unchanged wiki bytes. After removal, verify only `flow-wiki` is registered, one new ingest hook exists per configured event, old commands and skills are absent, and wiki bytes still match the baseline. If a step fails, report the unfinished state and preserve the wiki and rollback copy; do not claim migration complete. (S007)

The [Flow Wiki identity](./flow-wiki-identity.md) records the current names and command boundaries. (S007)
