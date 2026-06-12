# Security Analysis — WeebWorld

Audit date: 2026-06-12
Scope: Full codebase (2 HTML files, 1 JavaScript file)

---

## Summary

| Severity | Count | Status |
|---|---|---|
| High | 2 | 2 fixed |
| Medium | 2 | 2 fixed |
| Low | 1 | 1 fixed |

---

## Vulnerabilities

### HIGH-01: Unhandled `JSON.parse` exceptions on stored data

**File:** `anime.js:14-15, 91, 400-402`

**Status:** FIXED

Four places parsed localStorage/sessionStorage without try-catch:
- `recentlyViewed`, `favorites` (module-level)
- `loadFilters()` — filters
- `fetchAnime()` — session cache

**Fix applied:** Added `safeParse(value, fallback)` utility (`anime.js:78-80`) that wraps `JSON.parse` in try-catch. All four call sites updated to use it.

---

### HIGH-02: Supply chain risk — Tailwind CSS CDN

**Files:** `index.html`, `updates.html`

**Status:** FIXED

Replaced the CDN `<script>` tag with a local Vite + Tailwind CSS build:
- `tailwindcss` and `@tailwindcss/vite` installed as dev dependencies (locked in `package-lock.json`)
- Build output is 21 kB CSS (down from ~3 MB CDN payload) + 14 kB JS bundle
- CDN origin removed from CSP; `script-src` and `style-src` scoped to `'self'`
- No external runtime dependency on third-party CDN

---

### MEDIUM-01: No Content Security Policy

**Files:** `index.html`, `updates.html`

**Status:** FIXED

Added a CSP meta tag to both HTML files (`<head>`) that restricts scripts, styles, and connections to required origins only.

---

### MEDIUM-02: No `.gitignore` file

**File:** Missing at project root.

**Status:** FIXED

Added `.gitignore` covering `node_modules/`, `dist/`, `.env`, `.DS_Store`, `Thumbs.db`, and editor directories.

---

### LOW-01: Nested Git repositories

**Paths:** Previously `WeebWorld-main/.git/` (empty) + `WeebWorld/.git/` (active)

**Status:** FIXED

Flattened the repo structure: moved all inner files (including `.git/`) up one level, removed the empty outer `.git/` and stale duplicate files. Single git repo now at project root with full commit history preserved.

---

## Already Mitigated

| Issue | How |
|---|---|
| XSS via `innerHTML` | All data rendered via `textContent`, `createElement`, or safe `src` attributes |
| `javascript:` URL injection | `isValidUrl()` rejects non-http/https protocols |
| Tab-napping via external links | `safeSetHref()` always sets `rel="noopener noreferrer"` |
| API timeout / hanging requests | `AbortController` with 12-second timeout |
| Broken image handling | `setImageFallback()` prevents broken image icons |

---

## Recommendations (Priority Order)

1. ~~**Fix HIGH-01** — Wrap all `JSON.parse` calls in try-catch~~ **DONE**
2. ~~**Fix MEDIUM-01** — Add CSP meta tag~~ **DONE**
3. ~~**Fix MEDIUM-02** — Add `.gitignore`~~ **DONE**
4. ~~**Fix HIGH-02** — Move to local Tailwind build with locked version~~ **DONE**
5. ~~**Fix LOW-01** — Flatten nested git repos~~ **DONE**
