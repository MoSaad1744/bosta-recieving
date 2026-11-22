# Bosta | Receiving information (Bosta Dock)

Dock live operation app: vehicle check-in (Pickup / Line haul), yard queue, docks, dashboard, search & archive, team chat.

```
 Phone (Android app / PWA)  ──HTTPS──►  GitHub Pages (index.html)  ──POST {fn,args}──►  Apps Script /exec (doPost)  ──►  Google Sheet
```

| Part | Where | What |
|---|---|---|
| Web app | `index.html`, `config.js`, `sw.js`, `manifest.webmanifest`, `icons/` | Published to **GitHub Pages** by `.github/workflows/pages.yml` |
| Android app | `capacitor.config.json`, `android-res/`, `.github/workflows/android.yml` | APK built by GitHub Actions → release **android-latest** |
| Server | `server/Code.gs`, `server/Index.html` | Paste into the Apps Script project bound to the Sheet |

## Updating

- **Change the UI:** edit `server/Index.html`, run `python3 tools/build_web.py` (rebuilds `index.html`), commit & push. Pages updates in ~1 minute, and the Android app picks it up on next open (it loads the Pages site).
- **Change the server:** paste `server/Code.gs` into Apps Script → Deploy → Manage deployments → Edit → **New version**. The `/exec` URL stays the same.
- **API URL:** `config.js` → `window.BOSTA_API = 'https://script.google.com/macros/s/…/exec'`.

## Apps Script deploy settings (required)

- Execute as: **Me**
- Who has access: **Anyone** (the app has its own username/password login; "Anyone within Bosta" blocks calls from GitHub Pages / the app)

Only the functions listed in `doPost` are callable from outside (`setup`, `resetAdmin`… are not).

## Install on phones

- **Android:** open the release page → download `BostaDock.apk` → install (allow *Install unknown apps*).
- **iPhone / any phone:** open the Pages link in Safari/Chrome → Share → **Add to Home Screen**.
