# Antares — observatory & guesthouse

Landing page for **useantares.com**: a concept site for a private observatory with an eight-room
guesthouse at 2,140 m. The telescope is called the Antares.
Static HTML, CSS and a little JavaScript: no framework, no build step, no dependencies.
Concept and design by Private Technology House.

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
assets/img/           stars.webp (night windows), trails.webp (background when JS is off)
robots.txt, sitemap.xml
tools/build_stars.py  regenerates stars.webp and stars-960.webp (Python, numpy, Pillow)
```

## Sections and where they link

| Header / footer | Section | id |
|---|---|---|
| — | Dark sky. Big glass. — tonight's conditions and programme | `#top` |
| Nights | five packages with prices | `#nights` |
| Observatory | what's on the mountain | `#observatory` |
| Grounds | telescope, imaging pier, guesthouse, tonight's chart | `#grounds` |
| Eye to star | seven layers between your eye and Antares | `#sky` |
| Your night | how a night goes, six steps | `#your-night` |
| House rules | five rules + the booking form | `#rules`, `#book` |

## What is live

- **Background: star trails.** A long exposure of the sky drawn on canvas: every star leaves the
  same arc around the south celestial pole, just above the top right corner. The exposure builds
  up on load, the sky turns slowly and turns further as you scroll through the page; now and
  then a meteor crosses. Still frame with reduced motion; `trails.webp` when JS is off.
- **Tonight** (header toggle, key `T`, the `00` object). Moon illumination, phase, next new and full
  moon and the darkest nights ahead, from low-precision Sun and Moon positions. Checked against
  the 2026 phases: new moons within ~7 minutes, full moons within ~40. The same figures fill the
  Moon card in the first screen, the chart legend, the first house rule and the footer.
- **Menu** — button in the header (also ⌘K / Ctrl+K): search across sections and contacts.
- **Booking form** — shows the request as it will arrive; sends to `CONFIG.endpoint`, falls back
  to email or WhatsApp.

The first screen is labelled *forecast*: cloud, seeing and target altitudes are a sample night
(altitudes worked out for latitude 30° S in September), not a live feed.

## Edit

- **Places** — `OBJ` in `assets/js/main.js` (name, category, text, specs, URL in the window bar,
  link label). Previews are `.pv[data-preview="N"]` in `index.html`, drawn in HTML/CSS/SVG.
- **Layers** — `LAY` in the same file (title, text, specs, the facts card).
- **Contacts** — `CONFIG` at the top of `assets/js/main.js`: `endpoint`, `email`
  (`hello@useantares.com` — create the mailbox), `whatsapp`, `instagram`.

## Rights

- Every text on the site was written for it. The star field in the night windows and the star
  trails are generated in code (`tools/build_stars.py`, the canvas): no stock images.
- Fonts come from Google Fonts under the SIL Open Font License, free for commercial use.
- Astronomical facts and object names (M4, Rho Ophiuchi, α Scorpii) are public knowledge.
- The design system comes from the Private Technology House studio site.
- **The name.** "Antares" is used by several real observatories (France, Austria, Brazil,
  Argentina) and many companies. Before launching a real business under it, search trademark
  registers in classes 39, 41 and 43 (travel, events and education, lodging) for the target
  countries, and check who owns useantares.com.
- The footer carries a small credit, "Concept by Private Technology House" (also in the
  `author` meta and the source comment). Remove it if the site goes live for a real client.

## Deploy: GitHub Pages on useantares.com

The site is the repository root as it is; there is nothing to build. `CNAME` holds the domain,
`.nojekyll` tells Pages to serve the files untouched.

1. Repository **Settings → Pages → Build and deployment**: Source *Deploy from a branch*,
   branch `main`, folder `/ (root)`, Save. The custom domain fills in from `CNAME`.
2. Cloudflare **DNS** for useantares.com, all records *DNS only* (grey cloud):

   | Type | Name | Content |
   |---|---|---|
   | A | `@` | 185.199.108.153 |
   | A | `@` | 185.199.109.153 |
   | A | `@` | 185.199.110.153 |
   | A | `@` | 185.199.111.153 |
   | CNAME | `www` | dragonborn1211.github.io |

   Remove any other A record on `@`.
3. Back in **Settings → Pages**, wait for the DNS check to pass and the certificate to be issued,
   then tick **Enforce HTTPS**.

Every push to `main` redeploys the site within a minute or two.

## Before going live

- [ ] Real photos: the dome, the rooms, the night sky above the site.
- [ ] Location, road directions and the real altitude; prices and currency.
- [ ] Booking backend for `CONFIG.endpoint`, or a booking service.
- [ ] Mailbox `hello@useantares.com`, WhatsApp and Instagram in `CONFIG`.
- [ ] `og:image` 1200×630, privacy policy and terms.
- [ ] Self-hosted fonts (Cormorant Garamond, Manrope, JetBrains Mono).
