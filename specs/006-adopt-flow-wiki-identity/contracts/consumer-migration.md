# Consumer Migration Contract

This contract applies to a consumer moving from installed extension `wiki` to `flow-wiki`. It defines the intended user-visible procedure and validation; it is not an instruction to migrate a real consumer during planning.

## Inputs and Inventory

Record the installed old extension, its active hooks, every current `speckit.wiki.*` invocation and `speckit-wiki-*` skill reference, the old configuration file, `SPECKIT_WIKI_*` overrides, and the selected wiki state directory. Check for a simultaneous `flow-wiki` installation before changing anything. Treat historical specs, cited source text, and older changelog entries as evidence rather than active invocations.

## Transition

1. Preserve the consumer's `wiki/` state and record a content comparison baseline.
2. Record the old configuration values and prepare their mapping to the new names without changing the existing installation.
3. Install `flow-wiki` using documented Specify commands. During temporary coexistence, do not run either extension's hooks or wiki actions.
4. Transfer the recorded values to `.specify/extensions/flow-wiki/flow-wiki-config.yml`, rename active environment overrides to `SPECKIT_FLOW_WIKI_*`, and replace active command, skill, and hook references with the mapping in [extension-identity.md](./extension-identity.md).
5. Verify the new registry, generated skills, configuration, and hook targets; then remove the old extension. Compare wiki-state content with the baseline and confirm the five actions. Mark migration complete only after all checks pass.

No old command, skill, file, or environment name remains active in the new extension. The old and new extensions must not both own the same optional hooks in the completed state.

## Failure Contract

If inventory, configuration transfer, extension removal, installation, or verification fails, report the exact unfinished step and which extension is currently active. Preserve the original wiki state and recorded configuration values. Do not silently reconstruct source records, schema, pages, citations, or index entries, and do not claim migration complete. Recovery may resume from the recorded state after maintainer review.

## Published Location

The migration guide must use the verified `pegagio/spec-kit-flow-wiki` location when published. The repository rename and remote update occur after local extension validation. A redirect from the old repository path is not an active installation target or a substitute for updating current links. See [GitHub's rename guidance](https://docs.github.com/en/repositories/creating-and-managing-repositories/renaming-a-repository).
