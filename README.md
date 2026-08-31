# Make Paper Cut Collage

[简体中文](README.zh-CN.md) | **English**

Turn a photograph or short description into a clean, tactile hand-cut paper collage. The skill can also preserve an original photograph unchanged and pair it with a compact paper-cut interpretation on one continuous warm-white journal-paper sheet.

## Highlights

- Rebuilds food, pets, objects, plants, people, travel scenes, architecture, and moods from coherent hand-cut paper planes.
- Uses broad and medium pieces, simple colors or familiar paper patterns (stripes, dots, grids, florals), soft torn edges, and subtle overlap darkening.
- Keeps compositions spacious and editorial instead of filling the page with scrapbook decoration.
- Supports faithful photo preservation: no redraw, recoloring, retouching, stretching, or generative replacement of retained photo pixels.
- Produces one continuous clean warm-white paper surface with visible irregular fibers, subtle pulp relief, and faint discontinuous scan traces.
- Uses deterministic local composition for layout, paper, photo mounting, shadows, and typography, avoiding generated paper seams or mismatched backgrounds.
- Excludes tickets, receipts, labels, stamps, seals, pseudo-writing, visible tools, and unrelated vintage ephemera unless explicitly requested.

## Usage

Invoke the skill explicitly with `$make-paper-cut-collage`, or describe a request that clearly asks for a paper-cut, cut-paper, or 剪纸 collage.

### Transform a photo

```text
Use $make-paper-cut-collage to turn this cat photo into a hand-cut paper journal collage.
```

```text
Use $make-paper-cut-collage to transform this building photo into a quiet paper-cut poster. No text.
```

The subject is reconstructed from patterned paper; the source photograph does not appear in the final artwork unless you request that it be preserved.

### Preserve the original photo

```text
Use $make-paper-cut-collage to turn this photo into a paper-collage journal page and preserve the original photo.
```

```text
Use $make-paper-cut-collage to preserve this photo and place the title "TOKYO" beside the paper-cut motif.
```

This mode keeps the photo and the paper interpretation in separate layout zones. It generates only a transparent paper-cut motif, then assembles the final image locally so the source pixels and paper texture remain controlled.

### Generate from a description

```text
Use $make-paper-cut-collage to create a blue-gray paper-cut poster about solitude on a rainy day.
```

```text
Use $make-paper-cut-collage to create a warm paper-cut poster about being alone in a café.
```

Description-only work is text-free by default.

## Default behavior

| Property | Default |
| --- | --- |
| Final format | `3:4` portrait (`width:height`) for image-based work |
| Photo transformation | One recognizable subject built from about 8–15 coherent paper pieces, with up to two quiet source-derived environmental echoes when useful |
| Preserved portrait photo | Photo at left, paper panel at right |
| Preserved landscape or square photo | Photo above, paper panel below |
| Preserved-photo split | Approximately 50% photograph and 50% paper |
| Photo integrity | Unchanged retained pixels; no redraw, recoloring, retouching, stretching, or texture overlay |
| Photo cropping | At most 20% of source area, only when necessary; use no crop when subject integrity benefits |
| Photo mounting | Borderless print, natural torn exposed edges, restrained ambient and contact shadows |
| Preserved-mode motif | Compact motif-and-caption group in a balancing corner, about 40% of the paper panel |
| Motif shadow | Subtle narrow contact shadow (0.003) for paper-collage depth without looking 3D |
| Paper | Clean warm white, visibly fibrous, low contrast, non-repeating, and free of stains or heavy aging |
| Image-based text | One factual English title of one to three words unless exact text or no text is requested |
| Description-only text | None |
| Generation passes | One motif-generation call by default; at most one targeted revision for failed recognition or paper material |

Explicit user instructions override these defaults.

## Visual system

The preferred style is a **soft structural scene distillation**:

- Preserve one to three identification anchors.
- Translate perspective or volume into adjacent light, middle, and dark paper planes rather than drawn outlines.
- Keep the collage flat and front-facing, with shallow overlap depth rather than sculptural paper volume.
- Use a source-derived low-saturation palette of roughly four to six colors, with at most one restrained warm accent.
- Favor solid paper and subtle patterns such as stripes, checks, grids, florals, or dots — patterns harmonize with the source image's style and elements.
- Keep obvious wrinkles below about 15% of the motif region.
- Preserve generous negative space and a clear inset from every paper edge.

The bundled reference images guide material, abstraction, and composition only. Their depicted subjects must never be copied.

## How preserved-photo composition works

1. `compose_direct_split.py --plan` resolves the final ratio, panel orientation, crop limits, and pixel geometry (with automatic EXIF orientation correction).
2. Image generation creates one isolated, text-free paper-cut motif on transparent RGBA, using the source photo as a subject reference for exact silhouette and proportions.
3. The compositor synthesizes one continuous journal-paper surface for the entire canvas.
4. Original photo pixels are mounted without pixel-level filtering.
5. Torn-edge masks, restrained shadows, motif placement, and exact typography are added deterministically.
6. The result is checked for aspect ratio, crop fraction, source-pixel integrity, transparency, paper continuity, and safe margins.

This split workflow prevents generated white rectangles, checkerboards, stains, and color casts from leaking into the paper panel.

## Manual compositor use

The compositor is mainly used by the skill, but it can also be run directly with Python and Pillow.

Preview the resolved layout:

```bash
python scripts/compose_direct_split.py \
  --photo path/to/photo.png \
  --plan
```

Assemble a preserved-photo composition from a transparent motif:

```bash
python scripts/compose_direct_split.py \
  --photo path/to/photo.png \
  --motif path/to/motif-rgba.png \
  --output path/to/final.png \
  --caption "QUIET MORNING"
```

Run `python scripts/compose_direct_split.py --help` for layout, aspect, crop, motif, paper, shadow, and typography options.

## Installation

```bash
git clone https://github.com/casperrr0706-maker/make-paper-cut-collage.git
```

Place the complete folder in your skills directory. On Doubao/agent-mode systems, this is typically:

```text
~/.doubao/agent_mode/workspace/.user_skills/make-paper-cut-collage/
```

If the skill does not appear immediately, reload the agent so the skill catalog can discover it, then invoke it with `$make-paper-cut-collage` or a matching natural-language request.

Built-in image generation is used by default. Running the compositor manually requires Python and Pillow.

## Project structure

```text
make-paper-cut-collage/
|-- SKILL.md
|-- README.md
|-- README.zh-CN.md
|-- LICENSE
|-- agents/
|   `-- openai.yaml
|-- assets/
|   |-- style-references/
|   `-- examples/
|-- references/
|   |-- prompt-recipes.md
|   `-- style-system.md
`-- scripts/
    `-- compose_direct_split.py
```

## Customization

- Edit [`references/style-system.md`](references/style-system.md) to adjust the visual language, scale, palette, paper, exclusions, and preserved-photo rules.
- Edit [`references/prompt-recipes.md`](references/prompt-recipes.md) to refine generation templates and examples.
- Update [`scripts/compose_direct_split.py`](scripts/compose_direct_split.py) for deterministic geometry, photo mounting, paper synthesis, shadows, and typography.
- Add curated images under [`assets/style-references/`](assets/style-references/) and document their exact role in the style system. Treat references as evidence for style, never reusable subject templates.
- Keep [`SKILL.md`](SKILL.md) focused on routing, essential workflow, and non-negotiable invariants.

## Known boundaries

- Face identity is not preserved in paper-only transformations.
- Complex scenes are intentionally simplified to a few anchors and supporting echoes.
- Exact text is added deterministically in preserved-photo mode; generated freeform lettering is avoided.
- Source-photo preservation applies only when the request explicitly asks to keep or show the original photograph.
