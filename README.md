# floating-site

Public one-page site and beta download host for **FLOATING** — a capture-first
memory instrument by Fløa (AU / VST3).

Live: <https://floamusic.github.io/floating-site/>

## Contents

| Path | What it is |
| --- | --- |
| `index.html` | Home — what it does, the story, downloads, support, contact |
| `install-macos.html` | macOS beta install guide |
| `install-windows.html` | Windows beta install guide |
| `updates.html` | Update check — **the plug-in's corner menu opens this page**, see below |
| `changelog.html` | Release notes, read from the GitHub API at load |
| `assets/css/site.css` | Single stylesheet; palette lifted from the plug-in's `kDark` |
| `assets/fonts/` | ShareTechMono (SIL Open Font License, included) |
| `assets/img/` | Interface screenshots, web-optimised |
| `assets/video/` | Hero demo video, transcoded for web (see below) |

Static HTML and CSS only — no build step and no framework. The font is
self-hosted, and nothing is loaded from a CDN.

Four pages do call one third-party endpoint: `api.github.com`, to read the
latest release. Everything those calls affect is published in the HTML first
and only overwritten on success, so a blocked, failed or rate-limited request
leaves a correct page rather than a blank one — and the pages still work with
JavaScript off.

## Screenshots

`assets/img/hero-memory*.jpg` is `10-remembered.png` from the product repo's
manual render harness (`FloatingSmokeTest --manual-shots <dir> 3`), resized with
`sips -Z 1920` / `-Z 2560` at quality 86 / 82. The harness renders the real
editor through the shipping processor path with the footer version neutralised,
which is why no screenshot here carries a version number to go stale.

The harness lives in the private product repo, so refreshing a screenshot is a
render-then-copy, not something this repo can build.

## Hero video

The hero has two tabs: the interface still (default) and the demo video. The
video **never autoplays** — it carries real audio as part of the demonstration —
and is `preload="none"`, so it costs zero bytes until someone presses play. The
still doubles as the video's `poster`, so switching tabs shows the same frame
rather than a black box.

`assets/video/floating-promo-v3-captions.mp4` is a web transcode of the delivery
master (`video/floating-promo-v3-captions.mp4` in the product repo): same
1920×1080 and same 43.4 s, re-encoded from 13.8 Mbps to 2.2 Mbps H.264 + 128
kbps AAC, which takes it from 73 MB to 11 MB with the caption overlays still
crisp. `moov` is written ahead of `mdat` so playback can start before the file
finishes downloading. Re-transcode from the master rather than from this file.

## Downloads

Binaries are **not** in this repo. They are attached to the
[`v0.1.0-beta.1` release](https://github.com/floamusic/floating-site/releases/tag/v0.1.0-beta.1),
and the download buttons link straight at those assets. `SHA256SUMS.txt` on the
release lets anyone verify what they downloaded.

## The update check

FLOATING's corner menu has a **Check for Updates** item. It makes no network
request of its own: it opens

```
https://floamusic.github.io/floating-site/updates.html?v=<running version>
```

and `updates.html` does the comparison against the latest release. The plug-in
sends nothing but the version it is running.

⚠ **The path is hardcoded in the plug-in.** `updates.html` cannot be renamed,
moved, or deleted without shipping a new plug-in build — every already-installed
copy points here. The two repositories have no build-time link, so nothing would
catch the break except a user clicking the menu item.

The page reads the current release **live from the GitHub Releases API**, and
falls back to the `data-latest-version` value baked into the markup when that
request fails or JavaScript is off. That baked value is the same one the other
pages carry and is refreshed at release time; live is primary because a baked
number goes stale in silence.

## Note on this repo

This repo holds only the public site. The FLOATING source is private and lives
elsewhere; nothing here is generated from it automatically.
