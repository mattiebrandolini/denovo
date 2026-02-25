# De Novo

Personal site — essays, fiction, worldbuilding, homebrew rules, and interactive tools.

**Live:** [mattiebrandolini.github.io/denovo](https://mattiebrandolini.github.io/denovo/)

## Adding Content

**New post in any section:**
```bash
cd ~/projects/denovo
hugo new essays/my-post-title.md    # or worlds/, fiction/, rules/, tools/
```
This creates a draft with the right frontmatter for that section. Edit it, then set `draft: false` when ready to publish.

**Publish:**
```bash
git add -A && git commit -m "Add: title" && git push
```
Site rebuilds automatically via GitHub Actions (~30 seconds). CI checks run automatically — alt text, internal links, HTML structure.

## Frontmatter Reference

Every post supports:
```yaml
title: "Post Title"
date: 2026-02-25
description: "Shows in lists and meta tags"
summary_simple: "One-sentence plain-English summary for ELL readers"
tags: [pathfinder-2e, homebrew, combat]
series: ["My Series Name"]       # for multi-part content
toc: true                        # shows collapsible table of contents
lang: "en"                       # per-page language (for screen readers/translation)
draft: true                      # won't publish until set to false
```
`lastmod` is automatically pulled from git commit history.

Worlds posts also have `world: "wiege"`. Rules posts have `system: "Pathfinder 2e"`. Tools posts have `noscript_description: "..."`.

## Shortcodes

Use these inside any markdown file:

```markdown
{{</* callout type="warning" title="Optional Title" */>}}
Content here. Supports **markdown**.
{{</* /callout */>}}
Types: note, warning, example, tip

{{</* spoiler title="Click to reveal" */>}}
Hidden content.
{{</* /spoiler */>}}

{{</* definition term="Nephilim" translation="nefilim" lang="es" */>}}
A descendant of mixed mortal and celestial heritage.
{{</* /definition */>}}

{{</* figure src="image.jpg" alt="Required description" caption="Optional" credit="Optional" */>}}
(alt text is mandatory — build fails without it)

{{</* steps title="Optional heading" */>}}
1. First step
2. Second step
{{</* /steps */>}}
```

## Sections

| Section | Path | Purpose |
|---------|------|---------|
| Essays | `content/essays/` | Long-form analytical/reflective writing |
| Worlds | `content/worlds/` | De Novo worldbuilding — settings, lore, people |
| Fiction | `content/fiction/` | Short stories and creative writing |
| Rules | `content/rules/` | Homebrew rules and system modifications |
| Tools | `content/tools/` | Interactive web apps (DM screen, etc.) |

## Features

**Accessibility** (gear icon, all persistent via localStorage):
Dark mode, reduced motion (both respect OS preference), dyslexic-friendly font, text size, wide line spacing, colorblind-safe mode, skip-to-content link, ARIA landmarks, focus management

**Content:** Tags + Series taxonomies, related content engine, breadcrumbs, reading time, auto-TOC, "Updated" dates from git, simple summary block for ELL, prev/next series navigation

**SEO/Sharing:** Open Graph + Twitter cards, RSS feeds, sitemap, robots.txt, JSON-LD (planned), permalinks locked per section

**Developer:** Edit-this-page links, content archetypes, CI quality gates (alt text, links, HTML), self-hosted fonts (zero external requests), print stylesheet, no-JS fallback on tools

**Legal:** Licenses page (CC BY-NC-SA 4.0, Paizo Community Use Policy, font licenses)

## Local Development

```bash
cd ~/projects/denovo
hugo server --buildDrafts
# Opens at http://localhost:1313/denovo/
```

## Tech Stack

- **Hugo** static site generator
- **GitHub Pages** hosting via Actions
- Custom theme (`themes/denovo/`)
- Self-hosted fonts (Cormorant, Lora, OpenDyslexic)
- No external requests, no build dependencies beyond Hugo
