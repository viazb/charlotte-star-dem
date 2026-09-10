## What this is

A component-based rebuild of the Charlotte Star Service site (the plain
HTML/CSS/JS version lives one directory up, at the repo root — see its
own CLAUDE.md). This exists specifically to fix the pain of that
version: the nav/footer/buttons are copy-pasted across all 4 HTML
files, so every change had to be repeated 4 times by hand. Here they're
components, edited once.

Requires **Node 22.12+** (`.nvmrc` pins `22`; run `nvm use` before
anything else if your shell defaults to an older Node).

## Design system

`src/styles/global.css` holds the ported design tokens (`:root`
variables for colors/fonts — same values as the root site's
`css/style.css`) plus base resets, typography, `.container`, and
`.section-label`/`.divider` helpers. Page- and component-specific
styles live in each component's scoped `<style>` block instead of one
shared stylesheet — that's the main structural difference from the
root site.

## Components (`src/components/`)

- `SiteHeader.astro` — top banner + nav as one unit (was duplicated
  across 4 HTML files before). Nav links are a single array at the top
  of the file; add/reorder there, not per-page. Includes the
  scroll-spy script (only activates on pages with `#home`/`#services`/
  `#contact` sections) and the traveling gold accent line on the nav's
  own bottom edge.
- `SiteFooter.astro` — footer, same nav-links duplication problem
  solved the same way.
- `Button.astro` — `<Button href="..." variant="primary|outline">`,
  replaces copy-pasting `<a class="btn btn-primary">` everywhere.
- `Carousel.astro` — generic carousel wrapper around **Embla Carousel**
  (`embla-carousel` npm package, github.com/davidjerleke/embla-carousel
  — framework-agnostic, no React). Each direct child passed in must be
  pre-wrapped in `<div class="embla__slide">...</div>`; the component
  supplies the viewport/container scaffold, prev/next buttons, and dot
  indicators, and its script auto-initializes every `[data-carousel]`
  it finds on the page (safe to use more than once per page). Default
  is 1 slide per view on mobile, 2 from 720px up — see the component's
  `<style>` for how to override that per page.
- `TestimonialCard.astro` — small presentational card, demonstrates
  composing a component inside a `Carousel` slide.

`src/layouts/BaseLayout.astro` wraps `<html>`/`<head>` (imports
`global.css`, sets title/meta) and renders `<slot />` for the page body.

## Adding a new page

Add a `.astro` file under `src/pages/` (Astro's file-based routing —
`src/pages/warranty.astro` becomes `/warranty`). Wrap it in
`BaseLayout`, drop in `SiteHeader` and `SiteFooter`, and build the
middle from existing components plus new ones as needed — that's the
whole point of this version over the plain-HTML site.

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
