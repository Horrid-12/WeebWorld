# Analysis: WeebWorld

An anime tracking and discovery single-page application that displays currently-airing seasonal anime and movies, powered by the [Jikan API](https://jikan.moe/) (unofficial MyAnimeList API).

---

## Tech Stack

| Technology | Role |
|---|---|
| HTML5 | Page structure |
| Tailwind CSS (CDN) | Utility-first styling |
| Vanilla JavaScript (ES6+) | All logic, API, DOM |
| Jikan API v4 | Anime data source |
| Vercel | Deployment |
| Git & GitHub | Version control |

No build tools, bundlers, or backend — a pure frontend static site.

---

## Directory Structure

```
WeebWorld-main/
├── .git/
└── WeebWorld/
    ├── .git/
    ├── index.html          # Splash/landing page
    ├── updates.html        # Main application page (360 lines)
    ├── anime.js            # Core JavaScript (606 lines)
    └── README.md
```

---

## Architecture

### Pages

- **`index.html`** — Landing page with hero section and CTA linking to `updates.html`.
- **`updates.html`** — Main SPA-style page: navbar, search, filter panel, anime card grid, pagination, recently viewed strip, favorites grid, and detail modal.

### Data Flow

```
Jikan API (v4/seasons/now)
    ↓ fetch (12s AbortController timeout)
    ↓ JSON parse
currentData[]
    ↓ applyFilters() — genre, status, season, year, sort, search
filteredData[]
    ↓ paginate (9 per page)
pageData[]
    ↓ renderAnime() → DOM card grid
```

### Key Modules (all in `anime.js`)

- **`fetchAnime()`** — Fetches seasonal data with sessionStorage caching (10-min TTL) and error/retry handling.
- **`applyFilters()`** — Multi-criteria filter + sort pipeline.
- **`renderAnime()`** — Builds card grid with lazy images, score badges, hover effects.
- **`renderPagination()`** — Dynamic page buttons with smart range (current ±2).
- **`openModal()` / `closeModalFn()`** — Accessible modal with focus trap, ESC close, backdrop click, `aria-modal`.
- **`toggleFavorite()` / `addToRecentlyViewed()`** — localStorage persistence.
- **`saveFilters()` / `loadFilters()`** — Filter state persistence across reloads.
- **`clearAllFilters()`** — Resets all filters and search.
- **`debounce()`** — 400ms debounce on live search.

---

## Features

- **Live seasonal data** from Jikan API
- **Filters:** Genre, status (airing/upcoming/completed), season, year
- **Sort:** Title (A-Z), score, episode count, year
- **Search:** Text search with button or debounced live input
- **Pagination:** 9 per page with numbered buttons + prev/next
- **Detail modal:** Synopsis, genres, studios, producers, score, episodes, status, aired date, duration, rating, MAL link, trailer link, favorite button
- **Favorites:** Heart-based system, persisted in localStorage
- **Recently viewed:** Horizontal scroll strip of last 10 viewed anime
- **Caching:** 10-min sessionStorage TTL to reduce API calls
- **Security:** `isValidUrl()` and `safeSetHref()` sanitize external links; `rel="noopener noreferrer"` on all external anchors

---

## Notable Patterns & Design Decisions

1. **Zero-dependency frontend** — No npm, webpack, React, or backend. Vanilla JS + Tailwind CDN keeps it lightweight.
2. **Defensive programming** — Extensive optional chaining (`?.`), null checks, safe DOM access, URL sanitization.
3. **Accessible modal** — Focus trapping, Escape key, backdrop close, `aria-modal="true"`, `role="dialog"`, focus restoration.
4. **Client-side caching** — 10-minute sessionStorage TTL balances freshness with performance.
5. **State persistence** — Three localStorage keys (filters, favorites, recently viewed) survive page reloads.
6. **Debounced live search** — 400ms debounce prevents excessive filtering during rapid typing.
7. **SPA-like experience** — `updates.html` functions as a full SPA with client-side routing (filter/sort/paginate/modal) despite being a static HTML page.
8. **Progressive enhancement** — Static content displays without JS; API features enhance when JS is available.
