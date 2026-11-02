# Update — conditions on the board

Two things to upload:

1. `index.html` — replaces the existing one.
2. `cond/` — a new folder of 23 PNG icons. **This must sit next to `index.html`**,
   i.e. `https://<your-project>.pages.dev/cond/1f631.png` should resolve.
   If the folder is missing the panel still works; the board simply shows no
   condition icons (broken-image squares are not drawn — OBR just fails to load them).

`manifest.json`, `icon.svg`, `add.svg`, `remove.svg`, `drop.png`, `skull.png`
are unchanged.

## What the icons do

- One small disc per condition, in a row along the bottom edge of the token,
  a quarter of the token's size, shrinking to fit if a creature has many.
- Attached to the token, so they follow it when it moves; not clickable.
- Drawn per client from synced metadata, so a hidden token's conditions are
  never drawn on a player's screen.
- Custom conditions use the crystal-ball icon.

Icon artwork: Noto Emoji, Apache 2.0 — see `cond/NOTICE.txt`.
