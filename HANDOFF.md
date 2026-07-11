# HANDOFF — Gargantua

A quick, human-readable summary of the recent work, the current state, and ideas
for whoever (or whatever) picks this up next. For the technical/agent-facing
guide, see `CLAUDE.md`.

## What this project is

A single-file, offline, kid-friendly interactive black-hole story (`index.html`).
Open it by double-clicking — no install, no server, works offline. Built for one
curious 2nd grader, and recently extended with a series of her own requests.

## What was done in this round

Starting from `Gargantua v1.0`, the following were added (all in `index.html`):

1. **Rainbow accretion disk.** The disk now sweeps a full rainbow that swirls with
   the orbiting gas, with a hot near-white core so it still reads as a black hole,
   plus twinkling gold and silver sprinkles.
2. **A dancing, personified Earth.** A little Earth (oceans, continents, ice caps,
   clouds) with a friendly cartoon face — eyes, a smile, rosy cheeks, eye
   sparkles — that always turns to face you. It orbits Gargantua on a giddy,
   bobbing, swaying path and is fully gravitationally lensed. There is exactly one
   Earth.
3. **Gold/silver dust as a real simulation.** The earlier procedural "sprinkles"
   were replaced with an actual Newtonian-gravity particle sim (velocity Verlet +
   accretion drag). The dust spirals inward and plunges across the horizon, then
   respawns. Rendered as additive points, occluded correctly behind the shadow.
4. **A secret blue crystal treasure.** A faint, faceted, twinkling blue glimmer is
   hidden deep inside the black hole — visible only through rays that fall through
   the horizon. A treasure only she knows is there.
5. **A cinematic zoom-dive into a crystal dimension.** Zoom all the way in and the
   view slowly warps through the horizon (soft white bloom at the midpoint) into a
   calm, vast blue dimension. Zoom back out to return.
6. **Her real gem, enhanced and embedded.** The uploaded photo of her favorite
   crystal was enhanced (cleaner white balance, more clarity, richer blues) and is
   saved as `crystal_enhanced.png`. A background-removed cutout of it is embedded as
   the actual floating gem inside the crystal dimension — it bobs, breathes, and
   shimmers, replacing the earlier procedural shape.

## Current state

- Everything above is implemented and committed. Working tree is clean.
- `index.html` is ~220 KB and remains a single, self-contained, offline file
  (the gem texture is embedded as base64: a JPEG for colour + a small PNG alpha).
- Deliverables in the repo root: `index.html`, `crystal_enhanced.png`,
  `CLAUDE.md`, `HANDOFF.md`, plus the original `README.md` and `backups/`.

## Verification status — please read

- **Static checks only so far.** JS syntax was validated with `node --check`, and
  three focused reviews (GLSL compile-safety, particle physics + WebGL wiring, and
  the dive/zoom logic) found no blocking issues. Two real bugs surfaced by those
  reviews were fixed (dust settling into closed orbits instead of plunging; the
  "Fly closer" preset stranding the user mid-dive).
- **No live render was done in this environment** (no browser was connected), so
  the very first thing to do is open `index.html` and confirm it renders with a
  clean console. Priority visual checks: the rainbow disk, the faced Earth, dust
  falling in, and the zoom-dive showing the real gem.

## Suggested next steps / open ideas

- **Confirm the live render** and tune to taste: sprinkle density, dust fall speed
  and count (`NP`), Earth's dance amplitude/speed, dive threshold and pacing.
- **Gem cutout polish.** The background removal leaves a couple of faint edge wisps
  at the lower-right of the gem; the dimension's glow mostly hides them, but a
  cleaner matte (or a hand-touched alpha) would be nicer.
- **Tie the crystal dimension into the story.** Right now the dive is a free
  easter egg; it could become a narrated chapter ("the treasure only you know
  about") with the robot teacher.
- **Perf on low-end devices.** The ray marcher is heavy; the dust points and the
  dimension add a little more. The existing Quality setting caps raymarch cost —
  verify the additions are fine on a school laptop / tablet.
- **Accessibility / reduced motion.** Consider honoring `prefers-reduced-motion`
  for the dance and dive for motion-sensitive kids.

## Constraints to preserve

- Keep it a **single offline `index.html`** — no build step, no external
  URLs/packages, inline everything.
- Keep it **kid-safe, forgiving, and delightful**; the whole story should be
  completable with zero required button presses.
