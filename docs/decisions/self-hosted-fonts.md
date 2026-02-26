# Decision: Self-Hosted Fonts

**Date:** 2026-02-25
**Status:** Active

## Context
The site originally loaded Cormorant and Lora from Google Fonts, and OpenDyslexic from jsDelivr CDN.

## Problem
- School networks unpredictably block external CDNs
- Google Fonts requests leak student browsing data to Google
- CDN availability is not guaranteed (outages, geo-restrictions)
- GDPR implications for European visitors

## Decision
Self-host all fonts (Cormorant, Lora, OpenDyslexic) with woff2/woff files in `static/fonts/`. Font-face declarations in `static/css/fonts.css`. Zero external requests.

## Consequences
- Slightly larger repo size (~500KB of font files)
- Must manually update fonts if upstream releases new versions
- Site works fully offline, behind any firewall, on any network
- No data leakage to third parties

## DO NOT revert this without understanding the school network context.
