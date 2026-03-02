# Tailwind CSS v4.2 Skills for LLMs

A comprehensive, AI-optimized reference guide for **Tailwind CSS v4.2**. This repository contains a `SKILL.md` file designed to help LLMs (Claude, Gemini, ChatGPT, GitHub Copilot) understand and apply the latest Tailwind CSS features, best practices, and migration patterns.

## 🚀 Why this exists?

LLMs are often trained on older data. When working with **Tailwind CSS v4.x**, they might suggest deprecated v3 patterns (like `tailwind.config.js` or `bg-gradient-to-r`). This "Skill" provides the necessary context for AI models to:

- Use the new **CSS-first configuration** (`@theme`).
- Leverages the **OKLCH** color space for premium designs.
- Apply **Logical Properties** for better i18n.
- Use built-in **Container Queries**, **3D Transforms**, and **Text Shadows**.

## 📂 Contents

- **[SKILL.md](SKILL.md)**: The core reference file. Includes:
  - Installation guides (Vite, PostCSS, Webpack, CLI).
  - Deep dive into `@theme` tokens.
  - Dark mode and Responsive design strategies.
  - v4.1/v4.2 new utilities (Text shadows, Masks, Logical properties).
  - **Migration Pitfalls**: A critical table for converting v3 projects to v4.
  - AI-specific design best practices.

## 🛠 How to use

### Option 1: Context Injection (Claude/Gemini/ChatGPT)

Copy the content of `SKILL.md` and paste it at the beginning of your chat session, or upload it as a project knowledge file.

> "Use the attached SKILL.md for all Tailwind CSS related suggestions. Follow v4.2 patterns exclusively."

### Option 2: GitHub Copilot / Cursor

Add this repository (or just the `SKILL.md` file) to your project's `.github/copilot-instructions.md` or as a "Doc" in Cursor to ensure the AI always uses the correct syntax.

## 🆕 New in v4.2 Focus

This guide specifically highlights the latest February 2026 releases:

- **First-party Webpack Plugin** (`@tailwindcss/webpack`).
- **New Color Palettes**: Mauve, Olive, Mist, Taupe.
- **Enhanced Logical Properties**: `pbs-*`, `mbe-*`, `inline-s-*`, etc.
- **Font Features**: `font-features-*` utilities for OpenType control.

---

## 📄 License

MIT

## 🙌 Contributing

Feel free to open an issue or PR if you find new v4.x patterns that should be included!
