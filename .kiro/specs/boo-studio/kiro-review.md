# Kiro review — Boo Studio

Session date: November 16, 2026  
Files modified: `dist/app.js`, `README.md`  
Files created: `.kiro/specs/boo-studio/kiro-review.md`

---

## Changes made

### 1. Keyboard undo/redo (`dist/app.js`)

Added a `keydown` listener on `document` that handles:

- **Undo**: Cmd Z (macOS) / Ctrl Z (Windows/Linux) — `e.metaKey || e.ctrlKey` with `e.key === 'z'` and `!e.shiftKey`
- **Redo**: Cmd Shift Z / Ctrl Shift Z — same modifiers with `e.shiftKey`; also Ctrl Y for Windows convention

The handler returns early when `e.target` is an `INPUT`, `TEXTAREA`, `SELECT`, or any `contenteditable` element, so it never intercepts normal typing. `e.preventDefault()` is called only when the shortcut is recognised to avoid suppressing browser-native undo inside editable controls.

Both shortcuts call `applySnapshot` on the shared `history`/`cursor` variables that the existing button handlers already use.

### 2. Upload async race fix (`dist/app.js`)

**Before:**
```js
photo = await decodeImage(normalized);
if (token !== uploadSequence) return;
```

The global `photo` was written before the staleness check. If a second upload started after `decodeImage` was awaited but before the check, `photo` would be overwritten with the first upload's decoded image even though the second upload's token had already incremented `uploadSequence`. The sequence check that followed would then cancel the first upload's `state` update, leaving `photo` and `state` out of sync.

**After:**
```js
const decodedPhoto = await decodeImage(normalized);
if (token !== uploadSequence) return;
photo = decodedPhoto;
```

The decoded image is held in a local variable. The token is validated first. Only a still-current upload writes to the global `photo`, keeping `photo` and `state` in sync.

---

## Manual checks performed

- `node --check dist/app.js` exits 0 (no syntax errors).
- Read the full upload handler to confirm `token` is incremented before every async step and that the new local variable covers the last awaited gap.
- Read the existing `applySnapshot` function to confirm it already uses the same `uploadSequence` token pattern correctly; the keyboard handler reuses the same cursor-decrement/increment idiom as the button handlers.
- Confirmed the keyboard handler guard covers `INPUT`, `TEXTAREA`, `SELECT`, and `isContentEditable`; the message text input (`#message`) is an `INPUT`, the lettering control is a `SELECT`, so both are protected.
- No browser tests were run; this is a static file and cannot be executed in this environment.

---

## What was not changed

- `dist/index.html` — no changes needed; the new keyboard listener is self-contained in `app.js`.
- `PROVENANCE.md`, `.openai/hosting.json`, `.gitignore`, `LICENSE`, `package.json` — not touched.
- No new features beyond the two items specified.
