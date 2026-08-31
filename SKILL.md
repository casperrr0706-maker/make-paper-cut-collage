---
name: make-paper-cut-collage
description: Transform a supplied photo or text description into a clean, tactile paper-cut collage raster artwork, or pair a faithfully preserved borderless photo print with a spacious warm-white paper panel containing a compact paper-cut interpretation. Use for requests mentioning paper cut, paper-cut art, cut paper, 剪纸, 剪纸拼贴, 手撕纸, 纹样纸张剪纸, 保留原图, or photo-and-collage paper layouts.
---

# Make Paper Cut Collage

Create one restrained bitmap artwork that feels physically assembled from hand-cut patterned paper on a journal page. Favor a structurally readable motif, generous negative space, simple muted paper colors or familiar patterns, and believable torn edges over dense scrapbook decoration. The collage is flat and front-facing—no sculptural depth, no cast shadows.

## Load the style rules

Read [references/style-system.md](references/style-system.md) before generating. Read [references/prompt-recipes.md](references/prompt-recipes.md) when shaping or revising the image prompt.

Use the bundled images under `assets/style-references/` only when visual inspection would resolve ambiguity or help diagnose a weak result. Treat them as style references, never as subject matter to copy.

## Interpret the request

Classify the request as one of these paths:

- **Preserved-photo paper composition (DEFAULT for photo inputs):** Keep a faithful, unredrawn source-photo print and pair it with a separate paper-cut collage paper panel on a continuous warm-white journal sheet. This is the default mode for any photo input unless the user explicitly asks for paper-only.
- **Paper-only transformation:** When the user explicitly says "only paper-cut", "no original photo", "just the collage", or similar, extract the main subject from the supplied image and rebuild it as a flat paper-cut collage on a full journal-paper canvas.
- **Description generation:** Invent a single clear paper-cut motif from the brief.
- **Targeted revision:** Change only the requested property of a previously generated collage and preserve all other approved properties.

**Default mode rule:** For any request that includes a photo and does not explicitly say "only paper-cut" / "no original photo" / "just the collage", use the preserved-photo paper composition mode. Do not default to paper-only transformation.

## Preserve the original photo (default mode)

This is the **default mode for all photo inputs**. Use it unless the user explicitly asks for paper-only ("only paper-cut", "no original photo", "just the collage"). User instructions always override these defaults.

- Choose the layout orientation from the source by default: for a portrait photo (`height > width`), place the faithful photo at left and the paper-cut collage paper panel at right; for a landscape or square photo (`width >= height`), place the faithful photo above and the paper panel below.
- Let the photograph occupy approximately 50% of the finished canvas and the paper panel approximately 50%.
- Default the complete photo-and-collage composite to `3:4` width-to-height portrait (also described as vertical 4:3). Follow an explicit alternate final ratio.
- Preserve the photo pixels faithfully and realistically. Never redraw, stylize, recolor, retouch, extend, erase, add, move, regenerate, rescale, or non-uniformly stretch anything inside the retained photo. Do not apply grain, tint, contrast, print-noise, or other overlays to the photo pixels by default. Express a convincing physical photo-print quality through the unaltered photographic surface, visibly fibrous torn edges, and a natural contact shadow outside the retained image content.
- When necessary to improve the fit, crop the source without resampling while retaining at least 80% of its original pixel area. Default to a centered crop; adjust the crop anchor only to protect the subject. Do not exceed 20% removed area. If that crop limit still cannot fill the photo panel, keep the retained pixels at original size and let the same continuous warm-white journal-paper sheet used by the rest of the canvas show through. Do not synthesize or sample a second filler texture; do not stretch, mirror, tile, or copy panel pixels or collage content. The paper surrounding the photograph and the paper panel must therefore join with identical color and texture continuity.
- Obey any explicit alternate orientation, panel order, or ratio. Keep the two layout zones exactly aligned at one perfectly straight horizontal or vertical division.
- Generate one isolated, text-free paper-cut motif on a genuinely transparent RGBA background. **CRITICAL:** You MUST pass the original source photo as an image reference when generating this motif, so the model matches the subject's exact silhouette, proportions, and structural features. Text-only description will produce an unrecognizable generic shape. Upload the source photo first if only a local path exists, then use its public URL in `image_reference_url_list`. Label it clearly as "Image 1 is the subject reference only — recreate its exact subject silhouette and proportions." Do not ask the image model to generate journal paper, page texture, captions, borders, broad page shadows, or the complete two-panel composition. Then use a deterministic raster compositor—not a generative edit—to scale and place the motif, synthesize the only paper surface used by the final output, mount the retained source pixels, and add typography. Prefer `scripts/compose_direct_split.py --motif`; use an equivalent local compositor only when necessary. This prevents generated paper pixels, stains, scan marks, and color casts from leaking around the motif.
- Present the faithful crop as a borderless physical photo print by default: no white frame or paper-stock border and no pixel-level print filter. Keep it straight and aligned with its photo zone by default; rotate it only when the user explicitly requests rotation. Shape every exposed photo edge with a slowly varying hand-torn contour, fine translucent fiber breakup, and softly antialiased irregularities; avoid uniformly jagged sawtooth noise or an obvious digital cutout. Use a restrained two-stage shadow—a broad pale ambient lift plus a narrow contact shadow that follows the torn fibers—so the print settles naturally onto the same paper rather than floating above it. Clip the photo and both shadow layers to the photo zone so neither can overlap the paper-cut motif or paper-panel content. Do not add a Polaroid frame, curled photo, or dramatic floating depth.
- Run `scripts/compose_direct_split.py --photo <PHOTO> --plan` before motif generation. Use its resolved orientation, crop, final size, `PHOTO_CONTENT_BOX`, and `PAPER_PANEL_SIZE` to choose the motif's structural complexity, but do not generate a full paper panel. The script defaults to the orientation rules above, a 3:4 final canvas, a 50/50 split, a maximum 20% source-area crop, unchanged source pixels, straight photo placement, pronounced natural torn exposed edges, zone clipping, and one continuous independently synthesized paper sheet.
- Let the compositor place one compact paper-cut-and-caption group in a balancing corner. Never center it unless explicitly requested. Keep the whole group at about 40% of the paper panel by default and preserve about 60% as quiet negative space. Keep every part of the group safely inset from the paper-panel edges by at least about 9% of the panel's shorter side; do not let paper, wrinkles, shadows, or text touch or clip against an edge. It may grow when the source needs stronger expression, but must never exceed 60% of the paper panel. Correct scale, corner, inset, and caption placement deterministically; they never justify another image-generation call.
- For image-based work, add one concise factual English title near the paper-cut motif by default. Derive one to three words from the visible image content, use clear faded typewriter lettering at a 26–32 pt equivalent with 28 pt as the default, and never overlap the motif. Use moderately dark faded ink, adequate tracking, and immediate legibility while keeping the title harmonious with the composition. Exact user-supplied text overrides the summary; omit text only when the user explicitly asks for no text.
- Generate the transparent motif without text, then add the decided title deterministically with `scripts/compose_direct_split.py --motif <MOTIF> --caption "<TITLE>"`. The compositor places the title close to, but outside, the motif by default; use explicit caption coordinates only when art direction requires them.
- Never add meaningless microtext, tickets, receipts, labels, stamps, seals, stickers, or archival filler.

Read the preserved-photo section in [references/style-system.md](references/style-system.md) and use the paper-composition recipe in [references/prompt-recipes.md](references/prompt-recipes.md) for this mode.

## Paper-only transformation (explicit request only)

Use this mode **only** when the user explicitly says "only paper-cut", "no original photo", "just the collage", "只要剪纸", "不要原图", or similar. Never use it by default for photo inputs.

- Generate one complete flat paper-cut collage on a full continuous warm-white journal-paper canvas.
- The background must be a flat continuous paper surface filling the entire canvas — **never** a notebook, book, card, framed object, or anything with rounded corners, thickness, spine, hardcover, or 3D volume.
- Pass the source photo as an image reference so the model matches the subject's exact silhouette and proportions.
- Use the photo-transformation recipe in [references/prompt-recipes.md](references/prompt-recipes.md).
- Add "no notebook, no book, no card, no frame, no rounded corners, no thickness, no hardcover, flat paper surface filling entire canvas" to the avoid list.

## Set the format

- Use `3:4` portrait when there is no input image and the user gives no ratio.
- For any image-based collage, default the finished asset to `3:4` width-to-height portrait unless the user overrides it. In preserved-photo mode this ratio applies to the complete photo-and-paper composite.
- For a minimal isolated object, let the motif occupy roughly 20–30% of the paper canvas. For the preferred structural photo transformation, let the complete motif group's bounding box occupy roughly 45–60% of the canvas width and 35–50% of its height, usually near the center or lower-middle, while retaining approximately 60–75% visually quiet paper. In preserved-photo mode, keep the motif-and-caption group compact at about 40% of the paper panel unless the user requests otherwise.
- On every paper-only canvas or paper panel, keep the complete paper-cut motif and any text visibly inset from all page edges by at least about 9% of the shorter paper dimension unless the user explicitly requests edge contact or cropping.
- Present clean warm-white journal paper, never beige or strongly yellow. Its photographically believable uncoated paper surface must combine unmistakably visible fine diffuse fibers in varied directions, a smaller number of softer long fibers, subtle mid- and fine-scale pulp-density variation, sparse neutral inclusions, and a few faint discontinuous scan traces. The texture must still read when the complete image is fitted to an ordinary screen: a nearly blank digital-white field is a failure. Build that visibility from localized fibers, short thread clusters, and fine pulp relief—not from stains or broad tonal clouds. Keep broad low-frequency mottling extremely weak so it cannot resemble grime, water damage, or uneven aging. The paper must remain clean, low contrast, non-repeating, and continuous across every exposed canvas area. Avoid dominant horizontal lines, stretched or mirrored texture, mechanical tiling, excessive yellowing, dirt, cracks, burns, water marks, grunge, and theatrical archival aging. A faint dot grid remains optional only when it supports the composition.

## Handle text

- For image-based work, default to one factual one-to-three-word English summary title. For description-only generation, default to no text.
- If the user supplies exact wording, use it instead. If the user explicitly requests no text, omit it.
- When requested, allow one short title and optionally one compact metadata line containing a date, location, or number.
- Default to English; use another language only when requested or supplied.
- Treat every requested character as exact. Quote the text verbatim in the prompt, spell tricky words letter by letter, and specify a restrained vintage typewriter face or journal-style handwriting.
- Keep typography secondary to the collage but distinctly legible, using 28 pt equivalent by default and normally staying within 26–32 pt. Use a clear typewriter face, adequate tracking, and a moderately dark faded-ink tone. Do not add slogans, paragraphs, fabricated dates, or decorative pseudo-writing.
- In preserved-photo mode, render the title deterministically after generating the text-free transparent motif rather than asking the image model to draw it.

## Generate or edit

Use the built-in image generation tool by default.

For a new image from text, omit image-reference parameters and express the complete visual system in the prompt.

For a photo transformation:

1. Inspect every local input image before generation.
2. Label each image role explicitly as `edit target`, `style reference`, or `supporting input`.
3. Use local referenced-image paths when all target images are local. If any target exists only in conversation context, include the smallest number of recent images that covers every target instead.
4. Never provide both local referenced-image paths and recent-conversation image inclusion in the same call.
5. If useful and compatible with the chosen input mechanism, include no more than two bundled style references. State that their subjects must not appear in the result.
6. Preserve only the source properties the user values: subject category, pose or silhouette, defining landmark, palette, or mood. Permit abstraction, cropping, illustration, photocopy, and halftone treatment as specified.

For a preserved-photo direct splice, do not pass the source photo into a generative full-composite edit. Generate only one isolated transparent RGBA paper-cut motif, then assemble and verify the final flat image deterministically as described above. Use one ImageGen call by default. Allow at most one targeted generative revision, and only when subject recognition or the cut-paper material itself fails. Layout, motif scale, position, safe inset, paper texture, photo treatment, and typography must be corrected locally and never justify a generative retry.

Shape the prompt using the recipe in [references/prompt-recipes.md](references/prompt-recipes.md). Keep it production-oriented and explicitly list constraints and avoid items.

## Validate and refine

Inspect the result before delivery. Confirm all of the following:

- The subject or emotional proposition remains understandable.
- The artwork reads as physical hand-cut patterned paper rather than flat vector shapes or a digital scrapbook.
- Pure-color paper and basic patterns dominate; torn edges, slight overlaps, and paper fiber remain believable and restrained.
- The collage reads as a flat journal clipping with shallow layers, not a volumetric paper sculpture. Match the preferred reference quality through approximately 8–15 coherent broad or medium hand-cut paper pieces, adjusted to subject structure, with stacked planes, soft fibrous torn edges, and mild analog pigment variation. Preserve perceived volume when useful through adjacent light, middle, and dark paper faces, overlap order, and negative-space cuts without creating inflated or sculptural paper depth. Do not add gray graphite, pencil, pen, or ink outlines; communicate rims, handles, stems, grids, and structural edges through paper silhouettes, overlap seams, negative space, narrow paper strips, or simple paper patterns. Solid paper and familiar patterns such as stripes, checks, grids, florals, and dots still dominate. Natural paper fiber, slight mottling, and overlap darkening are desirable; digital gradients, glossy vector fills, photographic texture, and ornate multicolor prints are not. Keep the combined area of any necessary fragmented color patches below 70% of the collage-motif region. No cast shadows or drop shadows inside the motif.
- Negative space is generous, the page is not crowded, and the complete motif-and-caption group maintains a visible safe inset from every paper-panel edge.
- In preserved-photo mode, the complete canvas defaults to 3:4 portrait; portrait inputs resolve to left-right and landscape or square inputs to top-bottom unless overridden. Any source crop removes no more than 20% of source area. The retained photo pixels remain unchanged, without redraw, stretch, recoloring, or texture overlay. Physical-print character comes from a slowly varying torn contour, fine translucent edge fibers, and a restrained two-stage ambient/contact shadow—never from harsh sawtooth edges or a uniform dark halo. The photo and both shadow layers remain clipped to the photo zone and never overlap the paper-cut motif. The final image uses one continuous clean warm-white procedurally synthesized journal-paper sheet behind both zones; its irregular fine fibers, a few longer soft threads, fine pulp relief, and faint discontinuous scan traces must remain clearly perceptible at fitted viewing size while broad mottling stays extremely weak. Reject a textureless digital-white page just as firmly as beige paper or dirty blotches. Uncovered photo-band space and the paper panel therefore share identical color and texture continuity without sampling or copying generated pixels. Reject stretching, mirroring, tiling, visible repetition, color shift, texture seams, or copied collage content. Require a genuine alpha channel around the motif; reject opaque paper mattes, checkerboard previews, colored halos, and full-panel assets. The motif-and-caption group sits in a balancing corner at about 40% by default, remains at least about 9% of the panel's short side from every paper edge, and never exceeds 60% unless the user explicitly requests another scale.
- No new tickets, labels, stamps, vintage-photo fragments, paper rolls, pens, or unrelated desk props appear in the generated collage or paper panel unless explicitly requested. Never alter a preserved source photo merely because it already contains one of these elements.
- For image-based work, one clear factual English summary is present by default unless the user opted out; any user-supplied text is exact and immediately legible. Description-only work remains text-free by default.
- No watermark, signature, logo, or accidental pseudo-text appears.

If a semantic or paper-material check fails, make at most one targeted generative revision and re-check. Repeat invariants in the revision prompt. Correct geometry, paper, photo mounting, or typography deterministically instead of regenerating. Do not casually alter an approved subject, palette, layout, or exact text while correcting another issue.

Return the final image inline. For workspace-bound work, copy the selected file into the requested project location and report its path, the final prompt, and that the built-in generation path was used.
