# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

Ariatype is a TypeScript library that provides comprehensive type definitions for WAI-ARIA accessibility attributes and roles. The project is structured as a monorepo with multiple packages, each focusing on specific aspects of ARIA specifications (attributes, roles, and their subcategories).

## Development Commands

### Core Commands

```bash
# Install dependencies
pnpm install

# Build all packages
pnpm run build

# Build with Turbo (recommended for development)
pnpm run turbo:build

# Run development mode (watch mode for all packages)
pnpm run dev
pnpm run turbo:dev

# Type checking
pnpm run typecheck
pnpm run turbo:typecheck
```

### Linting and Formatting

```bash
# Format and lint all files
pnpm run lint

# Check formatting and linting without fixing
pnpm run lint:check:all

# Format all files with Prettier
pnpm run prettier:write:all

# Check Prettier formatting
pnpm run prettier:check:all

# Run ESLint on all files
pnpm run eslint:all
```

### Individual Package Development

```bash
# Work on a specific package (from package directory)
cd @ariatype/[package-name]
pnpm run build
pnpm run dev      # Watch mode
pnpm run typecheck
```

### Testing

Note: This repository currently does not have tests configured. The `pnpm run test` and `pnpm run turbo:test` commands exist but no test framework is set up.

### Publishing

```bash
# Version packages using changesets
pnpm run changeset:version

# Publish packages (CI only)
pnpm run ci:publish
```

## Repository Architecture

### Monorepo Structure

This is a pnpm workspace monorepo managed by Turbo. The repository contains:

- **Root Package**: Workspace configuration and shared tooling
- **@ariatype/\*** packages: Individual packages under the `@ariatype/` directory

### Package Categories

**Core Package:**

- `@ariatype/ariatype`: Main bundle that exports all other packages

**ARIA Attributes Packages:**

- `@ariatype/aria-attributes`: All ARIA attributes (aggregates below packages)
- `@ariatype/aria-attributes-drag-and-drop`: Drag and drop specific attributes
- `@ariatype/aria-attributes-global`: Global ARIA attributes
- `@ariatype/aria-attributes-live-region`: Live region attributes
- `@ariatype/aria-attributes-relationship`: Relationship attributes
- `@ariatype/aria-attributes-widget`: Widget-specific attributes

**ARIA Roles Packages:**

- `@ariatype/aria-roles`: All ARIA roles (aggregates below packages)
- `@ariatype/aria-roles-composite`: Composite roles
- `@ariatype/aria-roles-document-structure`: Document structure roles
- `@ariatype/aria-roles-generic`: Generic roles
- `@ariatype/aria-roles-landmark`: Landmark roles
- `@ariatype/aria-roles-live-region`: Live region roles
- `@ariatype/aria-roles-widget`: Widget roles
- `@ariatype/aria-roles-window`: Window roles

**Utility Packages:**

- `@ariatype/partially-required-props`: Utility for making specific props required

### Code Organization Pattern

Each package follows a consistent structure:

```
@ariatype/[package-name]/
├── src/
│   ├── lib/           # Runtime values/constants
│   ├── types/         # TypeScript type definitions
│   └── index.ts       # Main export file
├── package.json
├── tsconfig.json
├── tsup.config.ts     # Build configuration
└── README.md
```

### Aggregation Pattern

The main packages (like `@ariatype/aria-attributes`) aggregate their sub-packages by importing and combining arrays using `...new Set([...arrays])` to deduplicate values.

### Build System

- **Bundler**: tsup (TypeScript bundler)
- **Task Runner**: Turbo for orchestrating builds across packages
- **Package Manager**: pnpm with workspaces
- **Output**: ESM format with TypeScript declarations, minified and tree-shakeable

### Development Tools

- **ESLint**: Code linting with TypeScript support
- **Prettier**: Code formatting with import sorting
- **Husky**: Git hooks for pre-commit linting
- **lint-staged**: Run linters on staged files only
- **Changesets**: Version management and changelog generation
- **Commitlint**: Enforce conventional commit messages

### Type Safety

All packages export both:

- Runtime constants (arrays of strings)
- TypeScript types (union types derived from constants)
- Type utilities (like `PartiallyRequiredAriaTypes`)

### Dependencies

Each package uses `workspace:*` dependencies for internal packages, ensuring they always use the latest workspace version during development.
