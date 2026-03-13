# Frontend Rules

Append these to universal.md for React, Next.js, Vue, Svelte, or similar frontend projects.

## .claudeignore

Add to your project's `.claudeignore`:

```
node_modules/
.next/
dist/
build/
out/
storybook-static/
coverage/
*.tsbuildinfo
```

## Components

- Don't read component library source (MUI, shadcn, etc.) unless you're modifying the library itself.
- Follow the existing naming convention — check if the project uses PascalCase dirs, kebab-case files, or barrel exports before creating components.
- Check for existing similar components before creating new ones.
- Read the component you're modifying and its direct parent before making changes.

## Styling

- Detect the CSS approach first: Tailwind, CSS modules, styled-components, or plain CSS. Don't mix approaches.
- Don't add inline styles to components that use a design system.
- Check for existing design tokens/theme variables before hardcoding colors, spacing, or typography values.

## Build & test

- Scope type-checks to the files you changed: `tsc --noEmit` on the whole project is expensive.
- Don't read build output, webpack/vite configs, or bundler logs unless debugging a build issue.
- Run the project's existing test command scoped to the module you changed, not the full suite.
- Check for existing test utilities and render helpers before writing custom ones.
