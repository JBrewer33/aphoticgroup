# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A placeholder landing page for [aphoticgroup.com](https://aphoticgroup.com): a single self-contained
[index.html](index.html) (inline CSS + vanilla JS, no build step, no dependencies). There is no
package.json, no framework, and no test suite — everything lives in that one file.

## Develop & deploy

- Preview: open [index.html](index.html) in a browser, or `python -m http.server 8000` then visit http://localhost:8000.
- Deploy: pushing to `main` publishes automatically via GitHub Pages. [CNAME](CNAME) pins the apex domain `aphoticgroup.com` — do not delete it or custom-domain hosting breaks.

## Architecture

The page composites several full-viewport `.layer` elements inside a `.stage`, back-to-front:
`murk` (drifting blurred blobs) → `presence` (swelling glow) → `rays` (god-rays) → `#snow` (canvas
particles) → `vignette` (edge darkening) → `content` (wordmark + email). The deep-sea / "aphotic"
look comes from this stack plus CSS keyframe animations (`murkdrift`, `raysway`, `presenceswell`,
`textbreathe`).

The `#snow` canvas runs a `requestAnimationFrame` particle loop in the inline `<script>`: ~110
particles created by `mk()`, ~12% flagged `lum` (brighter, colored, pulsing/glowing), the rest dim
drifters; they recycle when they fall off the bottom.

When editing, preserve the two accessibility/perf guards already in place: the
`@media (prefers-reduced-motion:reduce)` CSS block and the matching `reduce` check in the JS loop
(both must stay in sync if you add motion), and the `dpr` capped at 2 in `resize()`.
