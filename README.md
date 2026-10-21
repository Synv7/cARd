# cARd — Josephs Morris

An AR business card for the Bakersfield College Innovation Lab. Point a phone at the
laser-cut card and a glowing circuit lightbulb rises out of the engraved logo and spins,
with eight matte keys under the card: Call, Email, Website, Save contact, LinkedIn,
Instagram, Facebook, YouTube.

## What's in the folder

| File | What it is |
| --- | --- |
| `index.html` | The whole experience (HTML, CSS, JS in one file). |
| `model.glb` | The circuit-lightbulb glTF model (glass, copper traces, filament, orbiting circle, square and diamond halo with a looping animation). |
| `card.jpg` | Photo of the card, used as the texture of the virtual card. |
| `card-target.jpg` | Cropped card image the tracker learns. |

## Three modes

- **3D preview** — works in any modern browser. Drag to orbit, tap a key.
- **Scan my card** — camera image tracking (MindAR). iPhone Safari and Android Chrome.
  The first scan teaches the tracker your card (about 10–30 s); the result is cached on
  the phone, so later scans start right away.
- **Place in room** — WebXR `immersive-ar` with hit-test and DOM overlay. Shows up only on
  browsers that support it (Android Chrome with ARCore, Meta Quest Browser).

## Put it online

The camera only works over HTTPS. Any static host works:

1. **Netlify Drop**: drag this folder onto app.netlify.com/drop.
2. **GitHub Pages**: push the folder to a repo, then Settings → Pages.
3. Your college web space, if it serves HTTPS.

Then put that address in `CONFIG.hostedUrl` (top of the script in `index.html`) and engrave a
QR code to it on the back of the card.

## Edit links, model, or target

Everything lives in `CONFIG` at the top of the `<script type="module">` in `index.html`:

- `keys` — labels, values and links. The four social keys currently point to Bakersfield
  College's official accounts; swap in personal profiles if you prefer.
- `modelUrl` — any glTF/GLB. It is auto-scaled to stand on the logo.
- `target.version` — bump this whenever you replace `card-target.jpg` (for example after a new card design), so phones that scanned the old card relearn the new one.
- `target.mind` — for an instant first scan, compile `card-target.jpg` with MindAR's online
  image-target compiler, save the result as `card.mind` next to `index.html`, and set
  `target: { image: './card-target.jpg', mind: './card.mind' }`.

## Tracking stability

`CONFIG.tracking` controls how steady the overlay is:

- `smoothing` — a One Euro filter on the card's position and rotation. Lower `posCutoff` / `rotCutoff` makes it steadier when you hold still; higher `posBeta` / `rotBeta` makes it keep up during fast moves.
- `cameraFov` — the phone camera's field of view across the long side of the image (most phones: 60–70°). If the bulb seems to lean or slide when you tilt the card, nudge this up or down by 5.
- `holdMs` — how long the overlay stays put when the card is briefly lost.
- `syncFeed` — shows the exact camera frame each pose was measured on, so the overlay never trails the video. The trade: while locked, the camera image updates at the tracker's rate (usually 20–30 fps) instead of 60.

`CONFIG.motion` sets the bulb's turn speed and the halo animation speed.

## Scanning tips

Even light, card fully in frame, phone 20–30 cm away. The engraved text and logo give the
tracker plenty of detail; glare on the wood is the main thing that breaks tracking.

## Credits

three.js r186 (MIT) · MindAR 1.2.5 (MIT) · brand icons from Font Awesome Free 6.7.2 (CC BY 4.0).
