# zurichbiosecurity.ch

Static site for the Zurich Biosecurity Hub. Plain HTML and CSS, no build step.
What is in this repository is what gets served.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site. One self-contained file: styles in a `<style>` block, the hub logo as inline SVG, the programme marks and the four team portraits as base64 `data:` URIs. Nothing external except the Google Fonts stylesheet and the Luma iframe. |
| `favicon.svg` | Petrol square with the reverse hub mark. |
| `og-image.png` | 1200 x 630 link preview image, rebuilt 11 September 2026 on the light-field art direction: deep petrol ground, cool and warm light sources, hexagon mesh and grain, hub mark, name, strapline, the four portraits. This is what LinkedIn, WhatsApp, Slack and Google show when the site is shared. The source lives with Anatole; there is no generator in this repository any more. |
| `.gitignore` | One line, `.DS_Store`, so macOS folder metadata never gets committed. |
| `.nojekyll` | Stops GitHub Pages from running the content through Jekyll. |
| `CNAME` | Created by GitHub when the custom domain is set in Settings, Pages. Do not delete. |

**Everything is in one file on purpose.** Earlier versions used a separate
`styles.css` and an `assets/` folder, and both broke the moment the file was
opened or shared on its own: unstyled text, then missing images. If the site
grows to a second page, split the CSS out then, not before.

The file is about 170 KB, most of it the nine inlined images. That is light for
a page with four photographs and it costs one request instead of ten.

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
| Cover screen | `#top` | **petrol** |
| What we offer, five rows | `#offer` | paper `#F4F1EA` |
| Who we are, intro and three people | `#who` | **petrol** |
| Why this matters, four numbers | `#why` | paper |
| Upcoming events, Luma embed | `#events` | **petrol** |
| Programmes | `#programmes` | paper |
| … Lectures and workshops | `#lectures` | |
| … Fellowship | `#fellowship` | |
| … Paper reading group | `#reading` | |
| … Career advising | `#advising` | |
| … Coworking space | `#coworking` | |
| Get in touch | `#contact` | **petrol** |
| Footer | | **petrol** |

The stat cards in "Why this matters" stay light whichever field they sit on, so
a few rules carry a `.field--dark .stat …` override to beat the dark link and
text colours. Same for the sticky header: it takes its background from
`.field--dark`, and setting `background` on `.header` as well would silently win
on source order and make the light-on-light lockup invisible.

## The cover screen

The first screen is sized `calc(100svh - var(--header-h))` and centres its
content, so it fills exactly one screen on any device. Three rules keep it
intact and should not be relaxed without re-testing:

1. **The wordmark holds one line.** `white-space: nowrap` plus
   `font-size: clamp(22px, 7vw, 76px)`. The 7vw coefficient is the largest that
   still fits "Zurich Biosecurity Hub" at 320px wide. A longer name would need a
   smaller coefficient.
2. **The strapline holds two lines**, from 320px to 2000px, via
   `clamp(14px, 4.2vw, 27px)` with `max-width: 38ch`. Editing that sentence means
   re-checking the line count.
3. **`svh`, not `vh`.** On mobile browsers `100vh` is the height with the address
   bar hidden, so the call to action falls below the fold until the user scrolls.

The portrait row is capped by `min(980px, 112vh)`, that is by height as well as
width, so the cover still fits on a short laptop screen. Below 820px it becomes
two by two: four across on a tablet in portrait gave 118px faces.

Vertical rhythm inside the cover is deliberate and unequal: about 14px between
the wordmark and the strapline, 32px before the button, 52px before the faces.
Equal gaps make the four blocks read as one flat list.

## Brand

Colours, type scale and spacing follow the Zurich Biosecurity Hub brand kit v1.0:
petrol `#12333B`, warm off-white `#F4F1EA`, burnt orange `#C8552A` (calls to
action only), muted slate `#6E8C93`. IBM Plex Sans throughout, IBM Plex Mono for
dates and sources. 8px spacing scale, hairline rules, no shadows, no gradients.

Three agreed departures from the kit:

1. **Rounded corners** on cards, panels, buttons and portraits (`--r: 14px`,
   `--r-sm: 8px`, buttons fully rounded). The kit calls for radius 0.
2. **Secondary running text** (notes, source lines, meta lines) is petrol at 70%
   rather than slate. Slate on a light field is only 3.2:1, which fails WCAG AA
   at those sizes. Slate is kept for the uppercase micro-labels.
3. **The dark bands are not flat petrol.** Since 11 September 2026 every
   `.field--dark` section sits on a deeper base `#0D2830` with two soft light
   sources and a film grain, see the next section. The kit says no gradients;
   this is the one place where that rule is set aside.

The hub logo is never rounded or altered. It is inlined as SVG `<symbol>`s at the
top of `index.html`: `#mark`, `#mark-accent` (orange inner portal, used in the
header and the footer) and `#mark-hairline` (thin, the watermark in the contact
block). Stroke weights are fixed at 14 outer and 12 inner.

## The light field on the dark bands

Added 11 September 2026. Every `.field--dark` section carries a two-source light
field instead of flat petrol: one cool source (`#6FD2E2`) and one warm source
(`#FF9468`) as large soft radial gradients in `screen` blend mode over a deeper
base, plus a film grain. It is pure CSS in the `<style>` block, using `::before`
for the light and `::after` for the grain, so there is no extra markup and no
extra request. The grain is an inlined SVG `feTurbulence` as a `data:` URI.

Rules that hold it together:

1. **The base is `#0D2830`, not the kit petrol `#12333B`.** On the flatter petrol
   a warm light in `screen` mode turns brown and grey instead of staying
   chromatic.
2. **The sticky header keeps the flat base** and has its pseudo-elements
   disabled. The cover is dark at its top edge, so the two meet without a step.
   The header also re-asserts `position: sticky` in `.header.field--dark`:
   the light-field block sets `position: relative` on `.field--dark` with equal
   specificity later in the sheet, and without the re-assertion the header
   scrolls away with the page.
3. **Outside the cover the sources sit at roughly 40% and are pushed past the
   edges**, so no running text crosses a bright core.

**Contrast was measured, not eyeballed.** Every text run inside a dark band was
checked against the field underneath it at its own font size and weight. All
pass WCAG AA: the smallest margin is the cover wordmark at 3.45:1 against a 3.0
requirement, and the tightest small-text run is the "Upcoming events" body copy
at 4.65:1 against 4.5. **If you move a light source, re-run that check.**
Shifting the cool source up or down by ten percent was enough to push the
founder captions from 11.7:1 to 3.5:1, which fails.

Below 820px the cover swings its two sources to a diagonal and enlarges them.
At the desktop placement they fall off the sides of a phone and the cover reads
as flat again.

The field is removed in `@media print`, where the sections fall back to `#12333B`.

## Programme marks

The five marks are inlined as 192px base64 PNGs, about 2 KB each. They are
downscaled from the 1024px originals in the "ZHBH Logos and Design Materials"
folder, which is not part of this repository. To swap one, resize the original to
192px, base64 it, and replace the `data:image/png;base64,…` string in the
matching `<img>`. Each mark appears twice, once in "What we offer" and once in
"Programmes", so both copies need replacing.

They are opaque petrol squares, so the rounded corners come from CSS, and on a
petrol field only the glyph shows. A 1px off-white ring
(`.field--dark .prog__mark`) keeps the tile shape readable there.

`mark-coworking` is **not** an original. There was no coworking mark in the
folder, so it was drawn in the same visual language (desk shape, off-white with
one slate element, same glyph box as the others). Replace it when a real one
exists.

The marks sit next to a visible heading everywhere they are used, so their `alt`
attribute is deliberately empty.

## Team portraits

Four portraits, inlined as square base64 JPEGs at quality 84. **Target size is
480px**, not 320: the largest display size is 227 CSS pixels, which is 454 device
pixels on a 2x screen, and a 320px image gets upscaled there and looks soft. As of
3 September 2026 Erik, Chau and Illia are at 480px. Erik and Illia come from large
originals; Chau's source is a 1600px photo with a 333px face crop, so her tile is
upscaled by about 1.4x and is a touch softer than those two. Anatole is still 320px
and needs re-exporting from his original. Each portrait appears twice, in the
cover screen (`.founder img`) and again in "Who we are" (`.person img`), so
replacing a person means replacing both copies of that `data:` string, and
asking Anatole to rebuild `og-image.png`.

**They are framed to a common rule, not cropped by eye.** Each face was located
with a detector, then the square was cut so the face box fills a set share of the
frame and sits at a set height. Without this, one portrait's head can be 40%
larger than another's in the same row, which reads as a mistake even when nobody
can say why. The current values: the face fills 55% of the frame at 46% height,
except Erik (50%, 54.5%, because his hair sits well above the detector's box and
a tighter crop clipped it). Illia's portrait was replaced on 3 September 2026
with a 2500px studio photo and uses the default rule. Chau's was replaced the same
day at her request with a wider version of her LinkedIn photo (1600 x 1200), also
on the default rule.

To add or swap one: locate the face, crop square to the same proportion, resize
to 480px, save as JPEG quality 84, base64 it. Larger than 480 is wasted. The
source crop should be at least 480px wide before resizing, otherwise the result
is upscaled and soft; ask for a bigger original rather than enlarging.

The portraits carry real `alt` text in the cover screen (the name) and empty
`alt` in "Who we are", where the name is already the adjacent text.

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
(`https://luma.com/zurich-biosecurity-hub`) needs updating in that case. The iframe
has a fixed height because Luma embeds do not resize themselves.

**The embed was verified in a browser on 2 September 2026** and renders the Hub
calendar correctly. The static fallback list of lecture dates that used to sit
below it as an HTML comment has been removed.

**One calendar, renamed.** `luma.com/zurich-biosecurity-hub` and the older
`luma.com/zurich-biosecurity` are the same calendar (same API id); it was renamed
from "Biosecurity Reading Group" to "Zurich Biosecurity Hub" in August 2026 and
now carries the lecture series as well as the reading group. Use the new slug
everywhere; the old one may stop resolving.

## Calls to action

There is exactly one call to action on the page, "Join us at our next event".
It appears four times: the header button, the cover screen, below the calendar
and in the closing band. All four point at the calendar:

```
https://luma.com/zurich-biosecurity-hub
```

An earlier draft pointed them at the next individual session instead. That is one
click shorter but goes stale after every event and has to be repointed by hand,
so the calendar was chosen. If you ever switch back to a dated event link, search
the file for `zurich-biosecurity-hub` and note the expiry in this README.

## Contact routes

Three, linked in this order of prominence:

1. **Luma calendar**, `https://luma.com/zurich-biosecurity-hub`: the button, see
   above.
2. **"Introduce yourself" form**,
   `https://airtable.com/apphuE6g3JDyct5L1/pagnomXiTyLYjOwrj/form`: a text link
   in three places, under the Luma button in "Get in touch", in the Career
   advising paragraph, and in the footer list "Elsewhere". Not in the navigation,
   not a button. All three open in a new tab (`target="_blank" rel="noopener"`).
   If the URL changes, search the file for `airtable.com`.
3. **Mail**, `info@zurichbiosecurity.ch`: a text link in Career advising and in
   the footer.

## Typography rules baked into the CSS

The body copy is not capped at a reading measure. On a 1240px container that
gives long lines, but it keeps every paragraph on one or two lines instead of
four short ones floating in white space. If a paragraph grows past two lines on
desktop, shorten it rather than reintroducing a `max-width`.

Two mechanisms prevent bad line breaks:

- **A non-breaking space binds the last two words of every paragraph.** Search for
  `&nbsp;` before a closing `</p>`. This guarantees no single word is left alone
  on the last line. When you rewrite a paragraph, put the `&nbsp;` back.
- **`text-wrap: balance` on desktop, `pretty` on mobile.** Balance evens line
  lengths, which is right on a wide field and wrong in a narrow column, where it
  leaves a stubby first line.

`COVID‑19` and `BSL‑4` use a non-breaking hyphen (U+2011) so they never split
across two lines inside the narrow stat cards.

## Editorial rules for this site

- The site is public and external facing. No internal strategy, no funnel or
  target numbers, no funder-facing arguments, no mention of the fiscal host.
- No em dashes.
- British spelling: programme, organised, defence, colour.
- **"Zurich" without an umlaut everywhere**, including ETH Zurich and University
  of Zurich. Both institutions use those spellings as their own English names,
  and the site is in English. This replaces the earlier rule, which kept "Zürich"
  outside the Hub's name.
- ETH Zurich is named only for the lecture series, which does take place there.
  The Hub itself is not branded as an ETH project, and no institutional logos
  appear on the site, because that would imply an endorsement that does not exist.
- Contact is always `info@zurichbiosecurity.ch`, never a personal address.

## Accessibility

Checked and passing: keyboard focus is visible, the skip link works, headings are
in order, images carry appropriate `alt` text, and `prefers-reduced-motion` is
respected.

One known failure, accepted deliberately: **off-white `#F4F1EA` on the orange
button `#C8552A` is 3.9:1, under the WCAG AA threshold of 4.5:1 for text at that
size.** Darkening the button to about `#BC4A1F` would pass. The kit colour was
kept on purpose; this is a decision, not an oversight.

## Verified

Layout was measured, not eyeballed, at 320, 360, 390, 430, 768, 1024, 1280, 1440
and 2000 pixels wide. On every one: the wordmark holds one line, the strapline
holds two, the cover screen fits the viewport exactly, no paragraph ends with an
orphan word, and the page never scrolls horizontally.

## Open

- `info@zurichbiosecurity.ch` needs a mailbox or a forward. Until then every
  contact link on the site goes nowhere.
- Room for the lecture series ("room to be announced").
- The events section carries one hand-written line about the Swiss AI Safety
  Days (7 and 8 November 2026, ETH Zurich). It is the only event on the page that
  is not on the Luma calendar; remove or replace it after the conference. If the
  conference is added to the Hub's Luma calendar as a featured event, the line
  becomes redundant.
- Fellowship applications are open (September 2026). The text links to the
  calendar; swap in the direct Luma event URL once it is to hand, and set the
  text back to "announced when applications open" when the round closes.
- The coworking paragraph describes PEAKS as a space for people working on the
  largest risks, with AI safety and the Hub named. PEAKS' own site still says
  "EA and AI Safety research"; Erik is updating it.
- The four people have agreed to be listed. Confirm they also agree to the
  photographs.
- The paper reading group is hybrid (UZH Irchel plus online). Keep that in both
  places it is mentioned, "What we offer" and "Programmes".
- Fonts load from Google Fonts. Self-hosting IBM Plex would remove that
  third-party request and would also make the 450 "Text" weight available.

## Hosting

GitHub Pages, served from `main`.
