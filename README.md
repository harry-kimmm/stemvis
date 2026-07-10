# STEMVIS

Stem-reactive video generator for promoting music on Instagram / TikTok.
Load isolated stems — each drives its own visual layer. Macro knobs morph the
whole composition at once (Serum-macro style), and DICE reseeds the arrangement,
so every render is a genuinely different video with zero timeline editing.

## Run

Open `index.html` in Chrome. No build step, no dependencies — fully self-contained.

Live version (once GitHub Pages is enabled):
`https://<username>.github.io/stemvis/`

## Use

1. Export stems from FL Studio (split mixer tracks, or solo-bounce each bus)
2. Load stems into the eight channels: KICK / BASS / CLAP / HATS / CHORDS / ARPS / VOX / FX
3. Pick a LOOK: **KINETIC** (cut-based type/image shots, default),
   **ORBIT** (abstract visualizer), or **POSTER** (editorial typography).
   Custom .ttf/.otf font files load fully offline.
4. Optionally add center art, a background image, and a title overlay
5. Play, hit LOOP on the hook, jam DICE + macro knobs until it feels right
6. SET IN at the hook, pick 15/30/60s, REC → downloads an mp4 (webm fallback)

Keep the tab visible while recording — capture is real-time.

## Architecture (v1)

- **Web Audio API** — one AnalyserNode per stem → envelope follower,
  transient onset detection, spectral centroid (rough pitch height)
- **Looks (visual modules)** — swappable full visual languages sharing the
  same stem analysis, macro knobs, seed system, and recorder:
  - **KINETIC** — music-video editing language: hard cuts to seeded shots
    (huge type / word stacks / full-bleed duotone art / word walls) every N
    kicks (CHAOS = cut rate) · kick: zoom pump + strobe invert (ENERGY) ·
    bass: row slides · arps: letter scatter · vox: type scale · hats: camera
    stutter · chromatic-ghost on hits
  - **ORBIT** — kick: frame punch + expanding rings · bass: low-end floor glow ·
    clap: frame flash · hats: top-frame shimmer · chords: gradient blob washes ·
    arps: pitch-mapped particle spawns · vox: waveform ring / cover-art pulse ·
    fx: slice-glitch post-process
  - **POSTER** — editorial typography over duotone imagery: kick pops the
    frame · bass breathes the paper · chords wash the headline · arps jitter
    the letters · vox lifts the script quote · hats flicker dust + scratches ·
    user-loadable font files (FontFace API, fully offline)
- **Macro knobs** — ENERGY, SPREAD, CHAOS, HUE, STYLE, DRIFT; each modulates
  many underlying parameters simultaneously
- **Seeded RNG** (mulberry32) — arrangements are reproducible per seed
- **MediaRecorder** — canvas + audio stream capture, 720p/1080p vertical

## Roadmap

- [ ] MIDI-driven mode — sample-accurate note events from FL piano roll exports
      (replaces onset-detection guesswork with exact timing + pitch)
- [ ] Savable looks — knob state + seed as exportable/importable JSON presets
- [ ] Lyric-sync text layer
- [ ] Batch render — generate N seed variations of the same clip region
- [ ] Additional visual modules per role (alternate kick/chord/arp styles)
