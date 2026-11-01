# recheck

A **client-only** hardware validator for refurbished / second-hand laptops and MacBooks.
Open it in a browser, run the checks, and get a condition report. **Nothing is
uploaded** — the page patches `fetch`/`XMLHttpRequest` and counts any attempted
outbound request (shown live in the header as `Outbound: 0`). It also works fully
offline if opened from `file://`.

## Run it

```bash
npm install
npm run dev        # local dev at http://localhost:4321
npm run build      # static output in dist/
npm run preview    # serve the built site
```

Deploy `dist/` to any static host (GitHub Pages, Cloudflare Pages, Netlify) — no
backend required.

### GitHub Pages (project site)

GitHub Pages serves from a subpath (`https://<user>.github.io/<repo>/`), so the
build must set Astro's `base`. Two options:

1. **Recommended — GitHub Actions:** push the included `.github/workflows/deploy.yml`.
   It auto-detects the repo name and sets `BASE_PATH` for you. Enable Pages in the
   repo settings → "Build and deployment" → Source: "GitHub Actions".
2. **Manual:** build with the repo name as the base, then publish `dist/`:
   ```bash
   BASE_PATH=/your-repo/ npm run build
   ```

`dist/` also contains a `.nojekyll` file so GitHub's Jekyll doesn't ignore the
underscore-prefixed `_astro/` folder. If you deploy with a custom domain or a
user/org page (served at root `/`), build **without** `BASE_PATH`.

## Use it

1. Open the page in **Chrome or Edge** for full coverage. Firefox/Safari still
   work but degrade battery, Bluetooth, USB, multi-monitor, and real-GPU checks
   to "Unsupported" (the page tells you this up front).
2. Fill the **Claim vs Detected** box with what the seller advertised.
3. Run the passive scan (automatic), then the interactive tests (display,
   keyboard, camera, etc.). Deep CPU/battery tests are opt-in and generate heat.
4. Hit **Generate report** → copy, download JSON, or print.

## What it can and cannot verify

| Component | Browser-verifiable | Notes |
|---|---|---|
| **Battery** | Charge level, charging state, discharge-rate capacity proxy (deep) | Health %, cycle count, swelling → OS only |
| **CPU** | Core count, sustained-load thermal-throttle detection, per-core balance | Exact model, clocks, temp → OS only |
| **RAM** | Coarse capacity only (`navigator.deviceMemory`, rounded down & **capped at 8 GB** in Chrome) | Can disprove low claims; cannot confirm 8/16/32 GB |
| **GPU** | Vendor/renderer (real in Chrome), WebGL2, shader benchmark | Exact model → OS only |
| **Display** | Dead pixels, backlight bleed, refresh-rate estimate, touch, HDR | Physical damage → visual |
| **Keyboard / Trackpad** | Key registration, multitouch, pressure | — |
| **Camera / Mic / Speaker** | Live preview, resolution, FPS, level, channels | — |
| **Storage** | Rough read/write speed proxy (Cache API) | Not a native benchmark |
| **Connectivity** | Bluetooth scan, USB devices, multi-monitor, network info | Chromium-only |

All browser results are **not authoritative** — they are browser-reported and can
be spoofed. Treat this as a fast first-pass filter, then confirm the rest in the OS.

## Verify the rest in the OS

These are the things the browser genuinely cannot read. Run them alongside:

**Battery health / cycle count**
- macOS: `system_profiler SPPowerDataType`
- Windows: `powercfg /batteryreport`
- Linux: `upower -d`

**Exact RAM / speed**
- macOS: `sysctl hw.memsize`
- Windows: `wmic memorychip get capacity`
- Linux: `free -h`

**CPU model**
- macOS: `sysctl machdep.cpu`
- Windows: `wmic cpu get name`
- Linux: `lscpu`

**Disk health (SMART)**
- macOS: `smartctl -a /dev/disk0`
- Windows: `wmic diskdrive get status`
- Linux: `smartctl -a /dev/sda`

## Privacy

No analytics, no CDNs, no network calls. Fonts are system fonts. The page can be
saved and opened from a USB stick (`file://` is a secure context, so camera/mic/
battery still work). The header badge proves zero outbound requests.
