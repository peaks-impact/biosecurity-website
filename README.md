# zurichbiosecurity.ch

Static site for the Zurich Biosecurity Hub. Plain HTML and CSS, no build step.
What is in this repository is what gets served.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site. One self-contained file: styles in a `<style>` block, the hub logo as inline SVG, the programme marks as base64 `data:` URIs. Nothing external except the Google Fonts stylesheet and the Luma iframe. |
| `favicon.svg` | Petrol square with the reverse hub mark. |
| `og-image.png` | 1200 x 630 link preview image. |
| `.nojekyll` | Stops GitHub Pages from running the content through Jekyll. |
| `CNAME` | Created by GitHub when the custom domain is set in Settings, Pages. Do not delete. |

**Everything is in one file on purpose.** Earlier versions used a separate
`styles.css` and an `assets/` folder, and both broke the moment the file was
opened or shared on its own: unstyled text, then missing images. If the site
grows to a second page, split the CSS out then, not before.

## Editing

Open `index.html` in a browser to preview locally, no server needed. Design
tokens are the CSS custom properties in `:root`.

```
git pull
# edit
git add -A && git commit -m "..." && git push
```

Pushes to `main` go live.

## Page structure

Fields alternate per section, starting dark.

| Section | Anchor | Field |
|---|---|---|
| Header, sticky | | **petrol `#12333B`** |
| Our mission, with the large mark | `#mission` | **petrol** |
| What we offer, five rows | `#offer` | paper `#F4F1EA` |
| Why this matters, four numbers | `#why` | **petrol** |
| Upcoming events, Luma embed | `#events` | paper |
| Programmes | `#programmes` | **petrol** |
| … Lectures and workshops | `#lectures` | |
| … Fellowship | `#fellowship` | |
| … Paper reading group | `#reading` | |
| … Career advising | `#advising` | |
| Team | `#team` | paper |
| Get in touch | `#contact` | **petrol** |
| Footer | | **petrol** |

The stat cards in "Why this matters" stay light inside the dark field, so a few
rules carry a `.field--dark .stat …` override to beat the dark link and text
colours. Same for the sticky header: it takes its background from
`.field--dark`, and setting `background` on `.header` as well would silently win
on source order and make the light-on-light lockup invisible.

## Brand

Colours, type scale and spacing follow the Zurich Biosecurity Hub brand kit v1.0:
petrol `#12333B`, warm off-white `#F4F1EA`, burnt orange `#C8552A` (calls to
action only), muted slate `#6E8C93`. IBM Plex Sans throughout, IBM Plex Mono for
dates and sources. 8px spacing scale, hairline rules, no shadows, no gradients.

Two agreed departures from the kit:

1. **Rounded corners** on cards, panels, buttons and programme marks
   (`--r: 14px`, `--r-sm: 8px`, plus `12px` on the mark tiles). The kit calls for
   radius 0. Revert by setting them to `0`.
2. **Secondary running text** (notes, source lines, meta lines) is petrol at 70%
   rather than slate. Slate on a light field is only 3.2:1, which fails WCAG AA
   at those sizes. Slate is kept for the uppercase micro-labels.

The hub logo is never rounded or altered. It is inlined as SVG `<symbol>`s at the
top of `index.html`: `#mark`, `#mark-accent` (orange inner portal, used in the
header, the mission block and the footer) and `#mark-hairline` (thin, the
watermark in the contact block). Stroke weights are fixed at 14 outer and 12
inner.

## Programme marks

The five marks are inlined as 192px base64 PNGs, about 2 KB each. They are
downscaled from the 1024px originals in the "ZHBH Logos and Design Materials"
folder, which is not part of this repository. To swap one, resize the original to
192px, base64 it, and replace the `data:image/png;base64,…` string in the
matching `<img>`.

They are opaque petrol squares, so the rounded corners come from CSS, and on a
petrol field only the glyph shows. A 1px off-white ring
(`.field--dark .prog__mark`) keeps the tile shape readable there.

`mark-coworking` is **not** an original. There was no coworking mark in the
folder, so it was drawn in the same visual language (desk shape, off-white with
one slate element, same glyph box as the others). Replace it when a real one
exists.

The marks sit next to a visible heading everywhere they are used, so their `alt`
attribute is deliberately empty.

## Figures on the page

Every number in "Why this matters" is sourced and linked. If you edit one, keep
the link and the year:

- 14.9 million excess deaths, 2020 and 2021: WHO, May 2022.
- 2% annual probability of a COVID-scale pandemic: Marani et al., PNAS 2021.
- 4 to 10% chance an engineered pathogen kills over 1% of humanity by 2100:
  forecaster and domain-expert estimates collected by 80,000 Hours, 2024.
- 1 of 27 countries with a BSL-4 lab has comprehensive dual-use research
  oversight: Global BioLabs Report 2023.
- About 1 billion dollars a year on catastrophic biological risk against roughly
  130 billion on outbreak preparedness: 80,000 Hours, 2024.

## Events calendar

The "Upcoming events" section embeds the Luma calendar:

```
https://luma.com/embed/calendar/cal-M6JthEVC9b0Ua7M/events
```

`cal-M6JthEVC9b0Ua7M` is the calendar's API id. It stays the same if the calendar
is renamed or its public slug changes, so only the human-facing link
(`https://luma.com/zurich-biosecurity`) needs updating in that case. The iframe
has a fixed height because Luma embeds do not resize themselves.

**The embed has not been verified in a browser yet.** A static fallback list of
the five lecture dates sits directly below it in `index.html` as an HTML comment.
If the embed does not render, delete the `.embed` block and uncomment the
`.dates` block.

## Team list

`.member` rows are a three-column grid: optional photo, text, optional link.
A photo would need a real image file, which means creating an `assets/` folder
again and pushing it; the row markup is
`<img class="member__photo" src="assets/team-name.jpg" alt="" width="56" height="56">`
as the first child of the row. Nothing else changes.

Three rows are live: Erik Jentzen, Chau Tran, Illia Onipko. All three agreed to
be listed. LinkedIn links are still missing; the `.member__link` paragraph in
each row is empty and ready for them. Do not leave an `<a>` with an empty `href`
in there, it links back to the page.

## Editorial rules for this site

- The site is public and external facing. No internal strategy, no funnel or
  target numbers, no funder-facing arguments, no mention of the fiscal host.
- No em dashes.
- British spelling: programme, organised, defence, colour.
- "Zurich Biosecurity Hub" without an umlaut as the name; "Zürich" and
  "ETH Zürich" with one everywhere else.
- ETH Zürich is named only for the lecture series, which does take place there.
  The Hub itself is not branded as an ETH project.
- Contact is always `info@zurichbiosecurity.ch`, never a personal address.

## Open

- `info@zurichbiosecurity.ch` needs a mailbox or a forward. Until then every
  contact link on the site goes nowhere.
- Room for the lecture series ("room to be announced").
- Whether the lecture series stays "free, no registration" or moves to Luma
  registration.
- Fonts load from Google Fonts. Self-hosting IBM Plex would remove that
  third-party request and would also make the 450 "Text" weight available.
- The orange call-to-action button is off-white on `#C8552A`, 3.87:1, below
  WCAG AA for text at that size. Not solvable inside the palette.

## Hosting

GitHub Pages, served from `main`.
