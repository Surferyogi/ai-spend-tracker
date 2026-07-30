# AI Spend Tracker

A single-page PWA for tracking AI subscription costs. Installs to the macOS Dock
and the iOS Home Screen, works fully offline, keeps currencies separate, and
stores everything locally in the browser.

**This repository contains no financial data.** The app ships empty; you load
your own figures on each device via **Data → Import scan (merge)**.

## Why it works this way

Two things drive the design:

- **Currencies are never mixed.** No exchange rate is applied anywhere. SGD and
  USD are reported side by side, because a converted total is a guess dressed up
  as a fact.
- **Every row states its basis.** `verified` means a receipt or an explicit
  confirmation exists. `assumed` means it is a projection at the last known
  price. Both are shown separately in every total, so a projection can never
  quietly inflate a headline number.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app — markup, styles, logic, no dependencies |
| `manifest.json` | Name, icons, standalone display |
| `sw.js` | Service worker; caches the shell for offline use |
| `icon-*.png` | App icons (192, 512, 180 for iOS) |

## Publishing

Settings → Pages → Source: **Deploy from a branch** → `main` → `/ (root)`.
Live at `https://<user>.github.io/<repo>/` in a minute or so.

> GitHub Pages serves from **public** repositories on the free plan. That is why
> this repo carries no data — anything committed here is world-readable.
> `.gitignore` blocks `ai-spend-*.json` so an exported ledger cannot be pushed
> by accident.

## Installing

- **macOS Safari** — Share → Add to Dock
- **macOS Chrome/Edge** — install icon in the address bar
- **iOS** — Safari only: Share → Add to Home Screen

## Data lives on the device

`localStorage` is per-origin, per-device: your Mac and your phone hold separate
ledgers and do not sync. Move data with **Export** → AirDrop → **Import**.
Import is additive and deduplicates on receipt number, then on
date + currency + amount with a per-signature budget, so re-importing the same
file adds nothing while genuinely repeated same-price charges on one day are
all kept.

Export regularly. Clearing site data wipes `localStorage`.

## After editing

Bump `CACHE` in `sw.js` (`ai-spend-v1` → `v2`) whenever `index.html` changes,
or the service worker keeps serving the cached copy and your edit appears not
to have happened.
