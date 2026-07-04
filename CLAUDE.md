# CLAUDE.md — Architecture & Implementation Context

This file is a reference for AI assistants (Claude, Gemini, etc.) working on this repository.

---

## What This Repo Is

A personal Linux command reference ("cookbook") by Peizhi Yan. Content lives in plain
Markdown files. A single `index.html` acts as a zero-build-step CMS that fetches and
renders those files in the browser.

---

## File Structure

```
LinuxCookbook/
├── index.html            # The entire front-end app (HTML + CSS + JS, single file)
├── README.md             # Source of truth for navigation (parsed at runtime by index.html)
├── CLAUDE.md             # This file
├── apple-touch-icon.png  # 180×180 PNG — iOS home screen icon
├── favicon-32.png        # 32×32 PNG — browser tab favicon
├── lib/
│   └── marked.min.js     # Vendored marked.js v9.1.6 (local copy, no CDN needed)
└── content/
    ├── chmod.md
    ├── ssh.md
    ├── tmux.md
    └── ... (.md files — add freely, just update README.md)
```

---

## How `index.html` Works

`index.html` is a **self-contained single-page app** — no framework, no build step.

### Dependencies (loaded at runtime)
| Library | Source | Purpose |
|---|---|---|
| `marked.min.js` | `lib/` (local) | Markdown → HTML |
| `highlight.js` | CDN (cdnjs) | Syntax highlighting for code blocks |
| Inter + JetBrains Mono | Google Fonts CDN | Typography |

> **Requirement**: Must be served via HTTP (e.g. `python3 -m http.server 8765`).
> `fetch()` is used to load markdown files; it does not work over `file://`.

---

## Navigation Data (`NAV` array)

The sidebar and home cards are driven by a `NAV` array that is **built dynamically at
runtime** by fetching and parsing `README.md`. **`index.html` does not need to be edited
when adding or removing articles — only `README.md` needs updating.**

The parser (`loadNav()` in `index.html`) looks for:
- Top-level list items (`- Category Name`) → new category
- Nested list items (`  - [emoji Label](./content/file.md)`) → article entry

The leading emoji in the link label is automatically split from the text label using
`Intl.Segmenter` (with a codepoint-range fallback for older browsers).

---

## Key JS Functions

| Function | Description |
|---|---|
| `loadNav()` | Fetches `README.md` and parses it into the `NAV` array |
| `initApp()` | Bootstrap: calls `loadNav()`, builds UI, starts prefetch, handles hash routing |
| `buildSidebar()` | Renders the sidebar nav from `NAV` |
| `buildHomeStats()` | Renders the article/category count pills on the home page |
| `buildCategoryCards()` | Renders the home page category cards from `NAV` |
| `loadDoc(file, label, category, emoji)` | Fetches a markdown file, renders it with `marked`, runs `hljs` on code blocks, adds copy buttons (desktop) or long-press copy (mobile) |
| `showHome()` | Switches back to the home/dashboard view |
| `prefetchContent()` | Fetches all `.md` files concurrently into `contentIndex` Map for full-text search |
| `runSearch(q)` | Searches `contentIndex` + titles, renders results with text snippets in the sidebar |
| `stripMd(text)` | Strips markdown syntax before snippet extraction |
| `getSnippet(rawText, q)` | Extracts a ~130-char snippet around the query match, HTML-highlights it |
| `findNavItem(file)` | Looks up a NAV item by filename (used for hash routing and internal links) |
| `showToast(msg)` | Shows a brief centered toast popup (used for mobile long-press copy feedback) |

---

## Full-Text Search

Search works **without pre-embedding** any content:

1. `prefetchContent()` fires immediately on page load — all `.md` files are fetched
   concurrently via `Promise.all` (~30 KB total, finishes in <1s on localhost).
2. Content is stored in `contentIndex: Map<file, rawMarkdown>`.
3. When the user types, the normal sidebar nav hides and a results panel appears showing
   matching articles with highlighted text snippets.
4. If the user types before the index is ready, a pulsing "Indexing…" indicator shows and
   results update automatically when the fetch completes.
5. The search box has a **clear (✕) button** that appears when the field has text.

---

## Theming

- **Default**: Light theme (GitHub-light palette, white background).
- **Toggle**: `☀️ Light` / `🌙 Dark` button in the header (top-right area).
- Theme is implemented via CSS custom properties on `:root` (light) and
  `[data-theme="dark"]` (dark). No `prefers-color-scheme` media query — user controls it.
- Syntax highlighting switches between `github.min.css` ↔ `github-dark.min.css`
  on the `#hljs-theme` `<link>` element.

### CSS Variables (light / dark)
| Variable | Light | Dark |
|---|---|---|
| `--bg-primary` | `#ffffff` | `#0d1117` |
| `--bg-secondary` | `#f6f8fa` | `#161b22` |
| `--accent` | `#0969da` | `#58a6ff` |
| `--text-primary` | `#1f2328` | `#e6edf3` |
| `--border` | `#d0d7de` | `#30363d` |

---

## Routing

Hash-based routing only. Example: `index.html#content%2Ftmux.md`

- On load, the hash is decoded and matched against `NAV` via `findNavItem()`.
- Clicking a sidebar link or card calls `loadDoc()`, which sets `location.hash`.
- Back button calls `showHome()`, which clears the hash with `history.pushState`.

---

## Markdown Rendering Notes

- The custom `marked` renderer strips the `[return to table](../README.md)` line that
  appears at the top of every content file (legacy nav link, not needed in the web UI).
- Internal links between `.md` files (e.g. `content/ssh.md`) are intercepted and handled
  via `loadDoc()` rather than a browser navigation.
- Code blocks get a hover-reveal **Copy** button injected after `hljs` processes them
  (desktop only — see Mobile section below).

---

## Mobile Behaviour (≤768px)

- Sidebar is hidden off-screen and slides in via a hamburger button + overlay.
- Brand title ("Linux Cookbook") is hidden to give the search bar full header width.
- Content area uses full width (`max-width: 100%`) with 16px side padding.
- Code blocks and tables are horizontally scrollable (`overflow-x: auto`).
- **Copy button is hidden**. Instead, long-pressing (600ms) a code block copies its text
  and shows a "Copied!" toast that disappears after 1 second.
- Category grid collapses to a single column.

---

## How to Add a New Article

1. Create `content/your-topic.md`
2. Add the `[return to table](../README.md)` line at the top (optional, it's stripped anyway)
3. Add an entry to `README.md` under the appropriate category:
   ```md
     - [🔧 Your Topic](./content/your-topic.md)
   ```
4. Refresh — the page picks it up automatically. No changes to `index.html` needed.

---

## Serving Locally

```bash
python3 -m http.server 8765
# then open http://localhost:8765
```

To check if a server is already running on a port:
```bash
lsof -i :8765
# kill it:
kill $(lsof -ti :8765)
```
