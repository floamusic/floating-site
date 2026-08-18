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
| `assets/css/site.css` | Single stylesheet; palette lifted from the plug-in's `kDark` |
| `assets/fonts/` | ShareTechMono (SIL Open Font License, included) |
| `assets/img/` | Interface screenshots, web-optimised |
| `assets/video/` | Hero demo video, transcoded for web (see below) |

Static HTML and CSS only — no build step, no framework, no third-party
requests. The font is self-hosted, so the pages load nothing from outside the
origin.

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

## Note on this repo

This repo holds only the public site. The FLOATING source is private and lives
elsewhere; nothing here is generated from it automatically.
