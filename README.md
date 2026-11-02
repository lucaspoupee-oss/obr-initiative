# Barovian Initiative — Owlbear Rodeo Extension

Gothic initiative tracker with on-board status overlays and separate GM / player views.

## Files

```
manifest.json   OBR extension manifest (action popover 430×700)
drop.png        Bloodied overlay icon (240×240, drawn on the board)
skull.png       Dead overlay icon (240×240, drawn on the board)
index.html      Entire app (UI + logic, OBR SDK loaded from jsdelivr CDN)
icon.svg        Action icon
add.svg         Context menu icon "Add to initiative"
remove.svg      Context menu icon "Remove from initiative"
```

## Deploy (Cloudflare Pages — same as your other extensions)

1. Push this folder as a Pages project (or drag-and-drop deploy). No build step, no framework.
2. In Owlbear Rodeo → **Extensions → Add Custom Extension**, paste:
   `https://<your-project>.pages.dev/manifest.json`
3. Enable it in the room. The action panel loads in the background for every client,
   which is what registers the context menu (GM) and draws the board overlays (all clients).

## How it works

- **Add tokens**: GM selects one or more tokens (CHARACTER or MOUNT layer) → context menu →
  *Add to initiative*. Same menu shows *Remove from initiative* for tracked tokens.
- **Per-entry data** (portrait, editable name, initiative, 🩸 Bloodied, 💀 Dead, free-text note,
  benched flag) is stored in **item metadata** under `eu.barovia.initiative/entry` — so it syncs
  natively, survives reloads with the scene, and vanishes automatically if the token is deleted.
- **Round / active token / sort direction** live in **scene metadata** (`eu.barovia.initiative/state`).
- **Next ▶** advances the turn; wrapping past the last combatant increments the round.
- **↓/↑ Init** toggles descending/ascending sort (entries without a value sink to the bottom).
- **↻ menu**: (1) reset all initiative values + round → 1, keep the roster;
  (2) clear the roster entirely + round → 1.
- **Set aside (⤓)** moves an entry to the *Out of combat* section at the bottom — out of the turn
  order but with all its data kept. **⤒** brings it back.
- **Board overlays** are OBR *local items* rebuilt per client from the synced metadata:
  - Active token: four red corner brackets sized to the token, pulsing (animated locally,
    zero network traffic).
  - Bloodied: glowing red teardrop, ½ of token width/height, top-right quadrant.
  - Dead: glowing red skull, same size and placement (takes priority over the teardrop).
  Because they are attachments, they follow the token when it moves.
- **Player view**: read-only list, no GM controls, and any token with **Hidden** state on the map
  is completely omitted — from the list *and* from that player's overlays. Notes are visible to
  players (only when non-empty); keep secrets out of them or tell me and I'll make notes GM-only.

## Known limitations

- Corner brackets and status icons ignore token **rotation** (they stay axis-aligned).
- If both Dead and Bloodied are checked, only the skull is shown.
- Turn order (GM side) still includes hidden tokens — players simply don't see them; when a hidden
  token is active, players see no highlight for that turn.
