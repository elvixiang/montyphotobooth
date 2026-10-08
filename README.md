# Photo with Monty · MONTY&Co.

Static web camera for outlets & booths: scan QR → camera → Monty appears behind you → photo → save → follow Instagram.
No backend, no login, no API keys. Everything runs in the customer's browser.

## How it works

1. The camera finds the customer's head (face detection).
2. Monty is placed next to the head, behind the shoulder, on the side with more room.
3. The customer's body is cut out of the camera image (person segmentation) and drawn **in front of** Monty,
   so Monty looks like he's standing behind them. The real background stays.
4. Monty follows the head. Customers can still drag / pinch / rotate him; Reset puts him back behind the shoulder.
5. The saved photo (1080×1920, Instagram Story size) is layered the same way, plus the brand frame.

If head tracking can't load (very old phone, blocked network), the app still works: Monty simply appears
in front and can be dragged/pinched by hand.

## Folder structure

```
photo-with-monty/
├── index.html                              ← the whole app (HTML + CSS + JS)
├── README.md
└── assets/
    ├── monty.png                           ← Monty character (transparent PNG)
    ├── logo.png                            ← OPTIONAL white logo for the photo frame
    └── models/
        ├── face_detection_short_range.tflite   ← head detection (MediaPipe, Apache 2.0)
        └── selfie_segmentation.tflite          ← person cut-out (MediaPipe, Apache 2.0)
```

The MediaPipe engine itself (JS + WebAssembly) loads from the jsDelivr CDN, pinned to version 0.10.35.

## Replacing Monty

Put the new PNG at `assets/monty.png` (same file name). Recommended: transparent background,
around 1000 px wide, under ~400 KB.

## Settings (top of the script in index.html → `CONFIG`)

- `logoSrc`: set to `'assets/logo.png'` after adding a logo.
- `instagramHandle`, `instagramUrl`, `frameCaption`.
- `behind`: how big Monty is and how far beside/below the head he stands (in face widths).
- `ai.enabled`: set to `false` to turn head tracking off (Monty always in front, drag by hand).

## Deploy: GitHub → Vercel

1. github.com → New repository → name it `photo-with-monty` → Create.
2. "uploading an existing file" → drag in `index.html`, `README.md` and the whole `assets` folder
   (including `assets/models`) → Commit.
3. vercel.com → Add New… → Project → Import the `photo-with-monty` repo.
4. Framework Preset: **Other**. Leave Build Command and Output Directory empty → Deploy.
5. Open the `https://….vercel.app` link on your phone and test. Make the QR code from this link.

Every new commit to GitHub redeploys automatically. Vercel serves HTTPS, which the camera requires.

## Good to know

- The camera only works on HTTPS (Vercel) or localhost — not when opening the file directly.
- First visit downloads the head-tracking engine (a few MB, cached afterwards). On slow data it can take
  a few seconds before Monty jumps behind the customer; meanwhile a "Finding you…" hint shows.
- Works best with 1–3 people, good lighting, and faces within ~2 m of the camera. Very far faces
  (rear camera, big groups) may not be detected — Monty can then be placed by hand.
- Cut-out edges around very curly/flyaway hair can be slightly soft — normal for real-time cut-out.
- iPhone: "Save to Photos" opens the share sheet → choose "Save Image".
  Fallback: press & hold the photo → "Save to Photos".
- Android: the photo downloads to the gallery/Downloads; "Share" sends it straight to Instagram/WhatsApp.
- In-app browsers (Instagram, LINE, WhatsApp) may block the camera. Customers should scan the QR with
  their phone's camera app, which opens Safari/Chrome.
