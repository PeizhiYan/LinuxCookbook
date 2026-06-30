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
├── README.md             # Original markdown table of contents (source of truth for NAV)
├── CLAUDE.md             # This file
├── build.py              # Optional: embeds all markdown into content-data.js (not used by default)
├── apple-touch-icon.png  # 180×180 PNG — iOS home screen icon
├── favicon-32.png        # 32×32 PNG — browser tab favicon
├── lib/
│   └── marked.min.js     # Vendored marked.js v9.1.6 (local copy, no CDN needed)
└── content/
    ├── chmod.md
    ├── ssh.md
    ├── tmux.md
    └── ... (29 .md files total)
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

The sidebar and home cards are driven by a hardcoded `NAV` array in the `<script>` block
of `index.html`. It mirrors `README.md` exactly. **When adding a new markdown file, update
both `README.md` and the `NAV` array.**

```js
const NAV = [
  {
    category: "Linux / Unix-like",
    items: [
      { emoji: "🔑", label: "Change File/Path Permission (chmod)", file: "content/chmod.md" },
      // ...
    ]
  },
  // ... 7 categories total
];
```

---

## Key JS Functions

| Function | Description |
|---|---|
| `loadDoc(file, label, category, emoji)` | Fetches a markdown file, renders it with `marked`, runs `hljs` on code blocks, adds copy buttons |
| `showHome()` | Switches back to the home/dashboard view |
| `prefetchContent()` | Fetches all 29 markdown files concurrently into `contentIndex` Map for full-text search |
| `runSearch(q)` | Searches `contentIndex` + titles, renders results with text snippets in the sidebar |
| `stripMd(text)` | Strips markdown syntax before snippet extraction |
| `getSnippet(rawText, q)` | Extracts a ~130-char snippet around the query match, HTML-highlights it |
| `findNavItem(file)` | Looks up a NAV item by filename (used for hash routing and internal links) |

---

## Full-Text Search

Search works **without pre-embedding** any content:

1. `prefetchContent()` fires immediately on page load — all 29 `.md` files are fetched
   concurrently via `Promise.all` (~30 KB total, finishes in <1s on localhost).
2. Content is stored in `contentIndex: Map<file, rawMarkdown>`.
3. When the user types, the normal sidebar nav hides and a results panel appears showing
   matching articles with highlighted text snippets.
4. If the user types before the index is ready, a pulsing "Indexing…" indicator shows and
   results update automatically when the fetch completes.

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
- Code blocks get a hover-reveal **Copy** button injected after `hljs` processes them.

---

## How to Add a New Article

1. Create `content/your-topic.md`
2. Add the `[return to table](../README.md)` line at the top (optional, it's stripped anyway)
3. Add an entry to `README.md` under the appropriate category
4. Add the same entry to the `NAV` array in `index.html`:
   ```js
   { emoji: "🔧", label: "Your Topic", file: "content/your-topic.md" }
   ```
5. Refresh — the page picks it up automatically.

---

## Serving Locally

```bash
python3 -m http.server 8765
# then open http://localhost:8765
```

---

## What `build.py` Does (optional, not required)

`build.py` reads all `content/*.md` files and writes a `content-data.js` file that
embeds them as a JS object (`window.CONTENT_DATA`). This was created to support
`file://` access but is **not currently used** — the app uses `fetch()` exclusively.
You can delete `build.py` and `content-data.js` if they exist.
