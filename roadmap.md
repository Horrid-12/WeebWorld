# Roadmap — WeebWorld

A phased plan for improving the WeebWorld anime tracking app.

---

## Phase 1: Foundation — Tooling & Repo Hygiene

- [ ] **Initialize `package.json` + Vite + Tailwind (PostCSS)** — enables module bundling, HMR dev server, Tailwind purge (3MB → ~10KB CSS), and production builds.
- [ ] **Add `.gitignore`** — prevent accidental commits of `node_modules`, `dist`, etc.
- [ ] **Clean up nested `.git`** — outer repo is empty and confusing; flatten structure and keep only the inner repo.
- [ ] **Add `vercel.json`** — explicit deployment config for Vercel (build command, output dir, rewrites).
- [ ] **Split `anime.js` into ES modules** — `src/api.js`, `src/filters.js`, `src/modal.js`, `src/storage.js`, `src/ui.js`, `src/main.js`.
- [ ] **Add JSDoc type annotations** — document the anime data shape and all function signatures.

## Phase 2: Performance

- [ ] **Tailwind purge** — Vite + `@tailwindcss/vite` or PostCSS scans HTML/JS and emits only used classes.
- [ ] **Use smaller card images** — `webp->image_url` instead of `webp->large_image_url` for the grid.
- [ ] **Add `<link rel="preconnect">`** — for `api.jikan.moe` and `cdn.tailwindcss.com`.
- [ ] **Add PWA support** — `manifest.json` + service worker for offline access and "Add to Home Screen".

## Phase 3: Testing & CI

- [ ] **Vitest unit tests** — filter logic, sort logic, pagination bounds, localStorage helpers.
- [ ] **Playwright E2E tests** — page loads → cards render → filter changes results → modal opens → favorite persists on reload.
- [ ] **GitHub Actions** — run `vitest` on push, Playwright on PR, deploy to Vercel on `main`.

## Phase 4: New Features

- [ ] **Season browser** — pick any year + season; fetch from `/seasons/{year}/{season}`.
- [ ] **Top anime page** — new view using `/top/anime`, switchable via nav tab.
- [ ] **Search all anime** — `/v4/anime?q=` endpoint for global search.
- [ ] **Weekly schedule** — `/schedules` endpoint showing anime by day of week.
- [ ] **Watch list tracking** — "Currently Watching", "Plan to Watch", "Completed" lists in localStorage.
- [ ] **Random anime** — `/random/anime` button that opens result in the modal.

## Phase 5: UX/UI Polish

- [ ] **Skeleton loading cards** — 9 gray card outlines with shimmer instead of a spinner.
- [ ] **Card entrance animations** — `IntersectionObserver` with staggered `fade-in-up`.
- [ ] **Responsive search box** — `w-full max-w-md` instead of fixed `w-64`.
- [ ] **Modal close button focus** — focus the close button on modal open.
- [ ] **Keyboard shortcuts** — `/` to focus search, `?` to show shortcuts overlay.
- [ ] **Skip-to-content link** — first focusable element for keyboard users.
