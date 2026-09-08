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

Compared against the live source with `pixelmatch`, full-page, at the six canonical breakpoints,
with every image decode-awaited so lazy-loading cannot masquerade as a difference:

| Viewport | Match | Mismatched pixels |
|---|---|---|
| 1280x800 | 100.000% | 0 / 8,577,280 |
| 1024x768 | 100.000% | 0 / 6,650,880 |
| 768x1024 | 100.000% | 0 / 4,953,600 |
| 430x932 | 100.000% | 0 / 3,738,850 |
| 390x844 | 100.000% | 0 / 3,373,110 |

Those five are exact.

**1440x900 cannot be quoted honestly.** That width renders a product grid whose contents the
source varies per request: two consecutive loads of the source itself differ there by 235,686
pixels (97.604%). The clone measures **the same 235,686 pixels** against the source — that is, its
entire deviation at 1440 is the source's own request-to-request variance, and nothing else.
A mirror is a frozen snapshot, so a diff at that width measures which products each side happened
to serve rather than the fidelity of the clone.

All 12 acceptance gates pass, with 0 hard issues.

## Layout

```
.
├── .nojekyll        REQUIRED — without it Pages runs Jekyll, which drops _xorigin/
├── .gitattributes   * -text — never rewrite line endings
├── index.html
├── chrome.css       every source stylesheet, consolidated in document order
├── assets/          same-origin assets, source paths mirrored verbatim
├── _xorigin/        cross-origin assets (webfonts), foldered by host
├── shop/  sites/    mirrored source subtrees
└── README.md
```

`.nojekyll` is load-bearing. Jekyll excludes any path beginning with an underscore, which would
silently 404 all 122 webfonts under `_xorigin/fonts.gstatic.com/`.

## What differs from source

A preview mirror, not a working storefront. Deliberate changes:

- **Fully self-contained.** The clone makes no request to the source, on load or on interaction —
  verified with every off-origin request recorded and blocked. The previous published version of
  this mirror POSTed to the source's cart endpoint on every page load; that is fixed.
- **Runtime origin removed.** The source hands its JavaScript a config object holding absolute
  source URLs, which its bundled scripts used to build request URLs at runtime. Those origins are
  stripped, so any such request stays on whichever origin serves this bundle.
- **Forms are inert.** Both forms had their action neutralised, their captured server-issued
  security tokens blanked, and submissions stopped in the capture phase before any inherited
  script sees them. Submitting either produces zero network requests.
- **Analytics and tracking removed** — GTM (including its `<noscript>` iframe fallback), Clarity,
  Klaviyo and the first-party `sg-collect` beacons, along with their vendor bundles.
- **Image `onerror` fallbacks removed.** The source pairs each image with a handler that silently
  re-fetches it from the source origin on failure — which both phones home and hides broken
  images.

Preserved verbatim: all markup, styling, imagery, and the site's own presentation JS.

## Known limitations

- **Single page.** Links to other routes (`/about`, `/contact`, `/shop/...`) point at the source.
- **Cart is not live.** The product widget requests cart state by POST; a static host cannot serve
  it, so that request fails locally. It no longer reaches the source.
- **Point-in-time.** Product listings, pricing and promotional imagery reflect the source at clone
  time and will drift.

## Ownership

Client content, cloned for preview. Rights remain with the client. The page carries
`<meta name="robots" content="noindex, nofollow">`. Confirm permission before circulating this URL.
