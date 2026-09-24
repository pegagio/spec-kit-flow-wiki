# Changelog

All notable changes to the Spec Kit Flow Wiki extension are documented here.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- Raised the minimum supported Specify CLI version for the current checkout to 1.0.10.dev0.

## [2.0.0] - 2026-09-23

### Changed

- Renamed the project identity to `spec-kit-flow-wiki` and the extension ID to `flow-wiki`.
- Replaced the five `speckit.wiki.*` commands and `speckit-wiki-*` skills with `speckit.flow-wiki.*` commands and `speckit-flow-wiki-*` skills; old names are not aliases.
- Replaced `wiki-config.yml` and `SPECKIT_WIKI_*` with `flow-wiki-config.yml` and `SPECKIT_FLOW_WIKI_*`; existing consumers must migrate their values explicitly.
- Raised the minimum supported Specify CLI version to 1.0.1.

## [1.1.2] - 2026-09-23

### Changed

- Fixed a bug in `.extensionignore` 

## [1.1.0] - 2026-08-26

### Changed

- Completed a retrospective Spec Kit Time Machine pass across wiki initialization, ingestion, query, lint, and status, with feature specifications, implementation plans, contracts, validation checklists, and execution tasks retained under `specs/`.
- Hardened `/speckit.wiki.init` configuration handling and repository-boundary validation while preserving non-destructive initialization.
- Made `/speckit.wiki.ingest` an all-or-nothing update: it now prepares and validates pages, source-registry state, and index changes before committing them together.
- Tightened `/speckit.wiki.query` to produce bounded, cited answers with explicit coverage gaps instead of unsupported conclusions.
- Refined `/speckit.wiki.lint` into deterministic evidence-based health checks with a narrow mechanical repair boundary and accurate derived reports.
- Refined `/speckit.wiki.status` into a strictly read-only structural snapshot with explicit unknown values, bounded filters, and one deterministic evidence-backed next action.

## [1.0.0] - 2026-07-03

### Added

- Initial release, adapting the LLM Wiki pattern from Andrej Karpathy's
  ["LLM Wiki" gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
  to spec-driven development: a persistent, LLM-maintained, compounding
  project wiki with three layers (raw sources → wiki pages → schema).
- `/speckit.wiki.init` — create the wiki skeleton: `SCHEMA.md` (structure and
  maintenance rules), `INDEX.md` (page directory), `sources.md` (source registry).
- `/speckit.wiki.ingest` — register a source (feature artifacts, file, or URL),
  extract wiki-worthy knowledge, and update the related pages with per-claim
  source citations, cross-links, conflict markers, and a hard per-run page cap.
- `/speckit.wiki.query` — answer questions strictly from wiki pages with page
  and source citations; report coverage gaps as concrete ingest suggestions.
- `/speckit.wiki.lint` — the maintenance pass: index drift, broken links,
  orphan pages, contradictions, stale claims, uncited claims; mechanical fixes
  applied per config, semantic issues reported with suggested edits only.
- `/speckit.wiki.status` — read-only snapshot (counts, freshness, open lint
  issues) plus exactly one recommended next action; the session-resume entry point.
- Optional hooks: `after_plan` → `speckit.wiki.ingest`,
  `after_implement` → `speckit.wiki.ingest`.
- `config-template.yml` for wiki location, ingest caps, query slices, and lint policy.
