# zurichbiosecurity.ch

Static site for the Zurich Biosecurity Hub. Plain HTML and CSS, no build step —
what is in this repository is what gets served.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site. Single page, anchor navigation, styles inlined in a `<style>` block so the file previews correctly on its own. |
| `favicon.svg` | Petrol square with the reverse mark. |
| `.nojekyll` | Stops GitHub Pages from running the content through Jekyll. |
| `CNAME` | Created by GitHub when the custom domain is set in Settings → Pages. Do not delete. |

## Editing

Open `index.html` in a browser to preview locally — no server needed. Styles live
in the `<style>` block at the top of the file; brand tokens are the CSS custom
properties in `:root`. If the site ever grows to a second page, pull that block
out into `styles.css` rather than duplicating it.

```
git pull
# edit
git add -A && git commit -m "..." && git push
```

Pushes to `main` go live.

## Brand

Colours, type scale and spacing follow the Zurich Biosecurity Hub brand kit v1.0:
petrol `#12333B`, warm off-white `#F4F1EA`, burnt orange `#C8552A` (calls to
action only), muted slate `#6E8C93`. IBM Plex Sans throughout, IBM Plex Mono for
dates, times and room codes. 8px spacing scale, hairline rules, no shadows, no
border radii, no gradients.

The logo is inlined as SVG `<symbol>`s at the top of `index.html`
(`#mark`, `#mark-accent`, `#mark-hairline`) and inherits `currentColor`.
Do not redraw or alter the stroke weights.

## Hosting

GitHub Pages, served from `main`. Fonts load from Google Fonts; self-hosting them
would remove that third-party request if it becomes a concern.
