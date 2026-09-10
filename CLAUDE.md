# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static marketing website demo for "Charlotte Star Service," a fictional/demo BMW & Mercedes-Benz repair shop in Charlotte, NC. No build tooling, no package manager, no framework — plain HTML/CSS/vanilla JS served as-is.

## Running locally

There is no build step. Open the HTML files directly in a browser, or serve the directory with any static file server, e.g.:

```
python3 -m http.server 8000
```

There are no tests, linters, or CI configured in this repo.

## Structure

- `index.html` — homepage (hero, services, testimonials, about teaser, contact/map, footer)
- `about.html` — company story, team, values, warranty, brand specialization
- `gallery.html` — photo grid (currently Unsplash stock placeholder images, not real shop photos)
- `faq.html` — accordion FAQ
- `css/style.css` — single shared stylesheet for all pages
- `README.md` — just the repo name, no other content
- `astro-template/` — a separate, in-progress **component-based rebuild** of this same
  site (Astro, Node 22+, see its own CLAUDE.md/AGENTS.md). Exists specifically to fix
  the "repeat-per-page" problem described below — nav/footer/buttons as real components
  instead of copy-pasted HTML. The plain-HTML site above is still the live version;
  nothing in `astro-template/` is deployed. Requires `nvm use 22` (or equivalent)
  before running any `npm`/`astro` commands there — the repo's ambient Node is older.

## Architecture: repeat-per-page, not componentized

Each HTML page is a fully self-contained document that duplicates the same nav, top banner, footer, and floating "Review Us on Google" button markup verbatim. There is no templating system, no includes, and no shared JS file — every page inlines its own `<script>` block at the bottom of `<body>`.

**Practical implication:** structural changes to the nav, footer, or top banner (e.g. adding a nav link, changing the phone number, editing hours) must be repeated identically across all four HTML files. When making such a change, grep across all `.html` files rather than editing just one page.

Common inline `<script>` behaviors, duplicated per page:
- Nav scroll effect: toggles `.scrolled` on `#nav` past 50px scroll.
- Mobile hamburger: toggles `.open` on `#navToggle` / `#mobileMenu`, closes menu on link click.
- `faq.html` additionally has an accordion script (`.faq-question` click toggles `.open` on `.faq-item`, closes all others — single-open accordion).

## Styling conventions (`css/style.css`)

- Dark "luxury" theme driven entirely by CSS custom properties in `:root` (colors: `--gold`, `--bg`, `--bg-card`, text tones; fonts: `--font-serif` = Cormorant Garamond for headings, `--font-sans` = Inter for body/UI). Change the palette/fonts once here rather than hardcoding colors in HTML.
- Google Fonts loaded via `@import` at the top of the stylesheet.
- Layout is section-based: each `<section>` on a page gets its own CSS block, ordered top-to-bottom matching the page (e.g. `HERO`, `SERVICES`, `TESTIMONIALS`, `CONTACT`, `FOOTER`, then page-specific sections like `ABOUT PAGE`, `GALLERY PAGE`, `FAQ PAGE`), separated by large `====` comment banners. Follow this ordering/commenting convention when adding new sections.
- Grid "card" sections (services, testimonials, team, values, gallery) share a pattern: a `display:grid` container with `gap:1px` and a `background: var(--border)` acting as the grid-line color, with individual `*-card`/`*-item` children set to `var(--bg-card)` — this fakes hairline borders between grid cells without individual border declarations.
- Responsive breakpoints are consolidated at the bottom of the file in three `@media` blocks (`1024px`, `768px`, `480px`) rather than inline per-component — add new responsive overrides into the matching existing block.
- Inline `style=""` attributes appear throughout the HTML for one-off tweaks (e.g. gradient overlays, spacing); this is the established pattern for non-reusable styling rather than adding one-off CSS classes.

## Content notes

- Business contact info (phone `704-332-4111`, address `4225 Monroe Rd, Charlotte, NC 28205`, hours Mon–Fri 8–5) is repeated across all pages' banners, contact sections, and footers — update consistently everywhere if it changes.
- Gallery images and the homepage hero photo currently use Unsplash placeholder URLs (`images.unsplash.com/...`), not real shop photography — flagged as such since a real deployment would need actual photos.
- The "Schedule Appointment" button on `index.html` has a `TODO` HTML comment noting it's a placeholder for a future Calendly integration.
- Google Maps is embedded via a plain `<iframe src="https://maps.google.com/maps?q=...&output=embed">` (no API key required for this basic embed).

## Idioma
Responder siempre en español.

## Decisiones bloqueadas (NO deshacer)
- El logo del nav usa la imagen REAL del cliente
  (https://charlottestarserviceinc.com/files/2023/03/logo.png)
  recortada por CSS background-image para mostrar solo el auto (~28% izquierdo).
  NUNCA reemplazarlo por un SVG dibujado a mano ni por un ícono de estrella.
- El logo debe ser idéntico en las 4 páginas: index, about, faq, gallery.
- El overlay del hero de gallery va al 52% de opacidad, no más.

## Bugs ya resueltos (no reintroducir)
- Atributo style duplicado en el hero de gallery
- Menú móvil filtrándose al escritorio
- Hero vacío / en negro
- Botones invisibles por bajo contraste

## Reglas de trabajo
1. Un cambio a la vez, verificado, antes del siguiente.
2. Si una decisión de esta lista estorba, AVISAR antes de cambiarla.
3. Verificar assets en el sitio real del cliente antes de proponer alternativas.
   No generar placeholders si el asset real existe.
4. Entregar la crítica honesta completa de una vez, sin esperar a que la pidan.

## Antes de decir "listo" (obligatorio)
- [ ] Cada src de imagen verificado, ninguna rota
- [ ] Ningún elemento invisible (botones, texto de bajo contraste, secciones vacías)
- [ ] El hero tiene contenido visible
- [ ] Sin desbordes en 375px, 768px y 1440px
- [ ] Menú móvil no aparece en escritorio
- [ ] Enlaces y botones existen y son clickeables

NUNCA declarar algo terminado sin verificarlo.
Si no se puede verificar, decirlo explícitamente.
Si hay un bug conocido pendiente, listarlo ANTES de entregar.