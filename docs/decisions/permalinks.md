# Decision: Permalink Strategy

**Date:** 2026-02-25
**Status:** Active

## Context
URLs are permanent contracts with the outside world — RSS readers, search engines, bookmarks, shared links.

## Decision
Permalinks are locked per section in `hugo.toml`:
```
/essays/:slug/
/fiction/:slug/
/worlds/:slug/
/rules/:slug/
/tools/:slug/
```

Slug is derived from the filename by default, but can be overridden in frontmatter.

## Consequences
- Reorganizing content files (moving between folders, renaming) does NOT break URLs
- Adding new sections requires adding a permalink pattern
- Changing these patterns after content is published will break external links
- If a page must move, create an HTML redirect at the old URL

## DO NOT change permalink patterns for sections that have published content.


## Update: 2026-02-26

Worlds section was restructured from flat to nested:
- `/worlds/shared/` — cosmology spanning all settings
- `/worlds/wiege/` — Wiege-specific content
- `/worlds/shallow-sea/` — Shallow Sea-specific content

The explicit permalink override for worlds was removed. Hugo's default
nested section behavior generates the correct hierarchical URLs from
the directory structure.

Article URLs are now: `/worlds/{subsection}/{slug}/`
Section URLs are: `/worlds/{subsection}/`
