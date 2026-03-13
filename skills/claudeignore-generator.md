---
name: claudeignore-generator
description: Detects the project's framework and tooling, then generates a .claudeignore file with framework-specific patterns. Presents the result for user review before writing.
---

# .claudeignore Generator

You generate `.claudeignore` files tailored to the project's stack. You detect the framework, identify tooling, and produce a file that excludes everything Claude shouldn't read.

## Process

### 1. Detect framework

Read the project root and identify the primary framework by checking for:

| File | Framework |
|---|---|
| `next.config.*` | Next.js |
| `nuxt.config.*` | Nuxt |
| `svelte.config.*` | SvelteKit |
| `angular.json` | Angular |
| `pyproject.toml` / `setup.py` | Python |
| `Cargo.toml` | Rust |
| `go.mod` | Go |
| `build.gradle` / `pom.xml` | Java/Kotlin |
| `Gemfile` | Ruby |
| `Package.swift` | Swift |
| `mix.exs` | Elixir |

If multiple frameworks are present (e.g., monorepo), note all of them.

### 2. Detect tooling

Check for the presence of:
- **Testing**: jest.config, vitest.config, pytest.ini, .rspec
- **Linting**: .eslintrc, .prettierrc, biome.json, .flake8, .rubocop.yml
- **Containers**: Dockerfile, docker-compose.yml
- **CI**: .github/workflows, .gitlab-ci.yml, Jenkinsfile
- **Bundling**: webpack.config, vite.config, rollup.config, esbuild
- **Docs**: docs/, .storybook/

### 3. Generate .claudeignore

Build the file with these sections in order:

```
# Build output
{framework-specific build dirs}

# Dependencies
{package manager dirs}

# Generated files
{lock files, type declarations, sourcemaps, compiled assets}

# Test artifacts
{coverage dirs, snapshot files if large}

# IDE and OS
.idea/
.vscode/
*.DS_Store

# Environment
.env*
```

Include only patterns relevant to the detected stack. Don't add patterns for tools that aren't present.

### 4. Present for review

Show the generated file to the user with a brief explanation of what each section excludes. Ask for confirmation before writing.

### 5. Write the file

After user approval, write `.claudeignore` to the project root. If one already exists, show a diff of what would change and ask before overwriting.

## Framework pattern reference

**Next.js:** `.next/`, `out/`, `node_modules/`, `*.tsbuildinfo`, `storybook-static/`

**Python:** `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `build/`, `*.egg-info/`, `.tox/`, `.mypy_cache/`, `.pytest_cache/`, `htmlcov/`

**Rust:** `target/`, `Cargo.lock` (for libraries)

**Go:** `vendor/` (if present), `bin/`

**Java/Kotlin:** `build/`, `.gradle/`, `target/`, `*.class`

**Ruby:** `vendor/bundle/`, `.bundle/`, `coverage/`, `tmp/`

See `examples/claudeignore/` for complete example files.
