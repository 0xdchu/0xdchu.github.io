---
name: brand-identity-kit
description: "Use when a client has a product but no identity: naming checks, logo marks as SVG, color and type system, layout rules and a one-page brand sheet. HTML/CSS and SVG only; no stock assets, no fonts that are not installed."
metadata:
  version: 1.0.0
---

# Brand identity kit

Scope this skill honestly: it produces a **working identity system** — marks, palette, type scale, usage rules — as SVG and HTML/CSS, reviewed as images. It does not produce a full brand book, custom typefaces, illustration systems or trademark clearance. Say that out loud before starting, so nobody expects an agency deliverable.

## Order of work (do not skip a step)

1. **Inputs.** Ask for, and refuse to invent: product name, one-line what-it-does, audience, 3 competitors, the product screenshots, and any word the client already uses about itself. Missing input is a blocker, not a place to guess.
2. **Positioning line first.** The identity follows the position, not the other way round. Take the positioning statement from `positioning-brief` (Shikamaru) or ask for it. Without it, stop.
3. **Three directions, not one.** Each direction: a name, one sentence of intent, a mark, a palette, a type pairing. Present as three rendered PNGs side by side. The owner picks one; two directions die here on purpose.
4. **System the winner.** Palette with roles (surface, ink, muted, accent, signal), type scale (h1–h4, body, caption) with weights and line-heights, spacing scale, radius, and one hairline rule. Hex values, not names.
5. **Usage sheet.** One HTML page: mark at 3 sizes, mark on light/dark, minimum size, clear space, wrong uses with a red X, colour swatches with hex, type specimen, and 4 sample applications (avatar, banner, email header, slide).

## Rules that keep the output usable

- **SVG marks, drawn as paths and shapes.** No embedded raster, no text that must stay a font. Keep a `mark.svg`, a `mark-mono.svg` and a `mark-badge.svg` (square, for avatars).
- **Legible at 32 px.** Render the mark at 32, 64, 256 px and look at it. If it breaks, it is not finished.
- **Contrast checked, not eyeballed.** Body text on surface ≥ 4.5:1; large text ≥ 3:1. Report the numbers.
- **Two typefaces maximum.** Use what is installed on the machine or a system stack; never promise a font that would need a licence.
- **No stock imagery, no generated logo that cannot be reproduced.** Every mark must be reproducible from the SVG in the repo.
- **Palette works in one colour.** A brand that needs four colours to survive is fragile; test a mono print version.
- **Names.** Check the handle and domain availability only by asking the owner to check; state clearly that trademark clearance is outside this team's capability.

## Deliverable layout

```
work/brand/<client>/
  brief.md             # inputs, positioning line, 3 directions, decision, date
  marks/mark.svg mark-mono.svg mark-badge.svg
  tokens.css           # :root { --surface, --ink, --muted, --accent, --signal; type + space scale }
  sheet.html           # usage sheet, the one page the client actually reads
  samples/             # avatar.png, banner.png, email-header.png, slide.png
  README.md            # what is fixed, what is a placeholder, what is out of scope
```

## Review gate before it leaves

Hand to Usop and the owner with: the three directions image, the chosen one with the reason, the contrast table, the 32/64/256 px renders, and the explicit list of what is *not* included (brand book, trademark, font licence, illustrations). A client who is told the boundary up front argues about taste, not about being misled.
