---
type: Report
title: Profile Page Appearance Controls — Styling Fixes, Bug Report & Rollback
description: A technical report on styling profile page appearance controls (theme tiles, accent swatches), a fix for color-scheme in dark mode, and a subsequent rollback due to parallel edits.
resource: /wiki/welcome/source/kkf
tags: [profile, styling, dark-mode, theme, accessibility, bug-fix, rollback]
timestamp: 2025-04-13T18:09:22+00:00
---

This page documents work done on the profile page appearance controls. The work included restyling theme tiles, font/text-size controls, and accent swatches, along with a genuine bug fix. **Note**: after the changes were pushed, `localhost:3000/profile` rolled back to the old style due to concurrent parallel edits by another session.

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

## Testing & Verification

- Tested in a real browser (headless Chromium) against the running dev server, logged in as the seeded admin.
- Every control works: Dark flips instantly, Forest turns `--primary` green, Serif and Comfortable change font/root size, all persist across reload and carry to other pages.
- Language selector saves to the account and flips to RTL.
- Tested in light mode, dark+forest mode, and Persian/RTL — layout mirrors correctly and the saved indicator appears.
- Typecheck and lint pass (one warning from a parallel session's dashboard file).
- The test account's language was switched back to English when done.

## Rollback Incident

After pushing the changes, `localhost:3000/profile` appeared to roll back to the old style. The suspected cause is another session editing the same repo in parallel, which may have overwritten the changes. This was discovered when checking what was on disk after the rollback.
