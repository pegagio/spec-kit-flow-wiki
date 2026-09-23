# Extension Identity Contract

The root `extension.yml` is the authoritative published extension manifest. The example below names the externally visible fields; the existing defaults, descriptions, and requirements remain part of the full manifest.

```yaml
extension:
  id: flow-wiki
  name: Flow Wiki
  version: 2.0.0
  author: pegagio
  repository: https://github.com/pegagio/spec-kit-flow-wiki
  homepage: https://github.com/pegagio/spec-kit-flow-wiki
requires:
  speckit_version: ">=1.0.1"
provides:
  commands:
    - name: speckit.flow-wiki.init
      file: commands/speckit.flow-wiki.init.md
    - name: speckit.flow-wiki.ingest
      file: commands/speckit.flow-wiki.ingest.md
    - name: speckit.flow-wiki.query
      file: commands/speckit.flow-wiki.query.md
    - name: speckit.flow-wiki.lint
      file: commands/speckit.flow-wiki.lint.md
    - name: speckit.flow-wiki.status
      file: commands/speckit.flow-wiki.status.md
  config:
    - name: flow-wiki-config.yml
      template: config-template.yml
hooks:
  after_plan:
    command: speckit.flow-wiki.ingest
    optional: true
  after_implement:
    command: speckit.flow-wiki.ingest
    optional: true
```

The five declared command names are unique, use the extension ID as their namespace, and point to packaged files. The extension registers no `speckit.wiki.*` command. The target repository URLs become published claims only after the external rename is verified.

## Generated Codex Skills

For a consumer initialized with Codex skills enabled, Specify creates exactly these skill IDs:

| Command | Skill ID |
|---|---|
| `speckit.flow-wiki.init` | `speckit-flow-wiki-init` |
| `speckit.flow-wiki.ingest` | `speckit-flow-wiki-ingest` |
| `speckit.flow-wiki.query` | `speckit-flow-wiki-query` |
| `speckit.flow-wiki.lint` | `speckit-flow-wiki-lint` |
| `speckit.flow-wiki.status` | `speckit-flow-wiki-status` |

The generated names must distinguish the five wiki actions. Specify's generated names are accepted; no custom display metadata or post-install step is required. The installed extension does not register `speckit-wiki-*` aliases.

## Configuration Contract

The installed file path is `.specify/extensions/flow-wiki/flow-wiki-config.yml`. Environment overrides use `SPECKIT_FLOW_WIKI_*`, for example `SPECKIT_FLOW_WIKI_INGEST_MAX_PAGES_PER_INGEST=8`. Preserve existing keys, types, defaults, validation, and precedence. A missing higher-precedence value falls through as before. Old file and environment names are ignored by the renamed extension and must be migrated explicitly.

## Behavioral Contract

| Action | Read boundary | Write boundary |
|---|---|---|
| Init | Validated configuration and initialization sentinel | Initial schema, index, registry, or explicitly requested appended scope only |
| Ingest | One selected source and bounded relevant wiki context | Registered source, affected pages, and index as one coherent change |
| Query | Bounded cited wiki pages and structural metadata | None |
| Lint | Selected bounded wiki artifacts and relevant registered sources | Derived report and documented mechanical index/link fixes only |
| Status | Structural wiki metadata and bounded active-feature metadata | None |

All five retain repository containment, configured limits, untrusted-text handling, source provenance, conflict visibility, and existing failure behavior. The wiki's default state path and content format do not change.
