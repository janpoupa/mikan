# Ant Survival — AR game (Meta Quest 3)

A WebXR augmented‑reality survival game built with plain HTML + [A‑Frame](https://aframe.io/).
The game renders as an **overlay on the Quest 3 passthrough cameras**: crawling "ants"
(the `mikan` sprite) spawn around you in the real room and crawl toward you. Rotate to face
them and shoot before they reach you. Survive as long as you can.

## How to play

- **Trigger** (right controller) — shoot. Also starts / restarts the game.
- The gun is mounted on your **right hand**, not in the centre of the screen.
- You have **3 lives**. When an ant reaches you, you lose one (red flash). At 0 lives the game ends.
- **Ammo** is capped at **10** and refills **+1 per second**, so you can't spam — aim deliberately.
- The gun has a **limited range** (~11 m); distant ants must come closer before you can hit them.
- **Score** = ants killed. It is shown on the floating HUD together with lives and ammo.

### Deterministic waves
Spawn timing, spawn angles and ant speeds all come from a **fixed seed** (`CONFIG.SEED`).
Every playthrough is therefore identical, so your score at a given elapsed time is directly
comparable between runs. Ants spawn gradually faster over time (see `CONFIG.spawnInterval`).

## Running it

WebXR requires **HTTPS** (or `localhost`). Serve the folder and open it in the Quest 3 browser.

### Quick local server
```bash
# Python 3
python -m http.server 8080
```
Then on a PC browser open `http://localhost:8080` to test with mouse + WASD.

### On the Quest 3 (needs HTTPS)
The headset browser will only enter immersive‑AR from a secure origin. Easiest options:

- **ngrok / cloudflared tunnel** to your local server:
  ```bash
  npx http-server -p 8080
  npx cloudflared tunnel --url http://localhost:8080
  ```
  Open the generated `https://…` URL in the Quest browser, then tap **ENTER AR**.
- **Or** host the folder on any static HTTPS host (GitHub Pages, Netlify, Vercel).

Tap **ENTER AR** in the headset (button appears when immersive‑AR is supported). Grant the
passthrough/camera permission and the game world is overlaid on your room.

## Desktop testing (no headset)
Just open the page over `http://localhost`:
- **Drag** the mouse to look around, **WASD** to move.
- **Click** or press **Space** to shoot (ray from the camera centre).

## Files
- `index.html` — the whole game (A‑Frame scene + components).
- `assets/mikan.png`, `assets/mikan2.png` — the two crawl frames used for the walk animation.

## Tuning
All gameplay knobs live in the `CONFIG` object at the top of the `<script>` in `index.html`:
ammo, reload rate, lives, gun range, aim‑assist cone, spawn distances, ant speed range and the
`spawnInterval(t)` difficulty curve.
