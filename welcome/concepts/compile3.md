---
type: Report
title: Profile Page Appearance Controls — Styling Fixes & Bug Report
description: A technical report on styling profile page appearance controls (theme tiles, accent swatches) and a fix for color-scheme not being set in dark mode.
resource: /wiki/welcome/source/kkf
tags: [profile, styling, dark-mode, theme, accessibility, bug-fix]
timestamp: 2025-04-13T18:09:22+00:00
---

This page documents work done on the profile page appearance controls. The work included restyling theme tiles, font/text-size controls, and accent swatches, along with a genuine bug fix.

## Bug Fix: Missing `color-scheme` in Dark Mode

Native UI controls (language dropdown popup, scrollbars) stayed light when the page was in dark mode. **Fix**: Added `color-scheme` to CSS:

```css
:root { color-scheme: light }
.dark { color-scheme: dark }
```

Set in `packages/ui/src/styles/globals.css`.

## Style Fixes on the Profile Page

Changes applied to the profile page (`apps/web/src/app/profile/page.tsx`):

### Theme Tiles
- Now show a miniature page-with-card preview per scheme (previously read as a black slab / invisible white box).
- System tile is split light/dark visually.
- Selected tiles get a clear accent border, tinted fill, and inline ✓ next to the label.

### Font & Text-Size Specimens
- Placed in fixed-height wells.
- The three "A"s share a baseline and use an exaggerated scale so the options read as a progression.

### Accent Swatches
- Larger with a hairline border and focus rings.
- All tiles received keyboard focus-visible treatment.

## Verification

- Tested in light mode, dark+forest mode, and Persian/RTL.
- Layout mirrors correctly in RTL.
- Saved indicator appears and persists across reloads.
- Typecheck and lint pass (one warning from a parallel session's dashboard file).

## Note: Rollback Observed

After the changes were pushed, `localhost:3000/profile` appeared to roll back to the old style. The suspected cause is another session editing the same repo in parallel, which may have overwritten the changes.