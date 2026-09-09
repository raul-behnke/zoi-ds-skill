---
name: zoi-ds
description: Use when building or reviewing any UI for ZOI Tech - enforces the @zoitechnologies/ds design system (tokens, Tailwind preset, component classes). Trigger with /zoi-ds or whenever writing HTML/CSS/React/Vue for ZOI projects.
---

# ZOI Design System

Official style guide of ZOI Tech, packaged as `@zoitechnologies/ds` (v1.0.2).
Everything visual MUST come from these tokens. Never invent a hex, a font size, or a spacing value.

## Setup (check this first)

If the project has no `@zoitechnologies/ds` in `package.json`, install and wire it before writing UI:

```bash
npm install @zoitechnologies/ds
```

```css
/* global stylesheet entry point */
@import '@zoitechnologies/ds/styles/theme.css';
```

```javascript
// tailwind.config.js
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  presets: [require('@zoitechnologies/ds')],
  theme: { extend: {} },
}
```

Live reference: `node_modules/@zoitechnologies/ds/styleguide/index.html` (open in browser; has light/dark toggle).

## The rules

1. **No raw values.** Colors, spacing, radius, font sizes, durations: always a token (`var(--space-6)`) or a Tailwind preset class (`p-6`, `bg-brand-primary`, `text-display-lg`). A literal `#b5ff81`, `padding: 22px`, or `font-size: 18px` in ZOI code is a bug.
2. **Semantic over primitive.** Prefer `var(--text-secondary)` to `var(--neutral-600)`, `var(--action-primary-bg)` to `var(--green-400)`. Primitives only when defining a new semantic token.
3. **Reuse component classes before writing CSS.** `.btn`/`.btn-primary`/`.btn-secondary`/`.btn-outline`/`.btn-sm`, `.form-group`/`.form-label`/`.form-input`, `.card`/`.card-dark`, `.container`, `.grid-2`/`.grid-3`. Don't restyle a button — use `.btn-*`.
4. **Headings use `--font-display` (Clash Display), body uses `--font-body`.** Buttons use display font too. Never mix in a third family.
5. **Brand color is an accent, not a background.** `--green-400` is for primary actions and highlights. Large surfaces are `--bg-page`, `--bg-surface`, or `--bg-dark`.
6. **Keep the focus ring.** Never `outline: none` without restoring `box-shadow: var(--focus-ring)`. Keyboard nav and A11y contrast are not optional.
7. **rem, not px.** Base is 16px = 1rem.
8. **Spacing scale only:** 1, 2, 3, 4, 5, 6, 8, 10, 12, 20, 25. Need 22px? Round to the scale.

## Tokens

### Brand & neutrals
| Token | Value | Use |
|---|---|---|
| `--green-400` | `#b5ff81` | primary action, brand accent |
| `--green-500` | `#90cc67` | primary hover |
| `--green-900` | `#364c26` | dark brand |
| `--magenta-500` | `#cc3366` | accent, sparingly |
| `--neutral-0 / 50 / 100 / 200 / 300` | `#ffffff` `#f7fff2` `#e7e7e7` `#cccccc` `#b0b0b0` | surfaces, borders, placeholder |
| `--neutral-500 / 600 / 700` | `#808080` `#5c5c5c` `#3f444b` | text tertiary/secondary, dark border |
| `--neutral-800 / 850 / 900` | `#333333` `#33373d` `#141414` | text primary, dark surfaces |
| `--system-error / success / warning` | `#e53935` `#43a047` `#fb8c00` | feedback only, never decoration |

### Semantic
Backgrounds `--bg-page` `--bg-surface` `--bg-dark` `--bg-input` ·
Text `--text-primary` `--text-secondary` `--text-tertiary` `--text-inverse` `--text-brand` ·
Actions `--action-primary-bg|fg|hover` `--action-secondary-bg|fg|hover` ·
Borders `--border-default` `--border-active` `--focus-ring`

### Scale
- **Spacing:** `--space-1` .25rem → `--space-25` 6.25rem (see rule 8)
- **Radius:** `--radius-sm` .25rem (inputs) · `--radius-md` .75rem (buttons, cards) · `--radius-lg` 1rem · `--radius-full` pills
- **Type:** `.text-display-xl` 3.5rem · `display-lg` 3rem · `display-md` 2rem · `body-lg` 1.25rem · `body-md` 1rem · `body-sm` .875rem
- **Layout:** `--container-max` 80rem / 1280px, padding `--space-5`
- **Motion:** `--duration-fast` 150ms · `--duration-normal` 300ms · `--ease-out` · `--ease-in-out`. Nothing animates longer than 300ms.
- **Z-index:** `--z-base` 0 · `--z-sticky` 100 · `--z-overlay` 1000 · `--z-modal` 1100. Never a raw z-index.
- **Breakpoints:** sm 640 · md 768 · lg 1024 · xl 1280. Mobile-first.

### Tailwind class names (from the preset)
`bg-brand-primary` `bg-brand-dark` `text-brand-accent` · `bg-bg-page` `bg-bg-surface` `bg-bg-dark` `bg-bg-input` ·
`text-text-primary` `text-text-secondary` `text-text-inverse` · `text-system-error` `text-system-success` ·
`font-display` `font-body` · `text-display-xl` … `text-body-sm` · `shadow-focus`

## Review checklist

When reviewing ZOI UI code, flag:
- [ ] hardcoded hex / rgb / px values that map to an existing token
- [ ] primitive token where a semantic one exists
- [ ] hand-rolled button, input, or card instead of `.btn-*` / `.form-*` / `.card`
- [ ] `outline: none` with no focus ring replacement
- [ ] font family outside Clash Display / system body
- [ ] spacing off the scale
- [ ] raw z-index or transition duration

## Known gaps

- `theme.css` ships **light mode only**. The `[data-theme="dark"]` overrides exist solely inside `styleguide/index.html`. If a project needs dark mode, lift those overrides into the project (or better: PR them into the package) — don't invent new dark values.
- The package ships tokens + CSS classes only. No React/Vue components. Build components locally on top of these classes.
