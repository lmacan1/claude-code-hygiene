# Monorepo Rules

Append these to universal.md + large-repo.md for monorepo projects (npm/yarn/pnpm workspaces, Turborepo, Nx, Lerna, Bazel).

## Workspace awareness

- Identify the workspace root and the target package before any operation.
- Read the workspace config (`pnpm-workspace.yaml`, `package.json` workspaces field, `nx.json`) to understand the package layout.
- Don't assume package locations — check the workspace config.

## Scoping

- Install dependencies at the package level, not the root, unless it's a shared dev dependency.
- Scope linters and type-checks to the affected package. Running them at the root checks everything.
- Run tests only for the package you changed. Use the workspace tool's filter flag (`--filter`, `--scope`, `nx affected`).
- Check the root `package.json` for workspace-level scripts before inventing commands.

## Cross-package changes

- Before changing a shared package's exports, trace its consumers: `grep -r "from '@scope/package-name'" packages/`.
- If you change a shared package's interface, update all consumers in the same commit.
- Don't modify multiple packages for unrelated changes in a single session — context gets expensive.
- Check for package-level `tsconfig.json` or `jest.config` that may override root settings.
