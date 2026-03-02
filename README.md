# Tailwind CSS v4.2 Skills for LLMs

A comprehensive, AI-optimized reference guide for **Tailwind CSS v4.2**. This repository contains a `SKILL.md` file designed to help LLMs (Claude, Gemini, ChatGPT, GitHub Copilot) understand and apply the latest Tailwind CSS features, best practices, and migration patterns.

## 🚀 Why This Exists

LLMs are often trained on older data. When working with **Tailwind CSS v4.x**, they frequently suggest deprecated v3 patterns (like `tailwind.config.js`, `@tailwind base`, or `bg-gradient-to-r`). This skill file provides the necessary context for AI models to:

- Use the new **CSS-first configuration** (`@theme`) instead of JavaScript config files.
- Leverage the **OKLCH** color space for premium, vibrant designs.
- Apply **Logical Properties** for better internationalization support.
- Use built-in **Container Queries**, **3D Transforms**, **Text Shadows**, and more — without plugins.
- Avoid common **v3 → v4 migration pitfalls**.

## 📂 What's Inside `SKILL.md`

The file is organized into **19 sections** covering everything an LLM needs:

| Section | Topic                                                                                                                  |
| ------- | ---------------------------------------------------------------------------------------------------------------------- |
| §1      | **Installation** — Vite, PostCSS, Webpack (v4.2), CLI, CDN (npm & pnpm)                                                |
| §2      | **CSS-First Configuration** — `@theme` tokens, namespaces, extending/overriding                                        |
| §3      | **Dark Mode** — Auto, class-based, data-attribute, 3-way toggle with JS                                                |
| §4      | **Responsive Design** — Breakpoints, `max-*` ranges, Container Queries                                                 |
| §5      | **Gradients** — `bg-linear-*` (renamed), radial, conic, interpolation modifiers                                        |
| §6      | **Colors** — OKLCH color space, all palettes (incl. v4.2: Mauve, Olive, Mist, Taupe)                                   |
| §7      | **Dynamic Utility Values** — Arbitrary values, properties, and variants                                                |
| §8      | **Shadows** — Stackable shadows, inset shadows, rings                                                                  |
| §9      | **3D Transforms** — Perspective, rotate-x/y/z, translate-z, backface                                                   |
| §10     | **Animations & Transitions** — Custom `@keyframes` in `@theme`, `starting:` variant                                    |
| §11     | **States & Variants** — Pseudo-classes, group/peer, `in-*`, descendant `**:`                                           |
| §12     | **Text Shadows & Masks** — `text-shadow-*`, `mask-*`, colored drop shadows (v4.1)                                      |
| §13     | **Logical Properties** — `pbs-*`, `mbe-*`, `inline-s-*`, `font-features-*` (v4.2)                                      |
| §14     | **Custom Utilities & Variants** — `@utility`, `@custom-variant`, `@layer`, `@variant`                                  |
| §15     | **Source Detection** — `@source`, `@source not`, `@source inline()`                                                    |
| §16     | **Official Plugins** — `@plugin` for Typography and Forms                                                              |
| §17     | **Common Utility Quick Reference** — Layout, spacing, sizing, typography, borders, effects, positioning, interactivity |
| §18     | **Migration Pitfalls (v3 → v4)** — Critical "DO NOT / USE THIS" table                                                  |
| §19     | **Design Best Practices** — Mobile-first, OKLCH gradients, container queries, dark mode                                |

## 🛠 How to Use

### VS Code + GitHub Copilot

Copy `SKILL.md` content into your project's Copilot instructions file:

```
your-project/
├── .github/
│   └── copilot-instructions.md   ← Paste SKILL.md content here
```

Alternatively, create path-specific instructions:

```
your-project/
├── .github/
│   └── instructions/
│       └── tailwind.instructions.md   ← Paste SKILL.md content here
```

### Cursor

Add the file as a project rule or doc:

```
your-project/
├── .cursor/
│   └── rules/
│       └── tailwindcss.mdc   ← Paste SKILL.md content here
```

Or go to **Cursor Settings → Docs → Add Doc** and point to this repository's `SKILL.md`.

### Windsurf

Place instructions in the Windsurf rules file:

```
your-project/
├── .windsurfrules   ← Paste SKILL.md content here
```

### Antigravity (Google)

Place the `SKILL.md` inside a skills directory in your project:

```
your-project/
├── .agents/
│   └── skills/
│       └── tailwindcss/
│           └── SKILL.md   ← Place here
```

Antigravity automatically discovers skills from `.agents/skills/` directories.

### WebStorm / JetBrains IDEs

Go to **Settings → Tools → AI Assistant → Project Settings → Path to guideline** and point it to `SKILL.md` in your project root:

```
your-project/
├── SKILL.md   ← Place here, then reference in settings
```

### Claude Code

Place the file as a project-level instruction:

```
your-project/
├── CLAUDE.md   ← Paste SKILL.md content here
```

Or inside the rules directory:

```
your-project/
├── .claude/
│   └── rules/
│       └── tailwindcss.md   ← Paste SKILL.md content here
```

### Gemini / ChatGPT / Other Web-based LLMs

Upload `SKILL.md` as a file attachment at the start of your session, or paste its content with:

> "Use the following as your Tailwind CSS reference. Follow v4.2 patterns exclusively."

## 🆕 v4.2 Highlights (February 2026)

- **First-party Webpack Plugin** (`@tailwindcss/webpack`)
- **New Color Palettes**: Mauve, Olive, Mist, Taupe
- **Enhanced Logical Properties**: `pbs-*`, `mbe-*`, `inline-s-*`, `inline-e-*`
- **Font Features**: `font-features-*` for OpenType control
- **Inline & Block Sizing**: `inline-size-*`, `block-size-*`

## 📄 License

MIT

## 🙌 Contributing

Feel free to open an issue or PR if you find new v4.x patterns that should be included!
