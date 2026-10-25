# plugins — Rez's fork of cursor/plugins, for pstack

Rez runs pstack in Claude Code from this fork (`medAzizRezgui/plugins`), installed as
`pstack@cursor-plugins`. Only `pstack/` is his to change; the rest of the repo is upstream's and
is left alone so syncs stay clean. Upstream is `cursor/plugins`.

Issue tracker: GitHub Issues, as `~/projects/work/engineering/guides/issue-tracker-github.md` says; side-finds go to `docs/inbox.md` first.

## Changing pstack

1. Edit the sources in `pstack/` (`skills/`, `agents/`, `docs/`, `README.md`, `plugin.json`).
   `harness/pstack/claude-code/` is generated: never edit it by hand.
2. `node scripts/generate-pstack-harness-trees.mjs`, then commit each change with its
   regenerated tree, so every commit passes `--check` on its own.
3. Before a push, the checks CI runs: `npm install --no-save ajv ajv-formats` (there is no
   `package.json`), then `node scripts/validate-plugins.mjs`,
   `node scripts/generate-pstack-harness-trees.mjs --check`, `node scripts/check-pstack-docs.mjs`
   and `node --test pstack/skills/*/scripts/test/*.test.mjs`.
4. Open the PR with `--repo medAzizRezgui/plugins --base main`: `gh` otherwise targets upstream.
   The merge is Rez's.
5. After the merge, run `claude plugin update pstack@cursor-plugins` (with `CLAUDE_CONFIG_DIR`
   set for a profile other than the current one), then grep the new lines in the `installPath`
   that `installed_plugins.json` gives `pstack@cursor-plugins`.

`pstack/plugin.json` carries no `version` on purpose: Claude Code keys an installed plugin by it,
so a fixed version freezes installs. An upstream sync that brings the line back conflicts on it;
keep it deleted.
