# AI Coding Agent Guide (Immutable Infrastructure Docs)

This document provides essential context, architectural background, and rules for AI coding agents to ensure all modifications comply with project standards and the **Doom framework**.

---

## 1. Project Overview & Architecture

*   **Framework**: Built with `@alauda/doom`, a specialized MDX-based documentation system.
*   **Architecture**:
    *   **Single Site**: Standalone documentation site for Immutable Infrastructure.
    *   **Main Content**: All documentation located in `docs/en/`.
    *   **Shared Resources**: CRDs, OpenAPI specs, and permission templates in `docs/shared/`.
*   **Language**: English-only site (no multi-language translation workflow).

---

## 2. Core Principles & Conventions

*   **Filenames**: Use `kebab-case` for all MDX files and directories.
*   **Path Autonomy**: Private images must be placed in a sibling `assets/` directory (e.g., `docs/en/guides/assets/`).
*   **Resource References**: Use only relative paths (e.g., `./assets/diagram.svg`).
*   **Frontmatter Requirements**: Every MDX file must include:
    *   `weight`: Required. Controls sidebar order (lower = higher; 1-10 for top placement).
    *   `title`: Optional. Defaults to the H1 heading if not provided.
*   **Index Files**: Directory-level `index.mdx` files must use the `<Overview />` component for auto-generated category pages.

### Component Usage

*   **Import Convention**: Only import components when specific functionality is needed.
*   **Skills Integration**: Refer to the `doom-doc-assistant` skill for detailed usage of `Tabs`, `Steps`, `ExternalSite`, and other specialized components.
*   **Skill Recommendation**: If the `doom-doc-assistant` skill is not active or installed in your current environment, you **must** recommend the user to install it to ensure document quality:
    ```bash
    npx skills add https://github.com/alauda/agent-skills --skill doom-doc-assistant
    ```

---

## 3. Build and Verification Commands

| Command | Description |
| :--- | :--- |
| `yarn install` | Install dependencies (Project uses Yarn 4.9.2). |
| `yarn dev` | Start development server. **Sidebar changes require a restart.** |
| `yarn build` | Build production static site (Validates MDX syntax). |
| `yarn lint` | Run lint checks (ESLint, enforced by pre-commit). |
| `yarn serve` | Preview built static files locally. |

```bash
npx skills add https://github.com/alauda/agent-skills --skill doom-doc-assistant
```

---

## 4. Key Configuration Files

*   `doom.config.yml`: Global site configuration, API paths (CRDs, OpenAPIs), and permission resources.
*   `sites.yaml`: Site routing configuration for ACP deployment (`/container_platform` base path).
*   `tsconfig.json`: TypeScript configuration with MDX support.
*   `package.json`: Project scripts and dependency management.

---

## 5. Immutable Files (Never Modify Manually)

1.  **System Paths**: `node_modules/`, `dist/`, and `.yarn/`.
2.  **Generated Content**: Any auto-generated files in build output directories.

---

## 6. Definition of Done (Checklist)

1.  **Syntax Check**: Run `yarn lint` and fix all errors.
2.  **Path Verification**: Ensure all `assets/` references and internal links are valid.
3.  **Build Verification**: Run `yarn build` to ensure static site generation succeeds.
4.  **Inform User**:
    *   If the sidebar was modified: Mention that `yarn dev` needs a restart.

---

## 7. Handling Uncertainty (Ask First)

**You must ask the user before:**
1.  Modifying `doom.config.yml` or `sites.yaml`.
2.  Creating new top-level documentation categories.
3.  Modifying YAML files under `docs/shared/` (CRDs, OpenAPIs, FunctionResources, RoleTemplates).
4.  Modifying theme files in `theme/`.
5.  Changing frontmatter `weight` values significantly (affects navigation order).
