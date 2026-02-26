# AI Maintenance Guide — De Novo

This file exists so any AI assistant (Claude or otherwise) can orient to this codebase immediately without re-deriving the architecture.

## Build & Deploy

```bash
cd ~/projects/denovo
hugo server --buildDrafts    # Local dev at http://localhost:1313/denovo/
hugo                         # Production build to public/
git add -A && git commit -m "message" && git push  # Auto-deploys via GitHub Actions
```
Deploy takes ~30 seconds. CI runs quality gates (alt text, internal links, HTML structure) before deploying.

## Architecture

**Static site generator:** Hugo (v0.142.0+)
**Theme:** Custom, lives in `themes/denovo/`
**Hosting:** GitHub Pages via Actions
**Repo:** github.com/mattiebrandolini/denovo

## File Map

```
hugo.toml                    # Site config: menus, taxonomies, permalinks, related content
content/
  essays/                    # Long-form writing (archetype: archetypes/essays.md)
  worlds/                    # De Novo worldbuilding (archetype: archetypes/worlds.md)
    shared/                  # Cosmology spanning all settings (Terrans, Dark Tapestry, Van)
    wiege/                   # Wiege-specific (nations, people, deities, organizations)
    shallow-sea/             # Shallow Sea-specific
  fiction/                   # Short stories (archetype: archetypes/fiction.md)
  rules/                     # PF2e homebrew (archetype: archetypes/rules.md)
  tools/                     # Interactive apps (archetype: archetypes/tools.md)
  about.md                   # About page
  licenses.md                # Licensing (CC BY-NC-SA 4.0 + Paizo CUP)
  accessibility.md           # Accessibility statement
themes/denovo/
  layouts/
    _default/
      baseof.html             # ⚠️ MASTER TEMPLATE — all pages inherit from this
      list.html               # Section index pages
      single.html             # Individual content pages
    partials/
      header.html             # Site header + accessibility panel
      footer.html             # Footer with edit/report/licenses links
      breadcrumbs.html        # Auto breadcrumb navigation
      series-nav.html         # Prev/next within a series
      related.html            # Related content block
    shortcodes/
      callout.html            # {{</* callout type="warning" */>}}...{{</* /callout */>}}
      spoiler.html            # {{</* spoiler title="..." */>}}...{{</* /spoiler */>}}
      definition.html         # {{</* definition term="..." */>}}...{{</* /definition */>}}
      figure.html             # {{</* figure src="..." alt="..." */>}} — ALT IS MANDATORY
      steps.html              # {{</* steps */>}}...{{</* /steps */>}}
    tools/
      single.html             # Wider layout + loading/error states + no-JS fallback
      list.html               # Tools index
    tags/                     # Tag taxonomy templates
    series/                   # Series taxonomy templates
    404.html                  # Custom 404
    index.html                # Homepage
  static/
    css/
      style.css               # ⚠️ ALL styles in one file — uses CSS custom properties
      fonts.css               # @font-face declarations (self-hosted)
    fonts/                    # Self-hosted: Cormorant, Lora, OpenDyslexic
    img/
      placeholder.svg         # Fallback for broken images
    favicon.ico / favicon.png
.github/workflows/hugo.yml   # Build + quality gates + deploy
```

## CSS Custom Properties (the style API)

All theming goes through variables in `:root` and `[data-theme="dark"]` in style.css:

```
--text, --text-light        # Text colors
--bg, --bg-alt              # Background colors
--accent, --accent-light    # Link/accent colors
--border                    # Border color
--body-font, --heading-font # Font stacks
--base-size                 # Root font size
--line-height               # Body line height
--max-width                 # Content container width (660px default, 1100px for tools)
```

## Common Tasks

### Add a new essay/post
```bash
hugo new essays/my-title.md
# Edit content/essays/my-title.md
# Set draft: false when ready
# Push to deploy
```

### Add a new section
1. Create `content/newsection/_index.md`
2. Add archetype in `archetypes/newsection.md`
3. Add menu entry in `hugo.toml` under `[menu]`
4. Add to homepage sections list in `themes/denovo/layouts/index.html` (if chronological content)
5. Add permalink pattern in `hugo.toml` under `[permalinks.page]` (not needed for worlds — uses directory structure)

### Add a new worldbuilding article
1. Create file in `content/worlds/{subsection}/article-name.md` (shared, wiege, or shallow-sea)
2. Set `article_type` in frontmatter (overview, nation, person, deity, organization, location, concept, history)
3. Set `knowledge` scope (common, uncommon, player)
4. Cross-link to related articles on first mention
5. URL will be `/worlds/{subsection}/article-name/` automatically

### Add a new worldbuilding subsection
1. Create `content/worlds/newworld/_index.md` with title and description
2. Articles placed in that directory automatically appear in the subsection
3. The subsection card appears on `/worlds/` automatically

### Add a new shortcode
1. Create `themes/denovo/layouts/shortcodes/name.html`
2. Add CSS in `themes/denovo/static/css/style.css`
3. Document usage in README.md

### Add a new tool
1. `hugo new tools/tool-name.md`
2. Write HTML/JS directly in the markdown (Hugo renders it)
3. The tools/single.html template provides: loading spinner, error state, `toolReady()` / `toolError()` helpers, no-JS fallback, wider layout
4. Call `toolReady()` when your tool initializes

### Change accessibility settings behavior
All a11y logic is in `baseof.html` bottom `<script>` block. Settings use `data-*` attributes on `<html>` element and localStorage keys prefixed with `dn-`.

## DO NOT TOUCH (without understanding why)

- **Self-hosted fonts:** External CDNs break behind school firewalls. See docs/decisions/self-hosted-fonts.md
- **Inline `<script>` in `<head>` of baseof.html:** Applies theme before CSS loads to prevent flash. Must stay inline and in `<head>`.
- **`style="display:none"` on a11y panel:** Prevents panel from rendering as page content before JS loads. CSS alone was insufficient.
- **`fetch-depth: 0` in GitHub Actions:** Required for git-based `lastmod` dates. Removing this breaks "Updated" timestamps.
- **Figure shortcode `errorf` on missing alt:** This is intentional — the build should fail if alt text is missing.
- **Permalink structure in hugo.toml:** URLs are permanent contracts. Changing these breaks external links.

## Taxonomies

- **Tags:** Cross-section discovery. Used everywhere.
- **Series:** Multi-part content with automatic prev/next navigation. Weighted 2x in related content.



## Search (Pagefind)

Full-text search powered by [Pagefind](https://pagefind.app/). Runs at build time, generates a static index. No server needed.

- Pagefind is installed and run in CI after Hugo builds (see `.github/workflows/hugo.yml`)
- Index output goes to `public/pagefind/` (not committed, generated fresh each deploy)
- Search page: `content/search.md`
- Search UI styling: CSS overrides in `style.css` under "PAGEFIND SEARCH OVERRIDES"
- Only content pages are indexed (list pages, taxonomy pages, homepage, 404 all excluded)
- Filters available: section (auto from Hugo section), tags (from frontmatter)
- To exclude a page from search: add `search_exclude: true` to frontmatter
- Local testing: `hugo && pagefind --site public --output-path public/pagefind`
## Quality Gates (CI)

On every push, before deploy:
1. Alt text check — fails if any markdown image has empty `![]()` brackets
2. Internal link check — verifies all `href` links in built HTML resolve to real files
3. HTML structure — one `<main>`, max one `<h1>`, `lang` attribute on `<html>`
