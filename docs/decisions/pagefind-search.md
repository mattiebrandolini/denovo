# Decision: Pagefind for Search

**Date:** 2026-02-25
**Status:** Active

## Context
The site will host worldbuilding content, homebrew rules, essays, and fiction. With growing content, users need to find things quickly — especially campaign setting details that cross-reference each other.

## Options Considered
- **Pagefind:** Build-time indexing, static output, no server, fuzzy matching built in. Free.
- **Lunr.js:** Client-side indexing at page load. Gets slow with large content.
- **Algolia/Meilisearch:** Requires a server or paid service. Overkill for a static personal site.
- **Fuse.js:** Client-side fuzzy search. Must load all content into browser memory.

## Decision
Pagefind. It indexes at build time and generates a static search index that loads progressively. Works offline once loaded. Fuzzy matching handles typos. Section and tag filters are auto-generated from Hugo metadata.

## Implementation
- Pagefind runs in CI after Hugo builds (`.github/workflows/hugo.yml`)
- Output directory `public/pagefind/` is generated fresh each deploy (not committed)
- Search page at `content/search.md` loads Pagefind UI
- Only content pages are indexed (list/taxonomy/utility pages excluded via `data-pagefind-ignore`)
- CSS overrides in `style.css` match De Novo's design tokens

## Consequences
- Search quality scales with content — currently thin, will get better as pages are added
- No ongoing costs or server maintenance
- Adding search to the nav now means users expect it to work (even with minimal content)
- Pagefind version is pinned in CI workflow — update manually when needed
