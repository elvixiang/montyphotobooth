# Photo with Monty · MONTY&Co.

Static web camera for outlets & booths: scan QR → camera → Monty → photo → save → follow Instagram.
No backend, no login, no API keys. Everything runs in the customer's browser.

## Folder structure

```
photo-with-monty/
├── index.html          ← the whole app (HTML + CSS + JS)
├── README.md
└── assets/
    ├── monty.png       ← Monty character (transparent PNG)
    └── logo.png        ← OPTIONAL white/transparent logo for the photo frame
```

## Replacing Monty

Put the new PNG at `assets/monty.png` (same file name). Recommended: transparent background,
around 1000 px wide, under ~400 KB so it loads fast on mobile data.

## Optional logo in the photo frame

1. Save a white logo with transparent background as `assets/logo.png`.
2. In `index.html`, find `CONFIG` near the top of the script and change
   `logoSrc: null` to `logoSrc: 'assets/logo.png'`.

Other easy settings live in the same `CONFIG` block: Instagram handle/link, frame caption,
Monty's starting position and size, output resolution.

## Deploy: GitHub → Vercel

1. github.com → New repository → name it `photo-with-monty` → Create.
2. "uploading an existing file" → drag in `index.html`, `README.md` and the `assets` folder → Commit.
3. vercel.com → Add New… → Project → Import the `photo-with-monty` repo.
4. Framework Preset: **Other**. Leave Build Command and Output Directory empty → Deploy.
5. Open the `https://….vercel.app` link on your phone and test. Make the QR code from this link.

Every new commit to GitHub redeploys automatically. Vercel serves HTTPS, which the camera requires.

## Good to know

- The camera only works on HTTPS (Vercel) or localhost — not when opening the file directly.
- iPhone: "Save to Photos" opens the share sheet → choose "Save Image".
  Fallback: press & hold the photo → "Save to Photos".
- Android: the photo downloads to the gallery/Downloads; "Share" sends it straight to Instagram/WhatsApp.
- In-app browsers (Instagram, LINE, WhatsApp) may block the camera. Customers should scan the QR with
  their phone's camera app, which opens Safari/Chrome. The app shows an "Open in Safari or Chrome" message
  with a copy-link button when the camera is unavailable.
- Safari may ask for camera permission again on each visit; that is browser behaviour.
