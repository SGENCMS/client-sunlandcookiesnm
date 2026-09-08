# client-sunlandcookiesnm

Static clone of the Sunland Park Cookies site, published as a preview.

**Preview:** https://sgencms.github.io/client-sunlandcookiesnm

| | |
|---|---|
| Source | `https://sunlandcookiesnm.staging.sgen.com/` |
| Cloned | 2026-09-08 |
| Shape | Single page |
| Build step | None. Pure static — open `index.html` or serve the directory. |

## Fidelity

Verified against the live source with `pixelmatch`, full-page, at the six canonical breakpoints:

| Viewport | Match | Mismatched pixels |
|---|---|---|
| 1440×900 | 100.000% | 0 / 9,838,080 |
| 1280×800 | 100.000% | 0 / 8,577,280 |
| 1024×768 | 100.000% | 0 / 6,650,880 |
| 768×1024 | 100.000% | 0 / 4,953,600 |
| 430×932 | 100.000% | 0 / 3,738,850 |
| 390×844 | 100.000% | 0 / 3,373,110 |

Measured three times — from `file://`, served over HTTP from the `/client-sunlandcookiesnm/`
subpath this site is published under, and again after unreferenced assets were pruned. All three
runs returned zero mismatched pixels across all 37,132,470 compared.

## Layout

```
.
├── .nojekyll        REQUIRED — without it Pages runs Jekyll, which drops _xorigin/
├── index.html
├── chrome.css       every source stylesheet, consolidated in document order
├── assets/          same-origin assets, source paths mirrored verbatim
│   ├── in-pages/    images referenced by the page
│   └── full-library/ every image captured from the source
├── _xorigin/        cross-origin assets (webfonts), foldered by host
├── shop/  sites/    mirrored source subtrees
└── README.md
```

`.nojekyll` is load-bearing. Jekyll excludes any path beginning with an underscore, which would
silently 404 all 122 webfonts under `_xorigin/fonts.gstatic.com/`.

## What differs from source

This is a preview mirror, not a functioning storefront. Deliberate changes:

- **Forms are inert.** Cloned forms keep the source's `action`, so a visitor submitting one would
  send their data to the client's live server. Both forms had their action neutralised, the
  captured server-issued security tokens blanked, and submissions are stopped in the capture phase
  before any inherited script can see them. Verified: submitting the newsletter field produces
  zero network requests.
- **Analytics and tracking removed** — GTM, Clarity, Klaviyo, and the first-party `sg-collect`
  beacons, along with their vendor bundles. The preview reports to nothing.
- **Stylesheet links back to the source server removed.** Their content is already inside
  `chrome.css`, so the page does not re-fetch CSS from the source at view time.

Preserved verbatim: all markup, styling, imagery, and the site's own presentation JS.

## Known limitations

- **Single page.** Only the homepage was cloned. Links to other routes (`/about`, `/contact`,
  `/shop/...`) are inert.
- **One outbound request.** The preserved product JS requests live cart state from the source. No
  backend exists behind a static clone, so it fails harmlessly and the rendered page is unaffected.
- **Point-in-time.** Product listings, pricing and promotional imagery reflect the source at clone
  time and will drift.
- `assets/full-library/` holds every image captured from the source, including ones the page does
  not reference. Kept for reuse; it costs almost nothing in the packed repo.

## Ownership

Client content, cloned for preview. Rights remain with the client. The page carries
`<meta name="robots" content="noindex, nofollow">`. Confirm permission before circulating this URL.
