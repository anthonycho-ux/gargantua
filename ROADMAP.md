# ROADMAP — Gargantua

Staged from a planning "blitz" session. Small, incremental releases.
Sequencing thesis: **fix → delight → meaning → refine.**

## Locked decisions (from the session)

- **Audience:** just the owner's daughter (not public). So we skip public-share,
  broad accessibility, and heavy mobile work.
- **Device:** laptop / desktop (mouse + keyboard). No touch pass needed; v1.4 stays light.
- **The treasure stays pure magic** — no lesson attached to the chest/dive.
  Teaching goes to the real science instead.
- **Appetite:** small increments. Bigger ideas get phased.

## Shipped — v1.0 → v1.0.x

Rainbow accretion disk; personified dancing Earth (with a face); real
Newtonian-gravity dust that spirals in and plunges the horizon; a secret blue
crystal inside the hole; cinematic zoom-dive into a crystal dimension; her real
enhanced gem embedded as the treasure; an interactive treasure-chest reveal
(drag the key) inside the dive; and a UI declutter (gear menu, one panel per
mode, idle auto-fade). All static-checked only — see v1.1.

## v1.1 — "Make it real" (polish, do first)

The base is unverified — everything so far is static-checked, never seen live.
Can't build on that.

- Run it in a real browser; watch the console for shader/JS errors.
- Screenshot the black-hole view, the dive, and the chest reveal; fix what's broken.
- Add her name to the treasure moment (e.g. "You found it, <name>!").
- Clear the stale git `.lock` files, then commit everything cleanly.

**Done when:** it renders correctly end-to-end on her laptop and history is committed.
**Needs:** a browser (connect the Chrome extension, or the owner opens it and reports).

## v1.2a — "Rotating treasures" (whimsy, small)

The existing chest reveals a *different* one of her real objects each dive.

**Done when:** 3+ of her things rotate as surprises in the chest.
**Needs:** 2-3 photos of her things (same as the crystal HEIC).

## v1.2b — "The hunt" (whimsy, small, follows 1.2a)

Add a couple of hiding spots beyond the chest (e.g. behind Saturn, in the
wormhole) so treasures are found in different places — a real scavenger hunt.

**Done when:** at least two non-chest hiding spots work.

## v1.3 — "Why does it happen?" (teaching)

Treasure stays pure magic, so the teaching is the real science.

- One clear, kid-simple "why does time slow near gravity?" beat.
- Optional: a small visual of light bending around the hole.

**Done when:** she can watch/hear one clear explanation of the time-slow effect.

## v1.4 — "Feels nice" (craft, light)

- Honor `prefers-reduced-motion` for the dance/dive.
- Small performance pass so it stays smooth on her laptop.

## Candidate backlog (not yet scheduled)

- **Physics-based palette:** re-derive the disk colors from real temperature /
  Doppler shift instead of a literal rainbow, so the color *emerges from the
  system* (from a generative/computational-art design review). Keeps the magic
  but makes the beauty more "honest." Good candidate to slot near v1.3/v1.4.
- Gentle music / lullaby toggle (hers to switch on).
- The Earth character reacts (cheers at planets, gasps near the hole).
- Chest sparkles echoing into the dust field.

## Constraints to preserve

Single offline `index.html`, no build step, inline everything. Kid-safe,
forgiving, delightful. The whole story completes with zero required button presses.
