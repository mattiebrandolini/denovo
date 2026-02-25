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
Site rebuilds automatically via GitHub Actions (~30 seconds).

## Frontmatter Reference

Every post supports:
```yaml
title: "Post Title"
date: 2026-02-25
description: "Shows in lists and meta tags"
tags: [pathfinder-2e, homebrew, combat]
toc: true          # shows collapsible table of contents (essays/rules)
draft: true        # won't publish until set to false
```

Worlds posts also have:
```yaml
world: "wiege"     # or shallow-sea, shared
```

Rules posts also have:
```yaml
system: "Pathfinder 2e"
```

## Sections

| Section | Path | Purpose |
|---------|------|---------|
| Essays | `content/essays/` | Long-form analytical/reflective writing |
| Worlds | `content/worlds/` | De Novo worldbuilding — settings, lore, people |
| Fiction | `content/fiction/` | Short stories and creative writing |
| Rules | `content/rules/` | Homebrew rules and system modifications |
| Tools | `content/tools/` | Interactive web apps (DM screen, etc.) |

## Accessibility

Gear icon (⚙) in the header opens the accessibility panel:
- Dark mode (respects system preference)
- Reduced motion (respects system preference)
- Dyslexic-friendly font (OpenDyslexic)
- Text size (small / medium / large)
- Wide line spacing
- Colorblind-safe mode

All settings persist via localStorage.

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
- No build dependencies beyond Hugo
