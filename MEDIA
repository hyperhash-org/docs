HYPER HASH — MEDIA SPEC (VERSION 0)

PURPOSE
This file defines the canonical rules for creating, storing, optimizing, and publishing all Hyper Hash media assets: logos, icons, diagrams, screenshots, videos, and social cards. It ensures brand consistency, small file sizes, and clear licensing.

SCOPE
• Applies to all repos in hyperhash-org, with the docs repo as the single source of truth for brand assets.
• Covers: directory layout, naming, export settings, optimization, licensing, and contribution workflow.

DIRECTORY LAYOUT (IN THIS REPO)

/docs/media/
  /brand/                 # Logos, wordmarks, lockups, color swatches, typography references
    /logo/                # Primary logo exports
      svg/                # Vector masters (preferred in docs)
      png/                # Raster fallbacks (@1x, @2x, @3x)
    /wordmark/
    /palette/             # Color chips (SVG + PNG), palette JSON
    /typography/          # Font licenses and usage PNGs
    /favicon/             # Favicon + app icon set
    /social/              # Open Graph/Twitter cards
  /diagrams/              # Architecture, flows, data models
    /source/              # Editable sources (Mermaid .mmd, Excalidraw .excalidraw, Figma exports .fig)
    /export/              # Committed SVG + PNG exports used in docs
  /screenshots/           # UI screenshots used in READMEs or website
    /web/                 # Site pages
    /mmi/                 # Node MMI
  /video/                 # Short explainer loops, product teasers
    /source/              # Editable project files
    /export/              # .mp4 (H.264) + .webm (VP9) + poster.png
  /third_party/           # Any third-party graphics with attribution files
  /LICENSES/              # Licenses for fonts, third-party icons, and borrowed assets


NAMING CONVENTIONS
• Lowercase, dash-separated, no spaces: hyperhash-logo-dark.svg, node-flow-v0.svg, ui-dashboard-2025-10.png.
• Include a purpose or surface: og-default-1200x630.png, twitter-card-1600x900.png.
• For raster densities: @1x, @2x, @3x suffixes (e.g., logo@2x.png).
• Version suffix for diagrams when content changes materially: -v0, -v1, etc.
• Never overwrite vector masters; add a new version.

SOURCE VS EXPORT
• Source files live under /media/*/source/ (Mermaid .mmd, Excalidraw .excalidraw, Figma .fig, Illustrator .ai, etc.).
• Never embed binary sources directly in Markdown; always export to SVG (preferred) or PNG.
• Keep exports in /export/ and reference those in docs.

PREFERRED FORMATS
• Diagrams: SVG (text preserved), PNG fallback for GitHub README rendering if needed.
• Logos/Icons: SVG (single-color variants); raster PNG for favicons and social cards.
• Screenshots: PNG or JPEG (quality 85+) depending on content; avoid huge PNGs for photos.
• Video: Provide MP4 (H.264) and WEBM (VP9) plus a poster.png still.

OPTIMIZATION PIPELINE
Use the following before committing exports:

SVG (lossless minify):

svgo --multipass -f docs/media -r


PNG (lossless):

oxipng -o 4 -i 0 --strip all docs/media -r


JPEG (visually lossless target ~85%):

jpegoptim --max=85 --strip-all --all-progressive docs/media -r


Video:

MP4 (H.264): constant rate factor target CRF 20–23.

WEBM (VP9): -b:v 0 -crf 32 for small docs embeds.

Keep < 15 MB per asset in the docs repo (use external hosting if larger).

COLOR & TYPOGRAPHY (INITIAL BASELINE)
These values are a starting point; update if brand decides otherwise.

Primary neutrals
• HH Black: #0B0C10
• HH Dark Gray: #1F2227
• HH Mid Gray: #2B2F36
• HH Light Gray: #A7AFBC
• HH White: #FFFFFF

Accent (Bifrost spectrum)
• Bifrost-Blue: #3BA2FF
• Bifrost-Cyan: #47E9E1
• Bifrost-Green: #63E06E
• Bifrost-Yellow: #FFD166
• Bifrost-Orange: #FF944D
• Bifrost-Red: #FF5C70
• Bifrost-Purple: #A784FF

Typography (as used in UI)
• Inter (SIL OFL) for UI and docs body.
• Space Mono (SIL OFL) for numbers, code accents, and telemetry labels.
Store font license files in /media/brand/typography/ and link to OFL.

LOGO & WORDMARK USAGE
Do
• Use SVG versions in docs and web where possible.
• Maintain clear space equal to the height of the “H” in “Hyper.”
• Use white or black wordmark on solid backgrounds.

Don’t
• Stretch, skew, or alter proportions.
• Recolor the primary mark except to white/black or approved accent.
• Place on visually noisy backgrounds without a solid plate behind.

FAVICON & APP ICON SET
Generate and store in /media/brand/favicon/:
• favicon.ico (16/32/48)
• favicon-32x32.png
• favicon-16x16.png
• apple-touch-icon.png (180×180)
• android-chrome-192x192.png
• android-chrome-512x512.png
• site.webmanifest (name, short_name, icons, theme_color, background_color)

SOCIAL CARDS (OG/TWITTER)
Defaults stored in /media/brand/social/:
• Open Graph default: og-default-1200x630.png
• Twitter card default: twitter-card-1600x900.png
• Include title block, brief subtitle, and logo plate.
• Keep file sizes ≤ 400 KB where possible; prefer JPEG @ 85%.

DIAGRAM WORKFLOW
• Prefer Mermaid (.mmd) and Excalidraw (.excalidraw) for source; export to SVG.
• When diagramming complex flows (e.g., Core ↔ Treasury payout), include a tiny “Diagram v0 • YYYY-MM-DD • commit <shortsha>” footer.
• Keep a small text file beside each diagram describing its purpose and last updated commit.

SCREENSHOTS
• Use consistent window size and UI theme (dark).
• Filenames must include context and date: mmi-lightning-channels-2025-10.png.
• Crop to the content; avoid huge margins; keep ≤ 2000 px width for docs.

ACCESSIBILITY
• Provide concise alt text for every image in docs.
• Ensure text remains selectable in SVG exports.
• Avoid text baked into raster images where possible.

LICENSING
• All first-party media in this repo: CC BY 4.0 (same as docs).
Attribution: “© Hyper Hash Network (CC BY 4.0)”
• Fonts: Inter and Space Mono under SIL Open Font License (include copies under /LICENSES/).
• Third-party icon pack: Lucide (ISC License). Include license in /LICENSES/.
• Any third-party asset added to /third_party/ must include its original license and attribution file.
• If an asset cannot be shared under CC BY 4.0, do not commit it here; store references only.

CONTRIBUTION RULES
• Never commit unoptimized exports; run the optimization commands above.
• Do not commit design binaries larger than 25 MB (use a link to cloud storage or design tool).
• Include a one-line purpose in the PR description for every new asset.
• For logo changes or brand color updates: open a proposal issue first; do not merge directly.

VERSIONING & CHANGE CONTROL
• Each media change must reference a Git commit or PR.
• Major visual changes (logo, color, typography) require a GitHub Discussion and two maintainer approvals.
• Keep a short CHANGES.txt inside /docs/media/ with date, author, and summary.

QUICK START CHECKLIST (FOR NEW ASSETS)
[ ] Place source in /media/.../source/
[ ] Export to SVG (and PNG as needed) into /export/
[ ] Optimize: svgo, oxipng/jpegoptim
[ ] Add alt text in associated Markdown pages
[ ] Update /media/CHANGES.txt with a one-liner
[ ] Verify license (first-party CC BY 4.0 or include third-party license)

TASK REGISTER (MEDIA-SPECIFIC)
All tasks start as PLANNED; move to IN PROGRESS/COMPLETE in the central CHANGELOG when done.
• Create base logo & wordmark exports (SVG, PNG @1x/@2x/@3x) — PLANNED
• Build favicon/app icon set + manifest — PLANNED
• Produce default OG/Twitter cards — PLANNED
• Export initial architecture & payout flow diagrams — PLANNED
• Capture first-pass UI screenshots (UI, MMI) — PLANNED
• Add optimization scripts to docs CI (svgo/oxipng/jpegoptim) — PLANNED
• Add media spell-check and link-check to docs CI — PLANNED
• Populate /LICENSES/ with OFL, ISC, and CC BY 4.0 texts — PLANNED
• Create CHANGES.txt and seed with v0 entries — PLANNED

CONTACT & OWNERSHIP
Media/Brand Owners (CODEOWNERS entries apply on this path):
/docs/media/ @brand @docs-maintainers
Questions or requests: open a GitHub Issue in hyperhash-docs or post in Discord #dev or #operations.

END OF FILE — MEDIA SPEC (VERSION 0)
