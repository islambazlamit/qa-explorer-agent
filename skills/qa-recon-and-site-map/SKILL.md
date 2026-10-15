---
name: qa-recon-and-site-map
description: Use when you must discover everything a website or app contains before testing, so no page, route or feature is missed.
---

# QA Recon and Site Map

Look before you act. Load the app, let it settle (wait for network idle or a stable DOM), then inventory.

## Sources of truth, in order
1. Rendered navigation: header, footer, sidebar, user menu, breadcrumbs, command palette, hamburger menu.
2. `/sitemap.xml`, `/robots.txt`, `/manifest.json`, feed URLs.
3. Links inside page content, buttons that navigate, cards, tables with row links.
4. Authenticated areas: repeat recon after login, and per role (user, admin, guest) if roles exist.
5. Network panel: API routes and lazy loaded chunks hint at hidden screens.
6. Source routes if the codebase is available (router config, page directories). Compare with what the UI exposes.
7. Standard probes: `/404`, `/login`, `/signup`, `/account`, `/settings`, `/admin`, `/privacy`, `/terms`, `/contact`, `/search`, `/cart`, `/checkout`. Only report ones that exist or should.

When a codebase is available, list page and route files (for example `app/**/page.tsx`, `app/api/**/route.ts`) to find screens the UI hides behind login. Probe logged out access with HEAD or page GETs only, and record where each route redirects (login, 403, 404). Check `<h1>`, `<main>`, `lang`, and form `autocomplete` attributes while on each page.

Collapsed menus hide links. Expand every group (or collect hrefs in script after clicking each group button, then restore). Compare three lists and flag mismatches: sidebar links, dashboard quick actions and in page buttons, and probed routes. Typical findings: quick action links that 404, "new" pages that 500, and working pages with no menu entry (orphans).

Also probe sibling routes that a module implies (list page `/x/orders` implies `/x/customers`, `/x/new`, `/x/1`). Whole modules can exist with no menu entry.

Once logged in, a scripted `fetch(path)` per page route from the browser (status only, GET page routes) quickly finds 404 and 500 pages. Confirm each in the UI before filing. A 500 or crash screen with a digest and no navigation is at least P1 when it blocks a main action.

## Capture for every page or screen
- URL or screen name, title, purpose, auth requirement, role.
- Interactive elements count by type: links, buttons, inputs, selects, toggles, file inputs, menus, modals, tabs, media.
- States: empty, loading, populated, error, disabled.
- Entry points (how a human gets there) and exits.
- A screenshot at desktop width.

## Output
A site map table saved to the report folder:

| ID | Route or screen | Needs login | Role | Elements | States | Source found |
|---|---|---|---|---|---|---|

Also list: external links (check they open and are not broken, do not test third party sites deeper), downloadable files, embedded media, third party widgets, and features behind flags.

## Done when
Every link in navigation and footer resolves to an ID in the table, and a second pass finds no new routes.
