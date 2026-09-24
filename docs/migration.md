# Migrate from Wiki to Flow Wiki

This guide moves a consumer from extension ID `wiki` to `flow-wiki`. The migration preserves the existing `wiki/` content and configuration values. The new extension does not provide old command, skill, configuration-file, or environment-variable aliases.

## Identity mapping

| Old identity | New identity |
|---|---|
| Extension ID `wiki` | Extension ID `flow-wiki` |
| `/speckit.wiki.init` | `/speckit.flow-wiki.init` |
| `/speckit.wiki.ingest` | `/speckit.flow-wiki.ingest` |
| `/speckit.wiki.query` | `/speckit.flow-wiki.query` |
| `/speckit.wiki.lint` | `/speckit.flow-wiki.lint` |
| `/speckit.wiki.status` | `/speckit.flow-wiki.status` |
| `speckit-wiki-<action>` skill ID | `speckit-flow-wiki-<action>` skill ID |
| `.specify/extensions/wiki/wiki-config.yml` | `.specify/extensions/flow-wiki/flow-wiki-config.yml` |
| `SPECKIT_WIKI_<SETTING>` | `SPECKIT_FLOW_WIKI_<SETTING>` |
| Hook owner `wiki`, command `speckit.wiki.ingest` | Hook owner `flow-wiki`, command `speckit.flow-wiki.ingest` |

The Specify-generated human-readable skill names may differ from `FlowKit <action>`; use the generated `speckit-flow-wiki-*` IDs. Old names stop resolving after the old extension is removed.

## Migration steps

1. Use Specify CLI 1.0.1 or newer for the published `v2.0.0` archive, or 1.0.10.dev0 or newer for the current checkout. Record the current extension registry, configured hooks, active command and skill references, old config values, and environment overrides. Save a rollback copy of `wiki-config.yml` outside `.specify/extensions/wiki/` and record a content baseline for `wiki/`.
2. Install the published `v2.0.0` archive while `wiki` is still installed: `specify extension add flow-wiki --from https://github.com/pegagio/spec-kit-flow-wiki/archive/refs/tags/v2.0.0.zip`. For development from a local checkout, use `specify extension add --dev /path/to/spec-kit-flow-wiki`. See the [README installation instructions](../README.md#installation) for the source warning and other installation options.
3. Do not invoke either extension or trigger `after_plan` or `after_implement` workflows while both extensions are installed. This avoids duplicate optional ingest hooks during the temporary coexistence period.
4. Transfer every value from `.specify/extensions/wiki/wiki-config.yml` to `.specify/extensions/flow-wiki/flow-wiki-config.yml` without changing the value. Preserve per-setting precedence: extension defaults, config file, `SPECKIT_FLOW_WIKI_*` environment overrides, then per-invocation `key=value` arguments. For each environment override, export the corresponding `SPECKIT_FLOW_WIKI_<SETTING>` value and stop exporting `SPECKIT_WIKI_<SETTING>`; the old prefix is ignored.
5. Replace active command and skill references using the mapping above. Preserve historical feature specs, changelog entries, and cited source text when the old name is historically accurate. Review `.specify/extensions.yml` and ensure the configured hooks point to `flow-wiki` and `speckit.flow-wiki.ingest`.
6. Verify the new extension registry, all five generated skill IDs, the effective configuration values, and the new Flow Wiki hook targets. Compare `wiki/` with the recorded baseline. Do not continue if the new config is invalid, a new hook target is missing, or any wiki file changed unexpectedly. The old and new ingest hooks may both be present until the old extension is removed; do not run them during this period.
7. Remove the old registration with `specify extension remove wiki --keep-config --force`. This unregisters its commands and hooks while preserving the old config directory for rollback. Confirm the registry contains `flow-wiki` and no `wiki` entry, exactly one active Flow Wiki ingest hook per configured event, no active old ingest hook, and no `speckit.wiki.*` command or `speckit-wiki-*` skill. Compare `wiki/` with the baseline again. If any check fails, report the unfinished state and preserve the rollback copy. After every check passes, move the old config copy to the chosen rollback location and remove the inactive `.specify/extensions/wiki/` directory.

## Failure and rollback

If installation, configuration transfer, old-extension removal, or verification fails, stop and report the exact unfinished step and which extension is currently registered. Preserve the original `wiki/` tree and the recorded configuration copy. Do not claim migration complete or reconstruct wiki sources, pages, citations, schema, or index as part of identity migration.

Keep the old config backup and wiki baseline until the new extension passes all checks. If rollback is needed, restore the recorded old config and reinstall the prior extension version from its trusted source; then verify its registry, hooks, generated skills, and wiki baseline before resuming normal use. Rollback is a recovery action, not a compatibility alias.

## Unsupported Specify versions

The current checkout requires Specify CLI `>=1.0.10.dev0`. When a CLI version that performs manifest compatibility checks is too old, the installer reports:

```text
Extension requires spec-kit >=1.0.10.dev0, but <installed-version> is installed.
Upgrade spec-kit with: uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

Upgrade Specify before installing. An older installer that does not enforce the manifest minimum is unsupported and must not be used to install or migrate this extension.
