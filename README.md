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
self-hosted. The home page takes its images and film from jsDelivr's copy of this
repo, pinned to a commit, with the site itself as the fallback (see Loading below);
nothing else comes from a CDN.

Four pages do call one third-party endpoint: `api.github.com`, to read the
latest release. Everything those calls affect is published in the HTML first
and only overwritten on success, so a blocked, failed or rate-limited request
leaves a correct page rather than a blank one — and the pages still work with
JavaScript off. The home and install pages keep the answer for an hour in
`localStorage` (`floa.floatingTag`, shared with the Fløa showcase on the same
origin) and never show a version older than the one in their markup;
`updates.html` and `changelog.html` always ask afresh.

## Screenshots

`assets/img/hero-memory*.jpg` is `hero-sync-curtain-3.png` from the product
repo's promo render harness (`FloatingSmokeTest --promo-shots <dir> 3`) — the
panel three frames into a FREE → SYNC switch, the same frame the v0.4.0
announcement images use — resized with `sips -Z 1920` / `-Z 2560` at quality
86 / 82. The harness renders the real
editor through the shipping processor path with the footer version neutralised,
which is why no screenshot here carries a version number to go stale.

The harness lives in the private product repo, so refreshing a screenshot is a
render-then-copy, not something this repo can build.

## Hero video

The hero has two tabs: the interface still (default) and the demo video. The
video **never autoplays** — it carries real audio as part of the demonstration —
and is `preload="none"`, so it costs zero bytes until someone presses play. The
still doubles as the video's `poster`, so switching tabs shows the same frame
rather than a black box. The poster (336 KB) is fetched only when someone points at,
focuses or chooses the film's tab, so a visit that never opens the film doesn't pay for it.

`assets/video/floating-promo-v3-captions.mp4` is a web transcode of the delivery
master (`video/floating-promo-v3-captions.mp4` in the product repo): same
1920×1080 and same 43.4 s, re-encoded from 13.8 Mbps to 2.2 Mbps H.264 + 128
kbps AAC, which takes it from 73 MB to 11 MB with the caption overlays still
crisp. `moov` is written ahead of `mdat` so playback can start before the file
finishes downloading. Re-transcode from the master rather than from this file.

## Downloads

Binaries are **not** in this repo. They are attached to each
[GitHub release](https://github.com/floamusic/floating-site/releases), and the download
buttons link through `/releases/latest/download/` to the newest one's assets. `SHA256SUMS.txt` on the
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

### v0.5.0 refresh — the cassette

- `assets/img/look-{cassette,classic}[@2x].jpg`: one held state painted in both looks by
  `FloatingSmokeTest --site-shots <dir> 3` (product repo), so the home page's Cassette | Classic switch changes
  only the face. Choir at GLOW, Atmosphere 55, FREE; the classic footer is neutralised and the cassette label's
  version box is covered, so neither carries a version. Resized with `sips -Z 1920` (q86) and full 2880 (q80).
- `assets/img/look-{cassette,classic}[@2x].avif`: the same two shots as AVIF (Pillow, quality 78, 4:4:4; at 60 the
  plastic's grain smooths away), 5–8 times lighter than the JPEGs, which stay as the fallback.
- `assets/img/cassette-hero-wide-{1600,2560,3840}.{avif,jpg}` and `cassette-hero-tall-{1080,1620}.{avif,jpg}`:
  Blender stills of the cassette wearing that same face (`docs/cassette-001/teaser/tools/teaser_scene.py --shot
  hero|hero-tall --face … --knobs 1,0.5,0,0.7,1`, rendered at 3840×2400 / 1620×2025, 512 samples, focus on the face,
  f/16). The wide one keeps the cassette in the right half so the hero type sits on the dark floor; the tall one is
  the phone's first screen. The hero is full-bleed (`sizes="100vw"`), so a 1440-wide window on a 2x screen asks
  for 2880 px: hence the 3840.

### Loading

- The first screen needs only the stylesheet, the font (WOFF2, 16 KB; the TTF stays as a fallback) and the hero
  (`fetchpriority="high"`). Everything below it is `loading="lazy"`. The classic look and the film's poster are
  fetched once the page has finished loading (the switch script), so switching is still instant.
- From some networks GitHub Pages' addresses (185.199.108–111.153) connect only now and then while jsDelivr and
  github.com answer at once. The images and the film are therefore served from jsDelivr's copy of this repo,
  pinned to a commit: `https://cdn.jsdelivr.net/gh/floamusic/floating-site@<sha>/assets/…`. ⚠ A new or changed
  image must be pushed first; then point the page at that commit. If jsDelivr fails, a small script in the
  `<head>` takes the same file from the site itself.
