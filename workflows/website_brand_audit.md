# Workflow: Website Brand & Structure Audit

## Objective

Given any website's root URL, produce a visual brand dossier: screenshots of key pages, the site's colour palette and typography (as detected from the live page), a map of its site structure, and a finished report delivered as both a standalone HTML file and a PDF — saved locally for the user.

## Required inputs

- The target site's root URL (e.g. `https://www.example.com/`).
- Optional: a short slug for naming output files (default: derive from the domain, e.g. `asos`).
- Optional: specific sub-pages to screenshot beyond the homepage (default: homepage + up to 2 top-level landing pages, e.g. the site's main category hubs).

## Tools to use

See [firecrawl-cheatsheet.md](../firecrawl-cheatsheet.md) for general Firecrawl tool details. In order:

1. **`firecrawl_map`** on the root URL to get a first pass at indexed URLs. Treat this as a rough signal only — on large commerce/content sites it tends to surface random product or article pages rather than clean navigation.
2. **`firecrawl_scrape`** the homepage with `formats: ["screenshot", "branding", "links"]` and `screenshotOptions: {fullPage: true}`. This one call gets you:
   - a full-page screenshot
   - `branding.colors` (primary/secondary/accent/background/text) and `branding.fonts` (family + usage count) and `branding.typography` (font stacks, sizes)
   - `links` — the homepage's own nav/feature links, which is almost always a cleaner structure map than the raw `firecrawl_map` output
3. **`firecrawl_scrape`** each additional key page (e.g. main category landings) with `formats: ["screenshot"]`, `fullPage: false` (above-the-fold is enough for secondary pages).
4. **Download every screenshot immediately** with `curl` to a scratch/temp directory. Firecrawl screenshot URLs are signed and expire — don't hold onto them or hotlink them.
5. **Compress screenshots** before embedding: `sips -Z 1400 -s format jpeg -s formatOptions 75 in.png --out out.jpg`. Full-size PNGs bloat the report; ~1400px JPEG at q75 is plenty for a design reference.
6. **Base64-encode** each compressed image (`base64 -i file.jpg | tr -d '\n' > file.b64`) and inline as `data:image/jpeg;base64,...` in the HTML. Do not link to external image hosts — if this report is ever published as a Claude Artifact, only a small CDN allowlist is permitted for `<img>` sources, and Firecrawl's storage host isn't on it.
7. **Build the HTML report** covering: screenshots, colour swatches (with hex values), typography sample + font stack + usage tally, and the site structure map (built from step 2's `links`, grouped by section). Always caveat the colour/font extraction as DOM-derived, not an official brand guideline — Firecrawl's own `branding.confidence` score is frequently `0`.
8. **Export to PDF** via headless Chrome (no extra install needed on macOS if Chrome is present):
   ```
   "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --disable-gpu \
     --no-pdf-header-footer --print-to-pdf="OUTPUT.pdf" "file://ABSOLUTE_PATH_TO.html"
   ```
   Check `which wkhtmltopdf` first in case that's installed instead — it's simpler when available.
9. **Save both files** to `outputs/{slug}/{slug}_brand_dossier.html` and `outputs/{slug}/{slug}_brand_dossier.pdf` in this project (create `outputs/` if it doesn't exist).
10. Optional: publish the HTML as a Claude Artifact if the user wants a shareable link rather than (or in addition to) local files.

## Expected output

- `outputs/{slug}/{slug}_brand_dossier.html` — self-contained (images inlined, no external dependencies except Google Fonts for the report's own chrome typefaces).
- `outputs/{slug}/{slug}_brand_dossier.pdf` — printable version of the same report.
- A brief summary in chat: what colours/fonts were found, and any caveats about extraction confidence.

## Edge cases / notes

- If the target site blocks scraping or renders mostly client-side with a slow hydration, add `waitFor` (milliseconds) to the `firecrawl_scrape` calls before capturing.
- If `branding.fonts` shows a licensed/non-Google font (e.g. Futura PT, Proxima Nova), don't try to load a lookalike — render the sample text in the site's own fallback stack (the next font in `fontStacks`) and note in the report that the primary face isn't web-embeddable.
- If a site has no clear top-level nav (e.g. a single-page site), skip the structure-map section rather than forcing one.
- If Chrome isn't installed and `wkhtmltopdf` isn't either, tell the user which one to install rather than guessing at a workaround.
- Keep the embedded image count/size in check — 3 compressed screenshots (~200-300KB each as base64) keeps the HTML under ~1MB and PDF export fast. For sites needing many more captures, consider linking full-size images as separate files instead of inlining all of them.
