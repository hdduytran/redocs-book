# Reader experience

The [[app:reader]] opens `index.html` and lands on the current milestone. The screenshots below were taken from this book with `scripts/screenshot-pages.mjs` (1280×900).

## A page with a diagram

![The System context page of this book: chapter sidebar, milestone bar, embedded archify diagram](asset:screenshots/redocs-system-context.png)

- **Header**: book title, milestone bar (planned milestones have a dashed border), search box (`/` focuses it), theme toggle.
- **Sidebar**: chapters in `book.json` order, then Catalog and Changes.
- **Right column**: an "On this page" table of contents when a page has at least two h2/h3 headings.

## A generated Changes page

![The generated Changes v1 → v2 page: added entities and pages with new content](asset:screenshots/redocs-changes.png)

## Accessibility

Skip link, `nav`/`main` landmarks, visible focus, horizontally scrolling tables on narrow screens, `prefers-reduced-motion` respected; below 760px the sidebar becomes a toggle menu.

> TODO(owner): Should finalize add an automated contrast check (axe)?
