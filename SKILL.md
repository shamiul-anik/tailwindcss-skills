---
name: Tailwind CSS v4
description: Comprehensive Tailwind CSS v4.2 reference for generating modern, utility-first CSS. Covers CSS-first configuration, theme variables, responsive design, dark mode, gradients, animations, custom utilities, and common v3→v4 migration pitfalls.
---

# Tailwind CSS v4.2 — LLM Reference Guide

> **Current Version**: 4.2.1 (February 2026)
> Tailwind CSS v4 is a ground-up rewrite. If your prior knowledge is based on v3, **read the entire "Migration Pitfalls" section** before generating any code.

---

## 1. Installation

### Vite (Recommended — fastest)

```bash
# npm
npm i tailwindcss @tailwindcss/vite

# pnpm
pnpm add tailwindcss @tailwindcss/vite
```

```js
// vite.config.js
import tailwindcss from "@tailwindcss/vite";
export default { plugins: [tailwindcss()] };
```

### PostCSS

```bash
# npm
npm i tailwindcss @tailwindcss/postcss

# pnpm
pnpm add tailwindcss @tailwindcss/postcss
```

```js
// postcss.config.js
export default { plugins: ["@tailwindcss/postcss"] };
```

### Webpack (New in v4.2)

```bash
# npm
npm i tailwindcss @tailwindcss/webpack

# pnpm
pnpm add tailwindcss @tailwindcss/webpack
```

```js
// webpack.config.js
const tailwindcss = require("@tailwindcss/webpack");
module.exports = { plugins: [new tailwindcss()] };
```

### Tailwind CLI

```bash
# npm
npx @tailwindcss/cli -i app.css -o dist/app.css --watch

# pnpm
pnpm dlx @tailwindcss/cli -i app.css -o dist/app.css --watch
```

### Play CDN (prototyping only)

```html
<script src="https://cdn.tailwindcss.com"></script>
```

### CSS Entry Point (all methods)

```css
/* app.css — this is the ONLY line required */
@import "tailwindcss";
```

---

## 2. CSS-First Configuration (`@theme`)

Tailwind v4 replaces `tailwind.config.js` with CSS-first configuration using the `@theme` directive. All design tokens are defined directly in your CSS file.

```css
@import "tailwindcss";

@theme {
  /* Colors (use OKLCH for P3 gamut) */
  --color-brand-50: oklch(0.97 0.01 250);
  --color-brand-500: oklch(0.62 0.17 256);
  --color-brand-900: oklch(0.25 0.09 260);

  /* Typography */
  --font-display: "Inter", "system-ui", sans-serif;
  --font-body: "Inter", "system-ui", sans-serif;

  /* Spacing base (default: 0.25rem) */
  --spacing: 0.25rem;

  /* Custom breakpoint */
  --breakpoint-3xl: 1920px;

  /* Custom easing */
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);

  /* Custom animation with @keyframes */
  --animate-fade-in: fade-in 0.3s ease-out;
  @keyframes fade-in {
    from {
      opacity: 0;
      transform: translateY(-4px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
```

### Theme Variable Namespaces

Each namespace maps to specific utility classes:

| Namespace          | Example Variable      | Generated Utilities                                  |
| ------------------ | --------------------- | ---------------------------------------------------- |
| `--color-*`        | `--color-brand-500`   | `bg-brand-500`, `text-brand-500`, `border-brand-500` |
| `--font-*`         | `--font-display`      | `font-display`                                       |
| `--text-*`         | `--text-3xl`          | `text-3xl`                                           |
| `--font-weight-*`  | `--font-weight-black` | `font-black`                                         |
| `--tracking-*`     | `--tracking-tight`    | `tracking-tight`                                     |
| `--leading-*`      | `--leading-snug`      | `leading-snug`                                       |
| `--breakpoint-*`   | `--breakpoint-sm`     | `sm:*` variant                                       |
| `--container-*`    | `--container-sm`      | `@sm:*` variant, `max-w-sm`                          |
| `--spacing-*`      | `--spacing`           | `p-4`, `m-8`, `gap-2`, `w-16`, `h-24`                |
| `--radius-*`       | `--radius-lg`         | `rounded-lg`                                         |
| `--shadow-*`       | `--shadow-xl`         | `shadow-xl`                                          |
| `--inset-shadow-*` | `--inset-shadow-sm`   | `inset-shadow-sm`                                    |
| `--drop-shadow-*`  | `--drop-shadow-md`    | `drop-shadow-md`                                     |
| `--blur-*`         | `--blur-lg`           | `blur-lg`                                            |
| `--perspective-*`  | `--perspective-near`  | `perspective-near`                                   |
| `--aspect-*`       | `--aspect-video`      | `aspect-video`                                       |
| `--ease-*`         | `--ease-out`          | `ease-out`                                           |
| `--animate-*`      | `--animate-spin`      | `animate-spin`                                       |

### Extending vs. Overriding

```css
/* Extend: adds to defaults */
@theme {
  --color-brand-500: oklch(0.62 0.17 256);
}

/* Override a namespace: resets ALL defaults in that namespace */
@theme {
  --color-*: initial;
  --color-brand: #3f3cbb;
  --color-white: #fff;
}

/* Full custom theme: resets EVERYTHING */
@theme {
  --*: initial;
  --spacing: 4px;
  --color-primary: oklch(0.62 0.17 256);
}
```

### Referencing Variables & Static Output

```css
/* Use `inline` when referencing other CSS variables */
@theme inline {
  --font-sans: var(--font-inter);
}

/* Use `static` to always emit CSS variables even if unused */
@theme static {
  --color-primary: var(--color-red-500);
}
```

---

## 3. Dark Mode

### Default (automatic via `prefers-color-scheme`)

```html
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  Content adapts to OS theme automatically.
</div>
```

### Manual Toggle (class-based)

```css
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));
```

```html
<html class="dark">
  <body class="bg-white dark:bg-black">
    ...
  </body>
</html>
```

### Manual Toggle (data-attribute-based)

```css
@import "tailwindcss";
@custom-variant dark (&:where([data-theme=dark], [data-theme=dark] *));
```

```html
<html data-theme="dark">
  <body class="bg-white dark:bg-black">
    ...
  </body>
</html>
```

### Three-Way Toggle (Light / Dark / System)

```js
// Add inline in <head> to prevent FOUC
document.documentElement.classList.toggle(
  "dark",
  localStorage.theme === "dark" ||
    (!("theme" in localStorage) &&
      window.matchMedia("(prefers-color-scheme: dark)").matches),
);

// User picks light
localStorage.theme = "light";
// User picks dark
localStorage.theme = "dark";
// User picks system
localStorage.removeItem("theme");
```

### `color-scheme` Utility

Fixes light scrollbars in dark mode:

```html
<html class="dark scheme-dark">
  ...
</html>
```

---

## 4. Responsive Design

### Default Breakpoints (mobile-first, min-width)

| Prefix | Min-width      | CSS                       |
| ------ | -------------- | ------------------------- |
| `sm:`  | 40rem (640px)  | `@media (width >= 40rem)` |
| `md:`  | 48rem (768px)  | `@media (width >= 48rem)` |
| `lg:`  | 64rem (1024px) | `@media (width >= 64rem)` |
| `xl:`  | 80rem (1280px) | `@media (width >= 80rem)` |
| `2xl:` | 96rem (1536px) | `@media (width >= 96rem)` |

**IMPORTANT**: Unprefixed classes target ALL screens (including mobile). `sm:` means "640px and UP", NOT "small screens".

```html
<!-- Mobile-first: stack on mobile, side-by-side on md+ -->
<div class="flex flex-col md:flex-row gap-4">
  <div class="w-full md:w-1/2">Sidebar</div>
  <div class="w-full md:w-1/2">Content</div>
</div>
```

### Max-Width / Range Variants

```html
<!-- Only on screens smaller than md -->
<div class="max-md:hidden">Desktop only</div>

<!-- Only between md and xl -->
<div class="md:max-xl:flex">Shows on md to xl only</div>
```

### Custom Breakpoints

```css
@theme {
  --breakpoint-xs: 30rem;
  --breakpoint-3xl: 1920px;
}
```

### Container Queries (Built-in, no plugin)

```html
<div class="@container">
  <div class="grid grid-cols-1 @sm:grid-cols-2 @lg:grid-cols-4">
    <!-- Responds to PARENT container width, not viewport -->
  </div>
</div>

<!-- Max-width container queries -->
<div class="@container">
  <div class="grid grid-cols-3 @max-md:grid-cols-1">...</div>
</div>

<!-- Named containers -->
<div class="@container/sidebar">
  <div class="@sm/sidebar:grid-cols-2">...</div>
</div>
```

---

## 5. Gradients (Renamed & Expanded in v4)

### Linear Gradients

```html
<!-- Direction keywords (renamed from bg-gradient-to-*) -->
<div class="bg-linear-to-r from-blue-500 to-purple-500"></div>

<!-- Angle values (NEW in v4) -->
<div class="bg-linear-45 from-indigo-500 via-purple-500 to-pink-500"></div>

<!-- Color interpolation modifiers -->
<div class="bg-linear-to-r/oklch from-blue-500 to-green-400"></div>
<div class="bg-linear-to-r/srgb from-blue-500 to-green-400"></div>
```

### Radial & Conic Gradients (NEW in v4)

```html
<div class="bg-radial from-white to-zinc-900"></div>
<div class="bg-radial-[at_25%_25%] from-white to-zinc-900 to-75%"></div>
<div class="bg-conic from-red-500 via-yellow-500 to-red-500"></div>
<div class="bg-conic/[in_hsl_longer_hue] from-red-600 to-red-600"></div>
```

---

## 6. Colors

### Default Color Space: OKLCH

Tailwind v4 uses **OKLCH** for wider gamut (P3) and perceptual uniformity. Opacity is handled via `color-mix()`:

```html
<!-- Opacity modifiers work with any color, including CSS variables -->
<div class="bg-blue-500/75"></div>
<!-- 75% opacity -->
<div class="text-white/50"></div>
<!-- 50% opacity -->
```

### Default Color Palettes

`slate`, `gray`, `zinc`, `neutral`, `stone`, `red`, `orange`, `amber`, `yellow`, `lime`, `green`, `emerald`, `teal`, `cyan`, `sky`, `blue`, `indigo`, `violet`, `purple`, `fuchsia`, `pink`, `rose`

**New in v4.2**: `mauve`, `olive`, `mist`, `taupe`

Each palette has shades: `50`, `100`, `200`, `300`, `400`, `500`, `600`, `700`, `800`, `900`, `950`

---

## 7. Dynamic Utility Values

Tailwind v4 supports dynamic values for many utilities — no arbitrary value `[]` syntax needed:

```html
<!-- Grid columns — any integer works -->
<div class="grid grid-cols-13"></div>

<!-- Spacing — derived from --spacing variable (default 0.25rem) -->
<div class="mt-18 px-7 gap-11"></div>
<!-- mt = 18 × 0.25rem = 4.5rem -->

<!-- Data attributes — no config required -->
<div data-active class="opacity-50 data-active:opacity-100"></div>

<!-- Dynamic widths -->
<div class="w-17 h-23"></div>
```

### Arbitrary Values (when dynamic isn't enough)

```html
<div class="w-[37.5%]"></div>
<div class="bg-[#1a1a2e]"></div>
<div class="text-[clamp(1rem,2vw,1.5rem)]"></div>
<div class="grid-cols-[200px_1fr_200px]"></div>
```

### Arbitrary Properties

```html
<div class="[clip-path:circle(50%)]"></div>
<div class="[writing-mode:vertical-rl]"></div>
```

### Arbitrary Variants

```html
<div class="[&:nth-child(3)]:bg-red-500"></div>
<div class="[@supports(display:grid)]:grid"></div>
```

---

## 8. Shadows (Stackable in v4)

```html
<!-- Standard shadows -->
<div class="shadow-sm"></div>
<div class="shadow-md"></div>
<div class="shadow-xl"></div>

<!-- Inset shadows (NEW in v4) -->
<div class="inset-shadow-sm"></div>

<!-- Ring (border-like shadow) -->
<div class="ring ring-blue-500"></div>
<div class="inset-ring inset-ring-white/10"></div>

<!-- Stack up to 4 layers! -->
<div
  class="shadow-lg shadow-black/20 inset-shadow-sm inset-shadow-white/10 ring ring-black/5 inset-ring inset-ring-white/10"
></div>
```

---

## 9. 3D Transforms

```html
<div class="perspective-distant">
  <div class="transform-3d rotate-x-12 rotate-y-6 translate-z-8">
    3D transformed element
  </div>
</div>
```

| Utility                                     | Description                 |
| ------------------------------------------- | --------------------------- |
| `perspective-normal`, `perspective-distant` | Set parent perspective      |
| `perspective-origin-center`, etc.           | Perspective origin point    |
| `transform-3d`                              | Enable 3D transform context |
| `rotate-x-*`, `rotate-y-*`, `rotate-z-*`    | 3D rotation                 |
| `translate-z-*`                             | Z-axis translation          |
| `scale-z-*`                                 | Z-axis scaling              |
| `backface-visible`, `backface-hidden`       | Backface visibility         |

---

## 10. Animations & Transitions

### Transitions

```html
<button class="transition-colors duration-200 ease-out hover:bg-blue-600">
  Hover me
</button>

<div class="transition-all duration-300 ease-spring">Smooth transition</div>
```

### Built-in Animations

```html
<div class="animate-spin"></div>
<div class="animate-pulse"></div>
<div class="animate-bounce"></div>
<div class="animate-ping"></div>
```

### Custom Animations in @theme

```css
@theme {
  --animate-slide-up: slide-up 0.3s ease-out;
  @keyframes slide-up {
    from {
      opacity: 0;
      transform: translateY(8px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
```

```html
<div class="animate-slide-up">Slides in</div>
```

### `starting:` Variant (CSS `@starting-style`)

Animate elements when they first appear — no JavaScript needed:

```html
<div
  popover
  class="transition-opacity starting:open:opacity-0 open:opacity-100"
>
  Animates in when popover opens
</div>
```

---

## 11. States & Variants

### Pseudo-Classes

| Variant          | CSS                | Example                     |
| ---------------- | ------------------ | --------------------------- |
| `hover:`         | `:hover`           | `hover:bg-blue-600`         |
| `focus:`         | `:focus`           | `focus:ring-2`              |
| `focus-visible:` | `:focus-visible`   | `focus-visible:outline-2`   |
| `focus-within:`  | `:focus-within`    | `focus-within:ring-2`       |
| `active:`        | `:active`          | `active:scale-95`           |
| `disabled:`      | `:disabled`        | `disabled:opacity-50`       |
| `first:`         | `:first-child`     | `first:mt-0`                |
| `last:`          | `:last-child`      | `last:mb-0`                 |
| `odd:`           | `:nth-child(odd)`  | `odd:bg-gray-50`            |
| `even:`          | `:nth-child(even)` | `even:bg-white`             |
| `nth-*:`         | `:nth-child(*)`    | `nth-3:bg-red-500`          |
| `placeholder:`   | `::placeholder`    | `placeholder:text-gray-400` |
| `not-*:`         | `:not(*)`          | `not-hover:opacity-75`      |

### Group & Peer

```html
<!-- Group: parent state affects child -->
<div class="group hover:bg-blue-100">
  <span class="group-hover:text-blue-600">I change on parent hover</span>
</div>

<!-- in-* variant (new, replaces group-* in many cases) -->
<div class="hover:bg-blue-100">
  <span class="in-hover:text-blue-600"
    >Same effect, no "group" class needed</span
  >
</div>

<!-- Peer: sibling state affects element -->
<input class="peer" type="checkbox" />
<label class="peer-checked:text-green-600">Checked!</label>
```

### Descendant Variant

```html
<div class="**:text-gray-700 **:leading-relaxed">
  <p>All descendants inherit these styles.</p>
  <p>Including this one.</p>
</div>
```

### Other Useful Variants

- `open:` — targets `<details>` and `<dialog>` open state, and `:popover-open`
- `inert:` — targets elements with the `inert` attribute
- `starting:` — CSS `@starting-style` for enter animations
- `not-*:` — negation pseudo-class and media queries
- `noscript:` — when JavaScript is disabled (v4.1+)
- `details-content:` — style `<details>` content for accordions (v4.1+)

---

## 12. Text Shadows & Masks (v4.1+)

### Text Shadows

```html
<h1 class="text-shadow-sm">Subtle shadow</h1>
<h1 class="text-shadow-lg text-shadow-black/25">Prominent shadow</h1>
```

### Masks

```html
<div class="mask-linear-to-b">Fades to transparent at bottom</div>
<img class="mask-radial" src="photo.jpg" />
```

### Colored Drop Shadows (v4.1+)

```html
<div class="drop-shadow-lg drop-shadow-blue-500/50">Colored shadow</div>
```

---

## 13. Logical Properties (v4.2)

Writing-mode-aware utilities for internationalization:

| Utility         | CSS Property                  |
| --------------- | ----------------------------- |
| `pbs-*`         | `padding-block-start`         |
| `pbe-*`         | `padding-block-end`           |
| `mbs-*`         | `margin-block-start`          |
| `mbe-*`         | `margin-block-end`            |
| `border-bs`     | `border-block-start`          |
| `border-be`     | `border-block-end`            |
| `scroll-pbs-*`  | `scroll-padding-block-start`  |
| `scroll-mbe-*`  | `scroll-margin-block-end`     |
| `inline-s-*`    | Replaces deprecated `start-*` |
| `inline-e-*`    | Replaces deprecated `end-*`   |
| `inline-size-*` | `inline-size` (logical width) |
| `block-size-*`  | `block-size` (logical height) |

### `font-features-*` (v4.2)

```html
<span class="font-features-[smcp]">Small Caps</span>
<span class="font-features-[liga,dlig]">Ligatures</span>
```

---

## 14. Adding Custom Utilities & Variants

### Custom Utilities

```css
/* Simple utility */
@utility content-auto {
  content-visibility: auto;
}

/* Complex utility with nesting */
@utility scrollbar-hidden {
  &::-webkit-scrollbar {
    display: none;
  }
}

/* Functional utility (accepts values) */
@utility tab-* {
  tab-size: --value(integer); /* bare number: tab-4 */
  tab-size: --value(--tab-size-*); /* theme key:   tab-github */
  tab-size: --value([integer]); /* arbitrary:   tab-[8] */
}
```

### Custom Variants

```css
/* Shorthand */
@custom-variant theme-midnight (&:where([data-theme="midnight"] *));

/* With nesting for complex logic */
@custom-variant any-hover {
  @media (any-hover: hover) {
    &:hover {
      @slot;
    }
  }
}
```

### Using `@variant` in Custom CSS

```css
.card {
  background: white;
  @variant dark {
    background: black;
  }
  @variant dark {
    @variant hover {
      background: #1a1a1a;
    }
  }
}
```

### CSS Layers

```css
/* Base styles */
@layer base {
  h1 {
    font-size: var(--text-3xl);
    font-weight: var(--font-weight-bold);
  }
  h2 {
    font-size: var(--text-2xl);
  }
}

/* Component classes (overridable by utilities) */
@layer components {
  .btn {
    padding: var(--spacing-2) var(--spacing-4);
    border-radius: var(--radius-lg);
    font-weight: var(--font-weight-semibold);
  }
  .card {
    background-color: var(--color-white);
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-xl);
  }
}
```

---

## 15. Source Detection & Content Control

```css
@import "tailwindcss";

/* Include additional source directories */
@source "../node_modules/@my-company/ui-lib";

/* Exclude specific paths (v4.1+) */
@source not "../src/legacy";

/* Safelist specific utilities for dynamic class names (v4.1+) */
@source inline("bg-red-500 bg-green-500 bg-blue-500");
```

---

## 16. Official Plugins

```css
@import "tailwindcss";

/* Typography plugin — adds `prose` class for rich content */
@plugin "@tailwindcss/typography";

/* Forms plugin — better default form styles */
@plugin "@tailwindcss/forms";
```

Usage:

```html
<!-- Typography -->
<article class="prose prose-lg dark:prose-invert">
  <h1>Rich formatted content</h1>
  <p>Automatically styled paragraphs, lists, code blocks, etc.</p>
</article>

<!-- Forms -->
<input type="text" class="form-input rounded-md" />
<select class="form-select rounded-md">
  ...
</select>
```

---

## 17. Common Utility Quick Reference

### Layout

```
flex  flex-col  flex-row  flex-wrap  flex-1  grow  shrink-0
grid  grid-cols-{n}  grid-rows-{n}  gap-{n}  col-span-{n}
block  inline  inline-flex  inline-grid  hidden
items-center  items-start  justify-center  justify-between
```

### Spacing

```
p-{n}  px-{n}  py-{n}  pt-{n}  pr-{n}  pb-{n}  pl-{n}
m-{n}  mx-{n}  my-{n}  mt-{n}  mr-{n}  mb-{n}  ml-{n}  m-auto
space-x-{n}  space-y-{n}  gap-{n}  gap-x-{n}  gap-y-{n}
```

### Sizing

```
w-{n}  w-full  w-screen  w-auto  w-fit  w-min  w-max  w-1/2  w-1/3
h-{n}  h-full  h-screen  h-auto  h-fit  h-min  h-max
min-w-{n}  max-w-{n}  min-h-{n}  max-h-{n}
size-{n}  (sets both width and height)
```

### Typography

```
text-{size}  text-{color}  font-{family}  font-{weight}
leading-{n}  tracking-{n}  text-center  text-left  text-right
underline  line-through  no-underline  uppercase  lowercase  capitalize
truncate  line-clamp-{n}  text-wrap  text-nowrap  text-balance
```

### Borders & Rounded

```
border  border-{n}  border-{color}  border-t  border-b  border-l  border-r
rounded  rounded-{size}  rounded-full  rounded-none
rounded-t-{size}  rounded-b-{size}  rounded-l-{size}  rounded-r-{size}
ring  ring-{n}  ring-{color}  ring-offset-{n}
divide-x  divide-y  divide-{color}
```

### Backgrounds

```
bg-{color}  bg-{color}/{opacity}
bg-linear-to-{dir}  bg-linear-{angle}  bg-radial  bg-conic
from-{color}  via-{color}  to-{color}
bg-cover  bg-contain  bg-center  bg-no-repeat  bg-fixed
```

### Effects

```
opacity-{n}  shadow-{size}  shadow-{color}
blur-{size}  brightness-{n}  contrast-{n}  grayscale  invert
backdrop-blur-{size}  backdrop-brightness-{n}
mix-blend-{mode}
```

### Positioning

```
relative  absolute  fixed  sticky
top-{n}  right-{n}  bottom-{n}  left-{n}  inset-{n}
z-{n}  z-auto
overflow-hidden  overflow-auto  overflow-scroll  overflow-visible
```

### Interactivity

```
cursor-pointer  cursor-not-allowed  cursor-grab
select-none  select-all  select-text
pointer-events-none  pointer-events-auto
resize  resize-x  resize-y  resize-none
scroll-smooth  scroll-mt-{n}  snap-x  snap-y  snap-start  snap-center
field-sizing-content  (auto-resize textarea, no JS needed)
```

---

## 18. Migration Pitfalls (v3 → v4)

> **CRITICAL**: LLMs trained on older data will suggest deprecated v3 patterns.
> Always use the v4 patterns below.

| ❌ v3 (DO NOT USE)                                                   | ✅ v4 (USE THIS)                                         |
| -------------------------------------------------------------------- | -------------------------------------------------------- |
| `tailwind.config.js` with `module.exports`                           | `@theme { }` in CSS                                      |
| `@tailwind base;` / `@tailwind components;` / `@tailwind utilities;` | `@import "tailwindcss";` (single line)                   |
| `content: ["./src/**/*.{js,jsx}"]` in config                         | Automatic detection (use `@source` only for exceptions)  |
| `bg-gradient-to-r`                                                   | `bg-linear-to-r`                                         |
| `bg-gradient-to-br`                                                  | `bg-linear-to-br`                                        |
| `theme.extend.colors` in JS config                                   | `@theme { --color-*: ...; }` in CSS                      |
| `darkMode: 'class'` in JS config                                     | `@custom-variant dark (&:where(.dark, .dark *));`        |
| `@apply` for custom components                                       | `@layer components { .btn { ... } }` using CSS variables |
| `@tailwindcss/container-queries` plugin                              | Built-in: `@container` + `@sm:`, `@lg:`, etc.            |
| `postcss-import` plugin                                              | Built-in import support                                  |
| `start-*` / `end-*`                                                  | `inline-s-*` / `inline-e-*` (v4.2 deprecation)           |
| `screens` in JS config                                               | `--breakpoint-*` in `@theme`                             |
| `ring-opacity-*` / `bg-opacity-*`                                    | `ring-blue-500/50` / `bg-blue-500/50` (modifier syntax)  |

### What still works

- All utility class names (e.g., `flex`, `p-4`, `text-center`) remain the same.
- Arbitrary value syntax `[value]` still works.
- All variant prefixes (`hover:`, `focus:`, `dark:`, `sm:`) still work.
- `@apply` still works but is discouraged for new code.

---

## 19. Design Best Practices

1. **Mobile-first**: Start with unprefixed utilities, add `sm:`, `md:`, `lg:` for larger screens.
2. **Use OKLCH colors**: More vibrant gradients and perceptually uniform color scales. Prefer `oklch()` in `@theme` over hex or rgb.
3. **Leverage CSS variables**: Tokens from `@theme` are available as `var(--color-brand-500)` in custom CSS or inline styles.
4. **Prefer semantic HTML**: Use `<header>`, `<main>`, `<nav>`, `<section>`, `<article>`, `<footer>`.
5. **Layer shadows**: Stack `shadow-*`, `inset-shadow-*`, `ring`, and `inset-ring` for depth.
6. **Use container queries**: Prefer `@container` + `@sm:` over media queries for reusable components.
7. **Use `transition-*` wisely**: Add `transition-colors`, `transition-transform`, or `transition-all` with `duration-*` and `ease-*`.
8. **Dark mode from the start**: Always pair light classes with `dark:` variants.
9. **Avoid arbitrary values** when a dynamic utility exists (e.g., use `grid-cols-13` not `grid-cols-[13]`).
10. **Use `@layer components`** for reusable component classes (cards, buttons, badges) — they stay overridable by utilities.
