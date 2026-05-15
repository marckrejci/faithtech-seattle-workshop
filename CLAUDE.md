# CLAUDE.md — Build With Intention Workshop

## What this project is

A single-file interactive workshop presentation (`index.html`) for FaithTech Seattle's "Build With Intention: Rapid Prototyping for Purpose-Driven Creators" workshop. Built in React 18 + Babel standalone — no build tools, no bundler. Everything except the LoFi MP3s is embedded directly in the HTML.

## How the build works

`outputs/gen.py` generates `index.html` by:
1. Reading a Python triple-quoted string containing the full HTML/JSX template
2. Reading `outputs/img_b64.json` which holds base64-encoded versions of all images
3. Replacing `PLACEHOLDER_xxx` tokens in the template with the actual base64 data
4. Writing the result to `index.html`

**Critical rule:** always replace longer placeholder names before shorter ones that share a prefix. Example: `PLACEHOLDER_LOGOMARK_4X` must be replaced before `PLACEHOLDER_LOGOMARK`, or the shorter replace will corrupt the longer token.

To rebuild: `cd outputs && python3 gen.py` (writes `index.html` to the project root)

## Adding or updating an image

1. Add the image to `Images/`
2. In a Python script, base64-encode it and add it to `img_b64.json` under a new key
3. Add a `const IMG_xxx = "data:image/png;base64,PLACEHOLDER_xxx";` near the top of the HTML template in `gen.py` (around line 69)
4. Add `html = html.replace("PLACEHOLDER_xxx", imgs["xxx"])` to the replace block at the bottom of `gen.py` — before any shorter keys that share a prefix
5. Reference `{IMG_xxx}` in JSX as needed
6. Run `gen.py`

## Slide architecture

Each slide is a React component. The `SLIDES` array (near the bottom of `gen.py`) maps slide IDs to render functions. Timer slides accept `{ isActive, audio }` props.

Key shared components:
- `Shell` / `SlideShell` — outer container respecting chrome bar heights (`CHROME_TOP=52`, `CHROME_BOTTOM=38`)
- `TimerSlide` — reusable timed slide with loopLabel, title, duration, body, footer
- `DiscernmentPrayerSlide` — dark-bg prayer slide with 1-min silent timer and ambient music
- `useTimerLogic` — shared hook for all countdown timers
- `AudioEngine` — manages LoFi MP3 playback with shuffle, fade, and chime

## Brand constants

```javascript
FT_YELLOW  = "#FFF737"   // Primary brand yellow
FT_BLACK   = "#16160C"   // Near-black
DARK_BG    = "#1A1A0F"   // Dark slide background
TEXT       = "#16160C"
TEXT_MUTED = "rgba(22,22,12,0.5)"
ACCENT_DIM = "rgba(255,247,55,0.28)"  // Muted yellow fill
CHROME_TOP    = 52   // px — fixed top bar height
CHROME_BOTTOM = 38   // px — fixed bottom bar height
```

## File structure

```
index.html               # Final output — do not edit directly
README.md
CLAUDE.md
LoFi Music/              # 17 MP3s by HoliznaCC0 (CC0) — required at runtime
Images/                  # Source assets — baked into index.html at build time.
                         # Not needed to run the site, but keep in the repo so
                         # future rebuilds don't require hunting down originals.
outputs/
  gen.py                 # Generator — edit this to change slides
  img_b64.json           # Base64 cache for all images
Workshop Handout.pdf
ProtoVibing_Workshop_Toolkit.pdf
```

## Unused Images/ files (can be deleted)

These @4x source files are not loaded by gen.py — only the @2x versions are used:
- `SEA-Call-Sign@4x.png`
- `SEA-City-Build@4x.png`
- `SEA-City-Card@4x.png`
- `SEA-City-Sign@4x.png`
- `SEA-Community-Logotype@4x.png`

## Deployment

For GitHub Pages: push `index.html` and `LoFi Music/` to the repo root. Everything else is optional (but keep `Images/` in the repo for rebuild purposes). Enable Pages from Settings > Pages > main branch / root.
