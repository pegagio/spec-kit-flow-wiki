# Research: Adopt Spec Kit Flow Wiki Identity

This research resolves the implementation choices for the identity change. The installed Specify CLI is pinned at 1.0.1 in `mise.toml`; observations of its local source apply to that version.

## Command and skill names

**Decision**: Declare extension ID `flow-wiki`, commands `speckit.flow-wiki.init`, `ingest`, `query`, `lint`, and `status`, and accept the generated skill display names. Require generated skill IDs `speckit-flow-wiki-<action>`.

**Rationale**: Specify CLI 1.0.1 validates command namespaces against the extension ID and derives Codex skill IDs from command names. Its skill generator also derives human-readable headings; manifest and command frontmatter cannot set exact `FlowKit <action>` names. The spec was reconciled during planning to accept those generated names while preserving the explicit skill IDs.

**Alternatives considered**: A Codex metadata copy step and a Specify CLI enhancement could force exact display names. Both add work and installation complexity for a requirement the maintainer chose to relax.

## Configuration and migration

**Decision**: Use `flow-wiki-config.yml` inside the installed `flow-wiki` extension and `SPECKIT_FLOW_WIKI_*` overrides. Keep the same value meanings, defaults, and precedence. Document copying existing values into the new configuration identity; do not read old names or provide command or skill aliases.

**Rationale**: The CLI resolves an extension's configuration under its ID and normalizes hyphens to underscores for environment overrides. A clean cutover matches the clarified contract and avoids two active configuration authorities. The wiki state directory remains `wiki/` by default.

**Alternatives considered**: Read old and new names together, or retain aliases for one release. The maintainer explicitly rejected old-name compatibility.

## Supported Spec Kit version

**Decision**: Set the renamed extension's minimum Spec Kit version to 1.0.1 and validate against the repository's pinned CLI. Do not claim support for older versions without a separate compatibility exercise.

**Rationale**: The current manifest advertises `>=0.2.0`, but this plan relies on command namespace validation and Codex skill generation observed in 1.0.1. The identity change is already a breaking release, so narrowing an unverified minimum is more accurate than carrying it forward.

**Alternatives considered**: Keep the old minimum without testing the new identity there. That would make a compatibility promise the feature has not verified.

## Source, installed snapshot, and historical records

**Decision**: Change the root extension source and active instructions. Treat `.specify/extensions/wiki/` and `.agents/skills/speckit-wiki-*/` as generated consumer installations; replace them through Specify's remove/add workflow after source validation, preserving and transferring tracked consumer configuration. Preserve historical `specs/001-005/`, completed Time Machine queue records, older changelog entries, source records, and cited wiki claims. Update only current operational command examples where needed.

**Rationale**: `.extensionignore` ships only the root manifest, commands, config template, README, and license. The installed wiki directory is a tracked 119-file snapshot, not authoritative extension source. A recursive textual rename would rewrite historical evidence and could hide migration mistakes. The completed Time Machine queue also contains an older absolute workspace path; it is historical queue evidence, not an active command contract.

**Alternatives considered**: A repository-wide string replacement. It would alter historical specs and sourced knowledge and make the diff difficult to review.

## Release identity and repository rename

**Decision**: Treat the extension ID change as a breaking release, with `2.0.0` as the target released version. Prepare and validate local source and a disposable consumer first. Then rename the published repository to `pegagio/spec-kit-flow-wiki`, verify the new location, update the local `origin` URL, and publish installation instructions only after the target exists. Keep release publication as a separate explicit execution step.

**Rationale**: Existing command, skill, configuration, and extension identifiers stop working. GitHub documents that repository renames redirect common Git operations, while recommending that local remotes be updated. The root manifest currently points to an older `formin/spec-kit-wiki` URL even though `origin` is `pegagio/spec-kit-wiki`; target links need verification before publication. See [GitHub's repository rename guidance](https://docs.github.com/en/repositories/creating-and-managing-repositories/renaming-a-repository).

**Alternatives considered**: Keep the old repository name or publish instructions to an unverified target. Both conflict with the clarified feature scope or risk broken installation guidance.

## Validation approach

**Decision**: Use static contract checks plus clean and migrated disposable Spec Kit consumers with Codex skills enabled. Compare wiki-state bytes before and after identity migration; exercise all five command contracts, malformed inputs, untrusted text, bounds, and failed writes with scoped fixtures. Check the packaged payload and generated skills, not just source files.

**Rationale**: This repository currently has no dedicated test suite or CI task; `mise.toml` only pins Specify CLI. The installation step copies a snapshot and generates skills, so source inspection alone cannot prove the consumer experience.

**Alternatives considered**: Adding a new test framework or relying only on a repository text scan. A framework is unnecessary for this rename, while text scans miss installation and generation behavior.
