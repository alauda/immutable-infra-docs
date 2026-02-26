# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

**You must read AGENTS.md and treat it as an extension of this file.**

---

## Quick Reference

### Project
Immutable Infrastructure documentation site, built with `@alauda/doom` framework. Deployed to ACP at `/container_platform` base path (v4.1).

### Commands
```bash
yarn install    # Install dependencies (Yarn 4.9.2)
yarn dev        # Start dev server (sidebar changes require restart)
yarn build      # Build production static files to dist/
yarn lint       # Run ESLint checks
yarn serve      # Preview built files locally
```

### Structure
```
docs/
├── en/              # English documentation (all content here)
│   ├── overview/    # Overview, architecture, features, release notes
│   ├── how-to/      # How-to guides
│   ├── apis/        # API documentation
│   └── *.mdx        # Top-level pages
├── shared/          # Shared Kubernetes resources
│   ├── crds/        # Custom Resource Definitions
│   ├── openapis/    # OpenAPI specs
│   └── roletemplates/ # RBAC templates
└── public/          # Static assets
theme/index.ts       # Custom theme (exports @alauda/doom/theme)
```

### Code Style
- 2-space indentation, LF line endings, UTF-8
- ESLint: `@alauda/doom/eslint`
- TypeScript strict mode with MDX checking
