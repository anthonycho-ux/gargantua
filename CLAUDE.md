# CLAUDE.md — Gargantua

Context for an AI coding agent working on this project. Read this before editing.

## What this is

An interactive, narrated space-story that teaches gravitational time dilation to
kids (inspired by *Interstellar*). It is a **single, self-contained `index.html`**
with **zero build steps and no dependencies** — you open it by double-clicking the
file, and it works offline. Keep it that way: do not add a bundler, npm packages,
external script/style/image URLs, or split the app into multiple files. Anything
you add (including images) must be inlined (e.g. base64 data URIs).

Audience is a 2nd grader, so changes should stay kid-friendly, forgiving, and
visually delightful. This repo has been co-developed feature-by-feature at the
request of the owner's daughter — treat whimsical requests literally and lovingly.

## How to run / verify

- **Run:** open `index.html` in a modern browser (needs WebGL2). No server needed,
  but a local static server also works.
- **There is no test suite.** After editing, verify by:
  1. `node --check` on the inline `<script>` bodies for JS syntax (extract them
     first — see "Gotchas"). There are two script blocks.
  2. Loading the page and watching the DevTools console for shader
     compile/link errors (they are thrown from `compile()` / program linking).
  3. Visually confirming the black-hole view and the journey map both render.
- `backups/` holds a pre-refactor snapshot. Do not edit it.

## Architecture (all inside `index.html`)

Two visual modes, switched by `window.MODE` (`'journey'` or `'blackhole'`) via
`setMode(m)` (~line 1184). The render loop `frame()` (~line 874) early-returns for
journey mode before the WebGL draw.

### The black hole — a WebGL2 fullscreen ray marcher

- Drawn as a single fullscreen triangle (`VERT` ~line 239 uses `gl_VertexID`, no
  vertex buffers). All the imagery is in the fragment shader `FRAG` (~line 244).
- `FRAG` main loop (`void main()` ~line 452) integrates bent photon geodesics
  (`acc = -1.5 h^2 r / |r|^5`), and at each step tests: capture by the horizon
  (`RS`), hit on Miller's planet (`uPlanet`), hit on the dancing Earth (`uEarth`),
  and a crossing of the accretion-disk plane.
- Constants near the top of `FRAG`: `RS=1.0` (horizon), `RIN=2.35`, `ROUT=11.0`
  (disk edges), `STEPS=320`.
- Key shader functions: `hash12`/`hash13`/`noise2`/`fbm` (noise), `stars()`,
  `disk()` (rainbow accretion disk + gold/silver sprinkles), `shadePlanet()`
  (Miller's ocean world), `shadeEarth()` (the personified Earth with a face),
  `crystalDim()` (the crystal dimension — samples the real gem photo).
- Uniforms are looked up into the `U = {...}` object (~line 595) and set at the top
  of `frame()`. IMPORTANT: `gl.uniform*` acts on the **currently bound program**;
  the `U.*` sets happen while `prog` is active, then the loop switches to the
  points program and switches back.

### Gold/silver dust — a real particle simulation (JS + a 2nd GL program)

- Physics in JS: `spawnGrain` / `stepParticles` / `buildParticleBuffer`
  (~lines 664–724). Each grain is a test particle under Newtonian gravity
  `a = -GM r/|r|^3` (`GM=6.0`), integrated with **velocity Verlet** plus extra
  substeps near the hole, plus a small **accretion drag** so grains spiral in and
  plunge the horizon, then respawn. `NP=110` grains.
- Rendering: a second tiny program (`PT_VERT`/`PT_FRAG` ~lines 726–757) draws the
  grains as **additive `GL_POINTS`** (`blendFunc(ONE, ONE)`) over the ray-marched
  scene, via a VAO with an interleaved buffer (stride 20 bytes: pos vec3 @0,
  seed float @12, alpha float @16). The point vertex shader **reprojects world
  points using the exact same pinhole camera basis as `FRAG`** (`fw/rt/up`, focal
  `1.35`, aspect `uRes.x/uRes.y`). If you change the ray-march camera, update this
  projection too. Shadow occlusion is computed CPU-side (impact parameter vs.
  `b_crit ≈ 2.6·RS`).

### The dive → crystal dimension

- Zooming in past a threshold sets `dive`→1 (hysteresis on `tDist`: in `< 5.2`,
  out `> 7.2`), eased into `uDiveVal` and passed as uniform `uDive`. In `FRAG`,
  `uDive>0.001` crossfades `crystalDim(uv)` over the scene with a white bloom at
  the midpoint. The dive is gated to blackhole mode so journey controls can't
  trigger it. Preset `vClose` sits at `7.5` (above the out-threshold) on purpose.
- `crystalDim()` renders the **real crystal photo** as a luminous, gently
  bobbing/breathing billboard with a halo, travelling facet shimmer, and gold
  motes. The photo is uniform `uCrystal`.

### The embedded gem texture

- The gem is stored as two data URIs: `CRYSTAL_RGB` (JPEG, colour premultiplied on
  black) and `CRYSTAL_A` (PNG, grayscale alpha), ~line 605. `loadCrystal()`
  (~line 616) draws both to canvases, **un-premultiplies** colour by alpha, and
  uploads a combined RGBA `ImageData` to the `crystalTex` texture (unit 0). Texture
  orientation: `UNPACK_FLIP_Y_WEBGL=false`, and `crystalDim` flips via
  `tuv.y = -0.5*gp.y + 0.5`. Sampled with `textureLod(..., 0.0)` because the fetch
  is inside conditional flow.
- Source assets live in the repo root: `crystal_enhanced.png` (the full-res
  enhanced photo). To regenerate the texture, cut out the gem (key on blue-channel
  dominance `B - max(R,G)` plus saturation, largest connected blob, fill holes,
  feather), premultiply onto black → JPEG, alpha → PNG, base64, and replace the two
  `CRYSTAL_*` consts.

### The journey map (2D canvas)

- The pilotable-ship map, story stops, clocks, and the 11-chapter guided lesson are
  plain 2D-canvas + DOM code lower in the file. Time-dilation uses the real
  `1/√(1−rs/r)` formula with movie-canon Kerr values at Miller's planet. Search
  `window.MODE`, `setMode`, `presets`, and the lesson array for entry points.

## Conventions

- Vanilla ES6+, no framework. 2-space indent. Comments explain the *physics/intent*,
  not the syntax — match that style.
- Prefer editing the shader strings in place; keep GLSL ES 3.00 (`#version 300 es`,
  `texture()`/`textureLod()`, `in/out`).
- Progress (story chapter, render quality) persists in `localStorage`.
- Keep it a single offline file. Keep it kid-safe and forgiving.

## Gotchas

- `index.html` is large (~220 KB) mostly because of the embedded gem data URIs on a
  couple of very long lines. Line-based tools may choke; slice by character range if
  needed.
- GLSL errors surface only at runtime in the browser console — there's no static
  shader linter wired up.
- When adding a lensed object, mirror the existing sphere-intersection blocks in the
  `FRAG` main loop (see Miller's planet / Earth), and remember additive points are
  *not* lensed.
