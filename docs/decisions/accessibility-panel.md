# Decision: Accessibility Panel Architecture

**Date:** 2026-02-25
**Status:** Active

## Context
The site has 6 accessibility settings that need to persist and apply instantly.

## Decision
- All settings stored as `data-*` attributes on `<html>` element
- CSS targets these attributes (e.g., `[data-theme="dark"]`, `[data-font="dyslexic"]`)
- Settings persisted via localStorage with `dn-` prefix
- Inline `<script>` in `<head>` applies settings BEFORE CSS loads (prevents flash)
- Panel hidden with `style="display:none"` inline (CSS-only hiding was unreliable)
- JS click handlers on toggle switches (CSS checkbox hacks were unreliable across browsers)
- Focus management: opening panel moves focus inside, closing returns to trigger

## Key constraints
- Panel markup must have `style="display:none"` — removing this causes it to render as page content
- The `<head>` script must stay inline and in `<head>` — moving it to footer causes flash-of-wrong-theme
- Toggle switches use JS click handlers, not CSS `:checked` — do not refactor to CSS-only
