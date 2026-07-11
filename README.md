# STEMVIS

Kinetic stem-reactive video generator for promoting music on Instagram / TikTok.
Load isolated stems and the visual hard-cuts between seeded "shots" like a music
video edit — huge type, word stacks, duotone imagery, datamosh — all driven by
the stems themselves. Timed lyric lines drive their own cuts. Macro knobs morph
the whole language at once, DICE reseeds the shot sequence, so every render is
a genuinely different video with zero timeline editing.

## Run

Open `index.html` in Chrome. No build step, no dependencies — fully self-contained.

Live version (once GitHub Pages is enabled):
`https://<username>.github.io/stemvis/`

## Use

1. Export **full-length, aligned** stems from FL Studio (File → Export →
   split mixer tracks — trimmed solo-bounces lose arrangement timing)
2. Pick a channel template (8 STEMS / 4-TRACK / 1 FILE) and load audio;
   each strip's role selector decides what that audio drives visually
   (PUNCH / GROOVE / SNAP / TICK / WASH / SPARK / VOICE / BREAK / MIX / OFF)
3. Add assets: IMAGE A/B (become full-bleed duotone shots), FONT A/B
   (your own .ttf/.otf, loads fully offline), WORDS (cuts pick from these)
4. Lyrics: paste lines → **TAP SYNC** → play the song and press ENTER as each
   line starts (one playthrough), or import a timestamped `.lrc` file.
   Timed lines force cuts with their text. Words/lyrics persist in localStorage.
5. Play, hit LOOP on the hook, jam DICE + macro knobs until it feels right
6. SET IN at the hook, pick 15/30/60s, REC → downloads an mp4 (webm fallback)

Keep the tab visible while recording — capture is real-time.

## Architecture

- **Web Audio API** — one AnalyserNode per stem → running-peak-normalised
  envelope follower, relative-jump onset detection, spectral centroid
- **Cut engine** — the composition hard-cuts to a new seeded shot every N
  kicks (CUTS knob: every 8 → every kick), on every timed lyric line, and
  whenever the FX stem hits; each cut lands with a 2-frame invert
- **Shot modes** — type (huge auto-fit word, per-letter render) · stack
  (word rows fill the frame, hot row rotates) · art (full-bleed duotone
  image + label strip) · wall (word grid with hot cell) · icon (big vector
  glyph + orbiting satellites) · iconField (icon grid with hot cell)
- **Filler pool** — gaps between timed lyric lines cut to icon shots /
  WORDS / images (never stray lyric text); the icon chips curate the pool.
  Each pool entry is a self-contained painter — the plug-point for richer
  filler scenes later
- **Per-stem reactions** — kick: zoom pump + strobe invert · bass: warp
  slice-melt + row slides · clap: border + frame flash · hats: camera
  stutter + ink specks · chords: background colour breathing · arps:
  per-letter ripple · vox: type swell + expanding word-echo rings ·
  fx: glitch storm + forced cut
- **Themes** — named aesthetics (MONO bold b&w · OCCULT glowing halo +
  serif · PRINT halftone colour fields). A theme
  bundles palette recipe, image treatment, type voice, texture, motion
  feel, and shot-mode weights; DICE + knobs vary *within* the theme, so
  re-running the same song under a different theme yields a different
  video. New aesthetics are added as THEMES entries — the main growth
  surface.
- **Macro knobs** — ENERGY (punch/pump/strobe), CUTS (cut rate), WARP
  (liquid slice displacement), SCRAMBLE (datamosh blocks + glyph
  corruption + glitch floor), HUE (palette/duotone), STYLE (morphs the
  theme palette), SIZE (type + icon scale)
- **Seeded RNG** (mulberry32) — the whole shot sequence is reproducible per seed
- **MediaRecorder** — canvas + audio stream capture, 720p/1080p vertical

## Roadmap

- [ ] MIDI-driven mode — sample-accurate note events from FL piano roll exports
      (replaces onset-detection guesswork with exact timing + pitch)
- [x] Lyric-sync text layer — tap-sync + .lrc import (auto-transcription via
      local Whisper → .lrc is a documented workflow, not in-app)
- [ ] Word-level lyric timing (karaoke-style reveals within a line)
- [ ] Savable looks — knob state + seed + lyric times as export/import JSON presets
- [ ] Batch render — generate N seed variations of the same clip region
- [ ] More shot modes (split-frame, image-punch collage, outline storm)
- [ ] Offline renderer — step the song frame-by-frame off the clock and
      encode losslessly (WebCodecs); removes realtime-capture frame drops
      entirely. This is the "scale up" milestone.
