# Build With Intention
### Rapid Prototyping for Purpose-Driven Creators
*A FaithTech Seattle Workshop Presentation*

A single-file interactive workshop presentation built in React (Babel standalone). No build step required — open `index.html` in any modern browser.

---

## Using the Presentation

- **Navigate** with arrow keys or the on-screen buttons in the bottom chrome bar
- **Fullscreen** via the icon in the top-right corner (or press F11)
- **Timers** start manually on each timed slide — press Start when the room is ready
- **Ambient music** plays during discernment slides — use the mute toggle if needed
- **Team presentations** (slide 25) — type a team name to enable the Start button; input hides while the clock runs and reappears when time is up

The LoFi Music folder must sit alongside `Presentation.html` for ambient audio to work.

---

## Project Structure

```
index.html               # The entire presentation — self-contained single file
LoFi Music/              # 17 ambient MP3 tracks (required at runtime)
Images/                  # Source assets — embedded as base64 at build time
                         # Not needed to run the presentation, but worth keeping
                         # in the repo so you can rebuild after image changes
Workshop Handout.pdf     # Printable participant handout
ProtoVibing_Workshop_Toolkit.pdf
outputs/                 # Build tooling (not needed for deployment)
  gen.py                 # Generator script — rebuilds index.html from template
  img_b64.json           # Cached base64 image data
```

---

## Rebuilding from Source

All images are embedded as base64 at build time. To regenerate `index.html` after changing images or code:

```bash
# Requires Python 3 + pillow + qrcode
pip install pillow qrcode[pil]

cd outputs
python3 gen.py
```

To update a source image, re-encode it into `img_b64.json` and re-run `gen.py`. See `CLAUDE.md` for full details.

> **Note on Images/:** The `Images/` folder contains the original source assets that get baked into the HTML at build time. The deployed site does not read from this folder at runtime, but you should keep it in the repo so future rebuilds are possible without hunting down the originals.

---

## Deploying to GitHub Pages

1. Push the repo to GitHub — include `index.html` and the `LoFi Music/` folder
2. Go to **Settings > Pages**, set source to `main` branch, root directory
3. Your presentation will be live at `https://[username].github.io/[repo-name]/`

The `Images/` and `outputs/` folders are not needed at runtime — everything visual is baked into `index.html`. That said, keeping `Images/` in the repo is recommended so you can rebuild in the future without needing to recover the originals.

---

## Music Attribution

Ambient tracks by **HoliznaCC0**, released under CC0 (Public Domain).  
Available via [Free Music Archive](https://freemusicarchive.org).

---

## License

Workshop content and design by Marc Krejci / FaithTech Seattle.  
Ambient music: CC0 Public Domain (HoliznaCC0).
