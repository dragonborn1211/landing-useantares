# Antares — observatory & guesthouse

The site of useantares.com: a private observatory with an eight-room guesthouse at 2,140 m.
The telescope is called the Antares. A design project by Private Technology House.

Static HTML, CSS and a little JavaScript: no framework, no build step, no dependencies.

## Run

```bash
npx http-server -p 8080 .        # or: python -m http.server 8080
```

Open http://localhost:8080. Opening `index.html` directly also works.

## Structure

```
index.html            all sections
assets/css/main.css   tokens, sections, tonight panel, responsive
assets/js/main.js     content (OBJ, LAY), Moon maths, menu, booking form, background
assets/img/           stars.webp (night windows), trails.webp (background without JS), og.jpg (link preview)
assets/fonts/         Cormorant Garamond, Manrope, JetBrains Mono (WOFF2, Latin subset) + OFL licences
robots.txt, sitemap.xml
tools/build_stars.py  regenerates stars.webp and stars-960.webp (Python, numpy, Pillow)
```

## Sections and where they link

| Header / footer | Section | id |
|---|---|---|
| — | Dark sky. Big glass. — a typical clear night and the evening programme | `#top` |
| Nights | five packages with prices | `#nights` |
| Observatory | what's on the mountain | `#observatory` |
| Grounds | telescope, imaging pier, guesthouse, tonight's chart | `#grounds` |
| Eye to star | seven layers between your eye and Antares | `#sky` |
| Your night | how a night goes, six steps | `#your-night` |
| House rules | five rules + the booking form | `#rules`, `#book` |

## What is live

- **Background: star trails.** A long exposure of the sky drawn on canvas around the south celestial
  pole. It builds up on load, turns slowly and turns further as you scroll; now and then a meteor
  crosses. Still frame with reduced motion; `trails.webp` without JS.
- **Tonight** (header toggle, key `T`, object `04`). Moon illumination, phase, next new and full
  moon and the darkest nights ahead, worked out for today's date. The same figures fill the Moon
  card in the first screen, the chart legend, the first house rule and the footer.
- **Menu** — button in the header (also ⌘K / Ctrl+K): search across sections and contacts.
- **Booking form** — see below.

The first screen shows *a typical clear night*: cloud, seeing and target altitudes are fixed
example values. Only the Moon follows the date.

## Edit

- **Places** — `OBJ` in `assets/js/main.js` (name, category, text, specs, URL in the window bar,
  link label). Previews are `.pv[data-preview="N"]` in `index.html`, drawn in HTML/CSS/SVG.
- **Layers** — `LAY` in the same file (title, text, specs, the facts card).
- **Contacts** — `CONFIG` at the top of `assets/js/main.js`.

## Booking form and contacts

`CONFIG` decides where requests go and which contacts show:

| Key | Effect |
|---|---|
| `endpoint` | URL that accepts `POST application/json` `{ night, contact, page, lang, ts }` |
| `email` | footer link, "Copy our email" in the menu, mailto fallback for the form |
| `whatsapp` | number, digits only: footer link and WhatsApp fallback for the form |
| `instagram` | handle without @: footer link |

An empty value hides its link. With none of `endpoint`, `email` and `whatsapp` set the form says
"Bookings open soon" and the button is disabled. A hidden field catches bots, and one browser can
send once every 20 seconds; a failed request says so.

## Deploy: GitHub Pages on useantares.com

The repository root is the site. `CNAME` holds the domain, `.nojekyll` tells Pages to serve the
files as they are. Every push to `main` redeploys within a minute or two.

DNS (Cloudflare, *DNS only*): four `A` records on `@` — 185.199.108.153, 185.199.109.153,
185.199.110.153, 185.199.111.153 — and `CNAME www → dragonborn1211.github.io`.
