# Boo Studio

A Halloween sticker workshop made from scratch on November 16, 2026 for Kiroween's Costume Contest category. No ClaritySnap code is reused.

## Features
- Four original canvas-drawn characters: ghost, pumpkin, cat and bat.
- Local PNG/JPG/WebP image import (up to 10 MB); oversized image dimensions are rejected.
- Four palettes, three lettering styles, character size/tilt and decorative stamps.
- 1200 × 1200 PNG export with an optional transparent scene background.
- Undo/redo and browser-local draft save/restore.
- Responsive layout, labeled controls and keyboard focus states.

No images are uploaded. The static app has no API keys, external dependencies, analytics, account system, AI image generation, or background-removal model. Uploaded photographs keep their original background and are placed in a photo frame.

## Keyboard shortcuts

| Action | macOS | Windows / Linux |
|--------|-------|-----------------|
| Undo   | Cmd Z | Ctrl Z          |
| Redo   | Cmd Shift Z | Ctrl Shift Z |

Shortcuts fire only when focus is on a non-editable element (not inside a text input, textarea, select or contenteditable). The on-screen Undo / Redo buttons work regardless of focus.

## Run
Requires Python 3. No package installation is needed.

```sh
python3 -m http.server 4187 --directory dist
```

Visit http://localhost:4187. Alternatively open `dist/index.html` directly; browser storage behavior may vary on file URLs.

```sh
node --check dist/app.js
```

## Kiro and provenance
See `PROVENANCE.md` for the actual contribution record. Initial specifications, implementation and artwork were prepared with Codex. Kiro CLI 3.3.0 then read the specs and steering, implemented keyboard undo/redo and fixed an asynchronous image-upload race. Codex reviewed these changes and corrected Shift+Z key normalization after a browser test. The actual Kiro review is recorded in `.kiro/specs/boo-studio/kiro-review.md`.

## Privacy
User images stay in memory unless the user explicitly saves a draft in localStorage. Saved drafts include the selected image and can be overwritten by saving a new draft. No application server receives image data. A hosting provider may independently record ordinary request metadata.

## License
MIT. Bundled character art is original drawing code covered by the same license.
