# Gargantua — My Black Hole

An interactive, narrated space-story that teaches gravitational time dilation
to kids, inspired by *Interstellar*. Built as a **single HTML file** with zero
build steps — double-click `index.html` and fly.

## The story

A signal arrives from another galaxy. You promise your best friend on Earth
you'll come back, then fly: Earth → Saturn → through the wormhole → Mann's
frozen world → Miller's ocean world → survive Gargantua's pull → orbit the
black hole → land on the waves → fly home. The ending computes the real cost
of your journey from your own play: *"You are still 8.0. Your friend is now
14.3."*

## Features

- **3D black hole** — GPU ray-marched gravitational lensing (bent photon
  geodesics), Doppler-beamed accretion disk, clickable Miller's planet
- **Journey map** — pilotable ship (tap, drag, or arrow keys), story stops,
  wormhole jump, warped spacetime fabric, orbital accretion dust, predicted
  drift trail, point of no return
- **Real physics** — Newton's first law (no space drag; braking is a visible
  retro-burn), inverse-square gravity, slingshots, real time-dilation formula
  1/√(1−rs/r) with movie-canon Kerr values at Miller's planet
- **Robot teacher** — 11-chapter guided story with target beacons, spoken
  lessons, progress stars, stuck-detection reminders (toggleable)
- **Voice** — neural TTS narration in-browser (Kokoro-82M via WebGPU/WASM,
  cached after first load; falls back to system voice), plus voice commands
  ("go to Saturn", "orbit", "stop")
- **Sonified telemetry** — space is silent, so the ship sings its data:
  thrust rumble, speed wind, gravity danger drone, time-warp glide tone
- **Dramatic events** — synchronized confetti/shockwave/shake/fanfare on every
  milestone; falling in triggers slow motion (the drama *is* the lesson)
- **Kid-friendly UI** — magnetic click targets, hover assist rings, click
  ripples, big cursor, no required buttons: cards read themselves and close
  themselves; the whole story completes with zero button presses

## Controls

| Input | Action |
|---|---|
| Tap / drag on map | Fly the ship (planets are magnetic) |
| Arrow keys | Manual thrust / turn / retro-burn |
| Drag in 3D view | Look around |
| Scroll / zoom buttons | Fly closer or farther |
| Click blue planet (3D) | Land on Miller's planet |
| Mic button + speech | "go to Saturn", "orbit", "fly home", "stop", "help" |

Settings (top-right): Journey/Black hole view, Gravity visuals, Read aloud,
Voice control, Sounds, Teacher, and (bottom-left in 3D) render Quality.

## Tech notes

- One file, no dependencies, works offline (neural voice needs internet once
  to download the model, then it's cached).
- WebGL2 fragment-shader ray marcher (320 steps, adaptive) for the black hole.
- 2D canvas for the journey map, Web Audio API for all synthesized sound.
- Progress (story chapter, quality) persists in localStorage.
- `backups/` holds a pre-refactor snapshot.

Made by Anthony & Aside for one very curious 2nd grader. 🚀
