# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A deliberately minimal, hand-written academic homepage (`https://0xC000005.github.io`). There is **no build system, no CSS file, no JavaScript, no CI** — GitHub Pages serves this folder verbatim from branch `master` root (`.nojekyll` disables Jekyll). To change the site: edit the HTML, commit, push. Nothing else.

## Hard rules

- Keep the "last century" aesthetic: zero CSS rules, default fonts/link colors, `hr` as the only divider, explicit `width`/`height` on images, pages under 10 KB. Do not add frameworks, stylesheets, scripts, analytics, favicons, or OpenGraph tags.
- Update the `Last updated:` line in `<small>` when editing a page.
- The `Blog` link on `index.html` points to a separate repo (`0xC000005/blog`), which is managed there — do not duplicate or restructure its content here.

## Verification

Open locally (`python3 -m http.server`) and click through; check total page weight stays under 10 KB (`wc -c *.html`).
