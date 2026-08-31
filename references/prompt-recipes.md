# Prompt recipes

Use only the fields that help the request. Keep the final prompt compact but explicit.

## Photo transformation

```text
Use case: style-transfer
Asset type: finished 3:4 portrait paper-cut collage artwork
Primary request: Transform the supplied image into a sparse, tactile hand-cut paper collage.
Input images: Image 1 is the edit target. Preserve <subject, silhouette, pose, landmark, palette, or mood>. Any other image is style reference only; never copy its depicted subject.
Scene/backdrop: clean warm-white real journal paper, not beige or strongly yellow, with unmistakably visible fine diffuse fibers in varied directions, a few softer long fibers, localized short thread clusters, subtle mid- and fine-scale pulp relief, sparse neutral inclusions, and a few faint discontinuous scan traces <and optional faint dot grid>; the tactile texture must survive normal fitted-screen viewing and must not read as blank digital white; keep broad cloudy mottling extremely weak; gain visibility from local fibers rather than stains, overall darkening, or yellowing; keep the surface quiet, continuous, and free of dominant horizontal lines, repeated digital patterns, grime, cracks, burns, or theatrical aging
Subject: preserve one clear main subject and, when helpful, one or two quiet source-derived environmental echoes; keep the echoes behind or beneath the subject and use them only when they improve recognition or mood
Style/medium: a completely flat, front-facing journal clipping physically assembled from approximately 12–18+ coherent broad or medium hand-cut patterned paper pieces, adjusted to subject structure; each paper piece has one uniform solid color with no internal gradients, highlights, or painted shadows—depth comes only from stacking and color choice; solid colors and familiar regular patterns such as stripes, checks, grids, florals, botanical prints, and dots dominate; major pieces may carry a low-contrast embedded flat pattern (fine dots, narrow stripes, light grid, washi-paper grain) uniform across the piece; each piece shows visible paper fiber, soft edge fuzz, mild analog pigment variation, and overlap darkening; translate perspective or perceived volume into adjacent light, middle, and dark paper planes, overlap order, and negative-space cuts rather than drawn outlines; use only paper silhouettes, negative space, overlap seams, narrow paper strips, or paper patterns to define rims, handles, stems, grids, and structural edges; no graphite, pencil, pen, or ink outlines; no cast shadows or drop shadows; no inflated or sculptural paper volume; no coasters, bases, stands, or pedestals
Composition/framing: 3:4 portrait unless overridden; keep the motif structurally complete rather than reducing it to a tiny isolated icon; for the preferred structural default, let the complete motif group's bounding box occupy roughly 45–60% of the canvas width and 35–50% of its height while retaining approximately 60–75% visually quiet paper; for a deliberately minimal isolated object, use roughly 20–30% of the canvas; keep the complete motif-and-title group visibly inset from every paper edge by at least about 9% of the shorter page side
Lighting/mood: soft diffuse daylight, quiet editorial mood, flat even illumination
Color palette: derive 3–6 colors from the source, with at most one stronger accent
Materials/textures: patterned paper overlaps, gentle paper mottling, clear overlap darkening along stacked paper edges, imperfect joins, mostly clean geometric cuts with a few natural torn edges on outer contours, one or two subtly lifted corners, and restrained extra creases where needed to clarify the source form; all visibly wrinkled areas together stay below about 15% of the collage-motif region; any necessary fragmented color patches together stay below 70% of the collage-motif region; no cast shadows inside the motif
Text (verbatim): none by default; <exact user text> only when explicitly requested
Typography: clear faded 26–32 pt-equivalent typewriter lettering, 28 pt by default, with adequate tracking and moderately dark faded ink, near but never over the motif
Constraints: preserve <invariants>; facial identity is not required; simplify aggressively; make the subject itself from cut paper; match the bundled style references for abstraction level, matte fibrous paper, shallow overlap depth, restrained palette, source-derived supporting context, and spacious composition; never copy their depicted subjects
Avoid: dominant horizontal scan lines, coarse pulp texture, excessive wrinkles, cast shadows, drop shadows, graphite outline, pencil outline, pen line, ink contour, pasted vintage photos, tickets, receipts, labels, stamps, seals, desk props, paper rolls, scissors, tiny paper shards, confetti-like color chips, dense mosaic subdivision, dense scrapbook decoration, digital gradients, glossy vector perfection, photographic texture inside paper, ornate multicolor prints, notebook, book, card, frame, rounded corners, thickness, spine, hardcover, 3D volume, pseudo-text, logo, signature, watermark
```

## Preserved-photo paper composition

First resolve the layout, then generate only one isolated transparent paper-cut motif. Do not ask the image model to reproduce the photo, journal paper, caption, or full composite.

**CRITICAL — subject reference required:** When generating the transparent motif, you MUST pass the original source photo as an image reference (`image_reference_url_list`) so the model can match the subject's exact silhouette, proportions, and structural features. Text-only description will produce an unrecognizable generic shape. Upload the source photo first if only a local path is available, then use its public URL in the reference list. Label it clearly as "Image 1 is the subject reference only — recreate its exact subject silhouette and proportions."

```text
Use case: transparent motif generation for a preserved-photo paper composition
Asset type: one isolated text-free paper-cut collage motif with genuine transparent RGBA outside the motif
Primary request: Recreate the main subject from the reference image as a hand-cut paper collage motif on a pure solid white background. Keep the abstraction level, composition, and light editorial feel consistent with the bundled style references.
Input images: Image 1 is a subject reference only. Recreate its exact subject silhouette and proportions; do not reproduce a photo rectangle or pasted photo fragment.
Scene/backdrop: pure solid white background (will be made transparent post-generation); no paper, page texture, colored matte, checkerboard preview, scan marks, stains, border, frame, or background shadow
Subject: one compact paper-cut motif group representing <subject with specific structural details>. Preserve the exact geometric proportions and angular shapes from the reference.
Style/medium: built from approximately 12–18+ coherent hand-cut pieces of patterned paper; each paper piece has one uniform solid color with no internal gradients, highlights, or painted shadows; opaque layered paper pieces with visible fibrous edges, soft paper texture, and subtle overlap where papers stack. Each paper piece has soft torn fuzzy edges, mild color variation, clear overlap darkening. Paper patterns are subtle and refined like decorative patterned paper: warm white paper with fine stripe texture for main surfaces, pale gray paper for volumes, narrow paper strips for structural elements, soft muted blue paper with gentle wave texture for water, warm beige paper with fine dot pattern for ground. Patterns are understated, not loud. No cast shadows, no sculptural depth, no graphite outlines, no gradients, no photographic texture, no translucency, no coasters or bases.
Composition/framing: clean editorial composition, generous white negative space; keep the motif structurally complete and fully inside the asset; final scale, corner placement, safe inset, page contact shadow, and caption will be handled deterministically
Text (verbatim): none
Shadow: retain only subtle darkening where paper pieces overlap; do not add a broad page shadow or a rectangular panel shadow
Constraints: the alpha channel outside the complete motif must be truly transparent; match the reference for exact silhouette and proportions while applying paper-cut material abstraction
Avoid: paper background, opaque matte, checkerboard preview, colored halo, photo fragment, photo frame, graphite outline, pencil outline, pen line, ink contour, cast shadows, drop shadows, tiny paper shards, confetti-like color chips, dense mosaic subdivision, digital gradients, glossy vector fills, photographic texture inside paper, ornate multicolor prints, loud or bold patterns, translucency, adhesive shine, plastic feel, meaningless microtext, ticket, receipt, label, stamp, seal, sticker, scrapbook filler, stains, scan marks, logo, signature, watermark
```

Choose the caption before composition. Default to NO caption. If the user supplied text, reproduce it exactly. If the user explicitly requested a title but supplied no wording, derive one concise, factual, one-to-three-word English summary from the visible subject; do not invent a date, place, brand, or event.

Before generation, run `scripts/compose_direct_split.py --photo <PHOTO> --plan`. It defaults to a 3:4 portrait canvas and 50/50 split, resolves portrait sources to left-right and landscape or square sources to top-bottom, and may remove no more than 20% of source area without resampling. Use the reported `PAPER_PANEL_SIZE` only to judge motif complexity; do not generate that whole panel. If cropping would harm the subject, change `--crop-anchor-x` or `--crop-anchor-y`, or use `--crop-mode none`.

Generate the transparent motif once. Permit at most one targeted generative revision, and only when subject recognition or cut-paper material fails. A wrong size, corner, safe inset, caption position, paper texture, or photo treatment must be corrected deterministically and never triggers another image-generation call.

Then use the same resolved settings with `scripts/compose_direct_split.py --photo <PHOTO> --motif <TRANSPARENT_RGBA_MOTIF> --output <OUTPUT> --caption "<DECIDED TITLE>"`. The compositor trims the motif to its alpha bounds, defaults its bounding box to about 40% of the paper-zone area, places it in the lower-right balancing corner, keeps it at least 9% of the panel's short side from every edge, creates a narrow page-contact shadow from the alpha mask, and positions the caption close to but outside the motif. Default motif maximum width is 75% of paper zone and maximum height is 60%. It also preserves unchanged photo pixels and straight photo placement, builds exposed photo edges from a slowly varying torn contour with translucent fiber breakup, adds a broad pale ambient shadow plus a narrow fiber-following contact shadow, applies strict photo-zone clipping, and creates one continuous clean warm-white journal-paper sheet. That sheet must emphasize low-contrast fine fibers and sparse discontinuous scan traces while suppressing beige cast and broad cloudy mottling. Use `--motif-position`, `--motif-area-fraction`, the motif maximum-size flags, or explicit `--caption-x` and `--caption-y` to art-direct geometry locally. Replace other defaults when the user specifies another layout, ratio, panel share, text size, flat photo treatment, explicit photo rotation, straight edges, or no-text state.

Verify that `PHOTO_CROP_FRACTION` is at most `0.200000`, `PHOTO_TREATMENT=print`, `PHOTO_BORDER=0`, `PHOTO_EDGE_STYLE=torn-exposed`, `PHOTO_ROTATION=0`, `PHOTO_ZONE_CLIP=enabled`, `PHOTO_SOURCE_TRANSFORM=unchanged`, `PHOTO_PIXEL_VALUES=unchanged`, `PHOTO_MATTE_SOURCE=unified-procedural-journal`, `COLLAGE_ASSET_MODE=transparent-rgba-motif`, `PAPER_TEXTURE_MATCH=not-applicable-single-source`, `PAPER_BACKGROUND_MODE=unified-procedural`, and `PAPER_PANEL_CONTENT=transparent-rgba-motif-only`. Reject any motif with opaque background pixels, a baked checkerboard, a paper-colored rectangle, or a colored edge halo. The page background must be clean warm-white journal paper only—no colored or patterned backgrounds. No text by default; if the user requested text, it must be exact and legible. The legacy `--paper-panel` path exists only for old assets and must not be used for new generations.

The finished composition must retain separate photo and paper zones. The retained photograph appears as a straight borderless physical print with unchanged pixels, a naturally paced hand-torn contour, fine translucent fibers, and a restrained two-stage ambient/contact shadow. Reject regular sawteeth, hard cutout edges, thick white borders, one uniform dark halo, or floating-card depth. Rotate only when the user explicitly asks. Clip the photo and both shadows to the photo zone so they never overlap the paper-cut motif. If the unchanged crop does not fill its photo band, the same single procedural paper sheet already behind the paper zone must show through; do not sample or copy a generated panel. Require clean warm-white paper whose irregular fibers, small thread clusters, fine pulp relief, and faint discontinuous scan residue remain visible at normal fitted-screen size. Reject both a nearly textureless digital-white field and beige/yellow cast, broad blotches, stretching, mirroring, tiling, visible repetition, color shift, texture discontinuity, imported background stains, or copied collage content. Never use a generative full-composite edit for this assembly.

## Description generation

```text
Use case: stylized-concept
Asset type: 3:4 portrait paper-cut collage poster
Primary request: Create a sparse hand-cut paper collage about <theme or subject>.
Scene/backdrop: clean warm-white journal paper, not beige or yellow, with unmistakably visible fine diffuse fibers in varied directions, a few softer long fibers, localized short thread clusters, fine pulp relief, sparse neutral inclusions, and faint discontinuous scan traces; the tactile paper grain remains visible when the full page is fitted on screen; suppress broad mottling, stains, dominant horizontal lines, and repeated texture
Subject: one compact paper-cut motif expressing <idea or emotion>
Style/medium: physically assembled from approximately 12–18+ coherent broad or medium pieces of hand-cut patterned paper, adjusted to subject structure and dominated by solid colors and familiar regular patterns such as stripes, checks, grids, florals, or dots; each piece has one uniform solid color with no internal gradients or shading; show paper fiber, soft edge fuzz, and mild analog pigment variation; use paper edges, overlaps, negative space, and narrow paper strips instead of graphite or ink outlines; no digital gradients, photographic texture, glossy vector fills, or ornate multicolor prints; no cast shadows or drop shadows; no coasters, bases, or stands
Composition/framing: central or lower-middle motif with restrained asymmetry; use roughly 20–30% of the canvas for a deliberately minimal isolated object and preserve generous quiet paper; keep the complete motif safely inset from every page edge by at least about 9% of the shorter page side
Lighting/mood: soft diffuse daylight, quiet editorial clarity with slight handmade roughness
Color palette: <3–6 colors and one optional accent>
Materials/textures: patterned paper overlaps, imperfect seams, mostly clean cuts with a few natural torn edges, slight lifted corners, and delicate overlap darkening; use wrinkles only where expressive and keep them below about 15% of the motif region; keep any necessary fragmented color patches below 70% of the motif region; no cast shadows
Text (verbatim): none
Constraints: keep the metaphor legible and the page spacious
Avoid: vintage ephemera, visible tools, craft-table clutter, dense collage, pseudo-writing, logo, signature, watermark
```

## Exact text add-on

Append this only when the user requests text:

```text
Text (verbatim): "<EXACT TEXT>"
Typography: one clear faded 26–32 pt-equivalent <vintage typewriter or user-requested journal-handwritten> title, 28 pt by default, accurately spelled and immediately legible with adequate tracking and moderately dark faded ink, with an optional compact metadata line "<EXACT METADATA>"
Placement: quiet and secondary, separated from the main motif by negative space
Constraints: reproduce only the quoted characters; do not add filler text or decorative pseudo-writing
```

## Automatic image-summary title

Use this only when the user explicitly requests a title but supplied no wording for image-based work:

```text
Derive one factual English title of one to three words from the visible main subject. Prefer concrete wording such as "BLUE HOUR", "CAT NAP", or "SWEET PAIR". Do not infer an unknown place, date, brand, identity, or event. Render it in clear faded 26–32 pt-equivalent typewriter lettering, 28 pt by default, with adequate tracking and moderately dark faded ink, near but never over the paper-cut motif and safely inset from the paper edge. For preserved-photo paper composition, add it deterministically after motif generation.
```

## Targeted revision

```text
Edit only <failed property>.
Keep unchanged: <approved subject>, <approved paper-cut construction>, <approved palette>, <approved composition>, <approved background>, and <exact text or no-text state>.
Correct <specific defect> by <single concrete change>.
Do not add any new object, decoration, wording, logo, signature, or watermark.
```

For a preserved-photo composite, add these invariants:

```text
Keep unchanged: every retained pixel of the source-photo panel, its approved crop box, the straight main seam, panel order and ratio, paper texture, negative-space ratio, motif corner, and exact text unless the user explicitly requests one of those properties to change.
Do not regenerate, recrop, resample, recolor, retouch, extend, or otherwise alter the photo panel.
```

## Example: cat photo

```text
Transform the supplied cat photo into a structurally complete hand-cut paper cat on a warm-white 3:4 journal page. Preserve the cat's pose, ear shape, tail gesture, and source-derived fur palette; facial identity is not required. Construct the body from approximately 8–12 coherent overlapping pieces of mostly solid patterned paper, with one restrained dotted or striped piece; use narrow paper strips and negative-space cuts for whiskers and paws, with no graphite or ink outlines and no cast shadows. Keep generous negative space and a safe inset from every page edge. Use a flat journal-clipping treatment, subtle translucency, narrow overlap darkening, imperfect joins, one lifted paper edge, and restrained wrinkles that clarify the pose. When text is requested, add the faded typewriter title "CAT NAP" near but not over the motif. No photo rectangles, tickets, labels, stamps, tools, paper rolls, pseudo-writing, logo, signature, or watermark.
```

## Example: cake photo

```text
Rebuild the cake in the supplied photo as a compact, flat hand-cut paper journal clipping on a clean white 3:4 page. Preserve the cake's layered profile and its two strongest colors. Define each cake layer with one horizontal paper band, mix solid color with one simple stripe or floral pattern, and use a tiny narrow paper strip only where needed. Keep abundant blank paper. Add subtle overlap darkening, clear stacked-edge definition, mixed cut and torn edges, and restrained creases that clarify the layers. When text is requested, add the faded typewriter title "SWEET LAYERS" near but not over the motif. No scrapbook ephemera.
```

## Example: rainy solitude

```text
Create a 3:4 portrait hand-cut paper collage poster about solitude on a rainy day. On a warm-white journal page, build one small abstract window-and-raindrop motif from blue-gray, softened violet, translucent slate, and one tiny muted silver accent. Use mostly solid paper with one fine grid pattern, narrow paper strips instead of drawn lines, uneven overlaps, slight wrinkles, and delicate lifted edges. Place the motif in the lower-middle, safely inset from every edge, and leave roughly 85% quiet negative space. Soft diffuse light, contemplative editorial mood, no text, no person, no tickets, labels, stamps, visible tools, pseudo-writing, logo, signature, or watermark.
```

## Example: travel photo

```text
Distill the supplied travel photo into one recognizable landmark silhouette built from hand-cut patterned paper. Preserve the landmark's defining outline, the source's dominant sky color, and one environmental accent; omit minor architecture, crowds, vehicles, and signage. Use a clean 3:4 portrait warm-white journal page unless the user requests another ratio. Keep the landmark compact, safely inset from all page edges, with large negative space. Use a flat front-facing journal-clipping treatment, solid paper with at most two restrained basic patterns, subtle overlaps, clear stacked-edge darkening, narrow paper details instead of drawn contours, and restrained folds that clarify the landmark. When text is requested, add one faded typewriter title that factually summarizes the visible subject without inventing a place name; exact user wording overrides it. No vintage photo treatment, tickets, maps, labels, stamps, desk props, logo, signature, or watermark.
```
