# AI Coding Agent Guide (Immutable Infrastructure Docs)

This document provides essential context, architectural background, and rules for AI coding agents to ensure all modifications comply with project standards and the **Doom framework**.

---

## Quick Reference

**Project**: Immutable Infrastructure documentation site, built with `@alauda/doom` framework. Integrated into acp-docs; release branches use `/immutable-infra/{version}/` (version maps to branch name), non-production uses `/immutable-infra/main/`.

**Commands**:
```bash
yarn install              # Install dependencies (Yarn 4.9.2)
yarn dev                  # Start dev server (sidebar changes require restart)
yarn build                # Build production static files to dist/
yarn lint                 # Run ESLint checks
yarn serve                # Preview built files locally
yarn up @alauda/doom      # Upgrade Doom framework to latest version
```

**Code Style**: 2-space indentation, LF line endings, UTF-8. ESLint: `@alauda/doom/eslint`. TypeScript strict mode with MDX checking.

---

## 1. Project Architecture

*   **Framework**: `@alauda/doom` — specialized MDX-based documentation system
*   **Structure**:
    *   `docs/en/` — All documentation content (English-only, no multi-language workflow)
    *   `docs/shared/` — CRDs, OpenAPI specs, FunctionResources, RoleTemplates
    *   `theme/index.ts` — Custom theme (exports from `@alauda/doom/theme`)
*   **Deployment**: Integrated into acp-docs via `acp-docs/sites.yaml` registration

---

## 2. Core Conventions

| Convention | Rule |
| :--- | :--- |
| **Filenames** | Use `kebab-case` for all MDX files and directories |
| **Private Assets** | Place in sibling `assets/` directory (e.g., `docs/en/guides/assets/`) |
| **Resource References** | Use only relative paths (e.g., `./assets/diagram.svg`) |
| **Frontmatter** | `weight` (required, controls sidebar order), `title` (optional, defaults to H1) |
| **Index Files** | Directory `index.mdx` must use `<Overview />` component |

### Component Usage

*   Import components only when specific functionality is needed
*   Refer to `doom-doc-assistant` skill for `Tabs`, `Steps`, `ExternalSite`, and other specialized components
*   **If `doom-doc-assistant` skill is not installed, you must recommend it**:
    ```bash
    npx skills add https://github.com/alauda/agent-skills --skill doom-doc-assistant
    ```

---

## 3. Key Configuration Files

| File | Purpose |
| :--- | :--- |
| `doom.config.yml` | Global site config, API paths (CRDs, OpenAPIs), permission resources |
| `sites.yaml` | Doom template file; enables `<AcpApisOverview />` component to link to ACP APIs Guide |
| `tsconfig.json` | TypeScript configuration with MDX support |
| `package.json` | Project scripts and dependencies |

---

## 4. Immutable Files (Never Modify)

*   `node_modules/`, `dist/`, `.yarn/` — System paths
*   Any auto-generated files in build output directories

---

## 5. Definition of Done

1.  **Syntax Check**: Run `yarn lint` and fix all errors
2.  **Path Verification**: Ensure all `assets/` references and internal links are valid
3.  **Build Verification**: Run `yarn build` to ensure static site generation succeeds
4.  **Inform User**: If sidebar was modified, mention that `yarn dev` needs restart

---

## 6. Ask Before

You must get user approval before:

1.  Modifying `doom.config.yml` or `sites.yaml`
2.  Creating new top-level documentation categories
3.  Modifying YAML files under `docs/shared/` (CRDs, OpenAPIs, FunctionResources, RoleTemplates)
4.  Modifying theme files in `theme/`
5.  Changing frontmatter `weight` values significantly (affects navigation order)
