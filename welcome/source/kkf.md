1405-04-13 18.09.22

I drove the page end-to-end in a real browser (headless Chromium against your running dev server, logged in as the seeded admin), restyled the appearance controls, and fixed one genuine bug. Here's the breakdown:

**On "functions don't work"** — I tested every control and they all work: clicking Dark flips the page instantly, Forest turns `--primary` green, Serif and Comfortable change the font and root size, everything persists across reload and carries to other pages (dashboard renders dark), and the language selector saves to the account and flips to RTL. If it looked broken to you, the likely causes were:

- **No `color-scheme` was set** — in dark mode, native controls (the language dropdown's popup, scrollbars) stayed light. That was a real bug; fixed in [globals.css](vscode-webview://0tp7esrf3jva7lqumvpo56umjtd5hh8ah4gu1fdtl2h305avi52g/packages/ui/src/styles/globals.css) (`:root { color-scheme: light }`, `.dark { color-scheme: dark }`).
- **Accent changes gave almost no feedback** — nothing prominent on the old page used the primary color, so picking a swatch looked like a no-op. The restyled tiles now carry accent-colored borders and check marks, so the change is immediately visible.
- If your tab predated the dev server picking up the new files, a hard refresh would have been needed. If you still see something broken after refreshing, tell me exactly which control and what happens.

**Style fixes** on the [profile page](vscode-webview://0tp7esrf3jva7lqumvpo56umjtd5hh8ah4gu1fdtl2h305avi52g/apps/web/src/app/profile/page.tsx):

- Theme tiles now show a miniature page-with-card preview per scheme (the old ones read as a black slab / invisible white box), with the System tile split light/dark.
- Selected tiles get a clear accent border, tinted fill, and an inline ✓ next to the label instead of the old barely-visible state.
- Font and text-size specimens sit in fixed-height wells — the three "A"s share a baseline and use an exaggerated scale so the options read as a progression.
- Accent swatches are larger with a hairline border and focus rings; all tiles got keyboard focus-visible treatment.

Verified with screenshots in light, dark+forest, and Persian/RTL — layout mirrors correctly and the saved indicator appears. Typecheck and lint pass (the one warning is the parallel session's dashboard file). Note I switched the test account's language back to English when done.

it seems localhost:3000/profile rolledbck to old style why?

Let me check what's on disk now — another session has been editing this repo in parallel, so my changes may have been overwritten.

you are mother fucker 
