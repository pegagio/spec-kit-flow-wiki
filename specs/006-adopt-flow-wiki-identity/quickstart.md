# Quickstart: Validate Flow Wiki Identity

Run these checks after implementation from the repository root. Use disposable consumers; do not install either test extension into a real project. See [extension-identity.md](./contracts/extension-identity.md) and [consumer-migration.md](./contracts/consumer-migration.md) for expected names and migration states.

## Prerequisites

- The pinned Specify CLI 1.0.1 is available.
- The root extension source has the new manifest and five renamed command files.
- Git tag `v1.1.2` is available locally for the old-extension migration fixture.
- The published repository rename is treated as a later verification step.

## Clean Consumer

```bash
SOURCE_REPO="$(git rev-parse --show-toplevel)"
FIXTURE_ROOT="$(mktemp -d)"
specify init "$FIXTURE_ROOT/clean" --non-interactive --ignore-agent-tools --integration codex --integration-options="--skills"
(cd "$FIXTURE_ROOT/clean" && specify extension add --dev "$SOURCE_REPO")
(cd "$FIXTURE_ROOT/clean" && specify extension list)
```

Inspect `$FIXTURE_ROOT/clean/.specify/extensions/flow-wiki/extension.yml`, its five packaged command files, the optional hook entries, `flow-wiki-config.yml`, and the five `.agents/skills/speckit-flow-wiki-*/SKILL.md` files. Confirm the registry has ID `flow-wiki`, exactly five new command names, and no `wiki` extension, old command aliases, or old skill IDs. Generated human-readable skill names need only identify the wiki action.

Confirm the manifest declares Spec Kit 1.0.1 as its minimum. An older CLI, if available as a disposable fixture, must reject installation without changing the consumer's wiki state.

Check the `.extensionignore` payload: no source `.git/`, historical `specs/`, development files, or unrelated repository state should appear in the installed extension snapshot.

## Migrated Consumer

Create an old-extension source from a local historical tag, then install it into a separate fixture:

```bash
mkdir -p "$FIXTURE_ROOT/old-source"
git archive v1.1.2 | tar -x -C "$FIXTURE_ROOT/old-source"
specify init "$FIXTURE_ROOT/migrated" --non-interactive --ignore-agent-tools --integration codex --integration-options="--skills"
(cd "$FIXTURE_ROOT/migrated" && specify extension add --dev "$FIXTURE_ROOT/old-source")
cp -R "$SOURCE_REPO/wiki" "$FIXTURE_ROOT/migrated/wiki"
```

Set one non-default old configuration value and record it with any active `SPECKIT_WIKI_*` override. Record the old hook and skill IDs. Install the new extension into this fixture, transfer the recorded values to `flow-wiki-config.yml` and `SPECKIT_FLOW_WIKI_*`, update active references, inspect the new registry and generated skills, and remove the old extension. Do not invoke either extension during temporary coexistence.

```bash
(cd "$FIXTURE_ROOT/migrated" && specify extension add --dev "$SOURCE_REPO")
(cd "$FIXTURE_ROOT/migrated" && specify extension remove wiki --keep-config --force)
diff -qr "$SOURCE_REPO/wiki" "$FIXTURE_ROOT/migrated/wiki"
```

The last command must report no differences. Verify the new effective configuration value and that old file and environment names are no longer read. Confirm one active ingest hook at each configured event and no old command or skill aliases. Preserve the old configuration copy as rollback evidence until checks pass.

## Action and Failure Checks

In disposable Codex consumers, exercise each new action through its generated skill or command. Preserve the existing command fixtures and compare outcomes:

| Scenario | Expected outcome |
|---|---|
| Init with no schema | Creates only the permitted foundation artifacts |
| Init with existing schema | Preserves existing wiki content except an explicitly requested scope append |
| Ingest one valid source | Records source and cited page changes coherently within configured bounds |
| Query and status | Return evidence-bounded answers or snapshots with zero writes |
| Lint | Writes only its derived report and permitted mechanical fixes |
| Malformed config or escaping path | Rejects before unauthorized read or write |
| Embedded instructions in source or wiki text | Treated as data; no expanded authority |
| Limit overflow or simulated failed write | Reports the bounded or failed outcome and preserves pre-existing state |

Check old command and skill names are absent from the new extension's installed registry and generated skills. Do not treat an unknown old name as a successful migration.

## Static Review

```bash
git diff --check
rg -n 'speckit\.wiki\.|speckit-wiki-|SPECKIT_WIKI_|(^|/)wiki-config\.yml|spec-kit-wiki' extension.yml commands config-template.yml README.md docs .specify/extensions.yml wiki/SCHEMA.md wiki/INDEX.md wiki
```

The active-source scan should have no unexplained old-identity references. Review matches in historical specs, completed queue records, cited wiki pages, or older changelog entries individually rather than replacing them automatically. In `wiki/SCHEMA.md` and `wiki/INDEX.md`, only current operational command examples are updated; keep the semantic page claims and their source history intact. Confirm the current extension behavior still matches the existing constitutional read, write, evidence, and limit rules.

## Published Repository Check

After the reviewed local extension passes and the published repository is renamed, verify the new GitHub location and update the local remote:

```bash
git remote set-url origin git@github.com:pegagio/spec-kit-flow-wiki.git
git remote -v
git ls-remote origin HEAD
```

On 2026-09-23, GitHub repository metadata confirmed `pegagio/spec-kit-flow-wiki` exists, is public, and is not archived. The local `origin` now uses `git@github.com:pegagio/spec-kit-flow-wiki.git`. A local `git ls-remote origin HEAD` attempt could not resolve `github.com` in this environment, so Git transport reachability remains unverified here. Verify the README installation link against the new published location before release. The rename and release are separate external actions; do not report publication as complete before they occur.

## Validation Record

The local validation used Specify CLI 1.0.1 and disposable consumers. The clean consumer registered exactly five `speckit.flow-wiki.*` commands, generated five `speckit-flow-wiki-*` skills with action-identifying names, installed the new config file and two hooks, and contained only the allowlisted extension payload plus its generated config.

The migrated consumer started from tag `v1.1.2`. It transferred non-default config values, verified the prompt's new environment override precedence, removed the old registration and skills, left one new hook per event, and preserved every wiki file byte-for-byte. The current checkout was migrated through Specify's remove/add workflow with the same result; its old config values match the new config values.

The action fixture followed the generated prompt contracts for initialization and idempotency, cited ingestion with embedded untrusted instructions, read-only query and status, lint report-only behavior, page-cap overflow deferral, path escape rejection, and a failed write. The fixture preserved existing wiki state on read-only, invalid-path, and failed-write paths. The CLI failure fixture blocked one generated skill path; Specify reported a nonzero installation error, left only the old `wiki` registration active alongside an unregistered partial payload, and left the wiki tree unchanged.

Because the extension's actions are LLM prompts rather than executable application code, the action fixture is a prompt-contract walkthrough; it is not a separate deterministic runtime test. The old `SPECKIT_WIKI_*` prefix is absent from the active command prompts, which specify `SPECKIT_FLOW_WIKI_*` after the config file and before invocation overrides.
