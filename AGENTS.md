# AI Agent Guide — Immutable Infrastructure Documentation

Context and rules for AI agents working in this repository. Everything here was
verified against the current tree; when this file and the code disagree, the
code wins — fix this file in the same PR.

---

## 1. What this repository is

An English-only product documentation site for **Immutable Infrastructure**
(Cluster API based Kubernetes cluster lifecycle management), built with the
[`@alauda/doom`](https://github.com/alauda/doom) framework and published as part
of the ACP documentation site.

- Content lives in `docs/en/` only. There is no multi-language workflow in this
  repository — do not add `docs/zh/`.
- `docs/shared/` holds the sources that the API reference pages are generated
  from: `crds/`, `openapis/`, `functionresources/`, `roletemplates/`. The paths
  are wired in `doom.config.yml`.
- `theme/` re-exports the doom theme.

## 2. Branches

| Branch | Line |
| :--- | :--- |
| `master` | main line |
| `release-1.0` | the v1.0 release line |

Both receive changes through pull requests.

**Any documentation change that is true for v1.0 must also land on
`release-1.0`.** Backports in this repository are made as fresh commits, not as
cherry-picks, so the two branches never share commit hashes. Never judge parity
by comparing commit logs — the branches routinely show a hundred-plus divergent
commits while holding identical content.

The authoritative parity check is the content diff:

```bash
git diff origin/release-1.0 origin/master -- docs/   # must be empty
```

Run it in both directions of reasoning: the `+` side shows what `master` has and
`release-1.0` is missing; the `-` side shows content that exists only on
`release-1.0`, which is either a missing forward-port or a deliberate
release-only difference. Two backports were lost before this check was adopted
(PRs #113 and the HCS half of #144, both recovered in PR #159).

## 3. Commands

```bash
yarn install    # Yarn 4 (Berry), nodeLinker: node-modules
yarn lint       # doom lint — the gate that must pass before you push
yarn build      # production build to dist/
yarn serve      # preview a completed build
yarn dev        # dev server; restart it after changing sidebar order
```

`yarn lint` runs **doom lint**, not plain ESLint, even though
`eslint.config.mjs` re-exports `@alauda/doom/eslint`. Spell checking uses
`@alauda/doom/cspell` through `cspell.config.mjs`.

`node_modules` is roughly 550 MB. On a small volume, install into a location
with real free space rather than a size-constrained workspace mount.

## 4. Authoring conventions

| Convention | Rule |
| :--- | :--- |
| Filenames | `kebab-case` for new pages. Some existing pages use snake_case (`release-notes/huawei_dcs.mdx`, `how-to/dcs_persistent_disk_upgrade.mdx`) — leave them alone, the filename is the published URL |
| Frontmatter `weight` | Required on every page; controls sidebar order |
| Frontmatter `title` | Optional, defaults to the H1 |
| Frontmatter `queries` | Repo convention (used on 105 of 136 pages): natural-language questions the page answers. Not read by the doom build |
| Directory `index.mdx` | Must render `<Overview overviewHeaders={[]} />` |
| Assets | Sibling `assets/` directory, referenced by relative path (`./assets/diagram.svg`) |
| Product name | `<Term name="product" />`, never a hardcoded product string |

### MDX rules that lint enforces

- **Explicit anchors.** Every internal link `[text](#some-id)` needs the target
  heading to declare the id: `## Some Heading \{#some-id}`. Doom does not derive
  a lint-acceptable id from heading text. The tree currently carries 174
  explicit anchors — follow the same pattern.
- **No HTML comments.** `<!-- ... -->` is invalid in MDX. Use `{/* ... */}` or
  delete the comment. The tree currently has zero HTML comments.
- **Code blocks inside lists** must stay indented far enough to remain part of
  the list item, otherwise the block splits the list and lint fails.

### Cross-site links

Links to another documentation site go through `ExternalSiteLink`, with the site
name taken from `sites.yaml`:

```mdx
<ExternalSiteLink name="acp" href="/install/index.html" children="the standard installation path" />
```

`sites.yaml` currently registers `acp` at base `/container_platform`. The `href`
is relative to that base and does **not** repeat the base prefix.

## 5. The `llms.txt` trio

Three repo-root files form a source-level index for agents that read a bare
clone: `llms.txt` (the map), `llmstxt-state.json` (per-file `sha256`, `size` and
description, so only changed files need re-describing) and `llmstxt-config.yaml`
(include/exclude globs, grouping, 150-word description cap).

Do not confuse them with `dist/llms.txt` and `dist/llms-full.txt`, which doom
emits on every build from the route table. Those are build output; the trio is
checked-in source.

**When you add, remove, rename or substantially rewrite a page under
`docs/en/`, update `llms.txt` and `llmstxt-state.json` in the same PR.** A
stale index is worse than no index: an agent reads a summary that no longer
matches the page, or never learns the page exists. Keep the copies on `master`
and `release-1.0` identical for as long as the documentation content is
identical.

**A page's description must be byte-identical in `llms.txt` and in
`llmstxt-state.json`.** Editing one without the other is the failure mode this
rule exists to prevent, and it does not heal on its own: the generator decides
whether to re-describe a page from the stored `sha256`, so once that hash is
current the two descriptions stay divergent through every later run. Eleven
entries had drifted this way before the check below was adopted. Verify with:

```bash
python3 - <<'EOF'
import json, re, pathlib
st = json.loads(pathlib.Path('llmstxt-state.json').read_text())['files']
ld = dict(re.findall(r'^- \[([^\]]+)\]\(\1\): (.+)$',
                     pathlib.Path('llms.txt').read_text(), re.M))
bad = [p for p, e in st.items() if p in ld and e['description'] != ld[p]]
print('description mismatches:', bad or 'none')
EOF
```

## 6. CI

Continuous integration is **Tekton Pipelines-as-Code** (`.tekton/`), not GitHub
Actions — this repository has no `.github/` directory.

- `doc-pr-build.yaml` runs on pull requests targeting `main`, `master` or
  `release-*`; re-trigger it by commenting `/doc-pr-build`.
- `doc-build.yaml` runs on pushes to those branches; re-trigger with
  `/doc-build`.

CI is not a substitute for running `yarn lint` locally before pushing.

## 7. Definition of done

1. `yarn lint` reports 0 errors and 0 warnings.
2. `yarn build` succeeds when you changed links, anchors, or page structure.
3. The backport decision is made and stated — either the change is on both
   branches, or the PR says why it is `master`-only.
4. `llms.txt` and `llmstxt-state.json` reflect the pages you touched.
5. If you changed sidebar order, say so — `yarn dev` needs a restart to pick it up.

## 8. Get approval before

- Editing `doom.config.yml`, `sites.yaml`, or anything in `theme/`.
- Editing the YAML sources under `docs/shared/` — the API reference pages are
  generated from them.
- Creating a new top-level documentation category, or reshuffling `weight`
  values across a section.
- Deleting or renaming a published page. The filename is the URL; a rename
  breaks inbound links and must be planned with a redirect.
