# floating-site

Public one-page site and beta download host for **FLOATING** — a capture-first
memory instrument by Fløa (AU / VST3).

Live: <https://floamusic.github.io/floating-site/>

## Contents

| Path | What it is |
| --- | --- |
| `index.html` | Home — what it does, the story, downloads, support, contact |
| `install-macos.html` | macOS closed-beta install guide |
| `install-windows.html` | Windows closed-beta install guide |
| `assets/css/site.css` | Single stylesheet; palette lifted from the plug-in's `kDark` |
| `assets/fonts/` | ShareTechMono (SIL Open Font License, included) |
| `assets/img/` | Interface screenshots, web-optimised |

Static HTML and CSS only — no build step, no framework, no third-party
requests. The font is self-hosted, so the pages load nothing from outside the
origin.

## Downloads

Binaries are **not** in this repo. They are attached to the
[`v0.1.0-beta.1` release](https://github.com/floamusic/floating-site/releases/tag/v0.1.0-beta.1),
and the download buttons link straight at those assets. `SHA256SUMS.txt` on the
release lets anyone verify what they downloaded.

## Note on this repo

This repo holds only the public site. The FLOATING source is private and lives
elsewhere; nothing here is generated from it automatically.
