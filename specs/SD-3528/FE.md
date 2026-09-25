# FE — Engineer Profile PDF: Document Content & Layout

Layout handoff for **SD-3528**. The document rendered by SD-3527. `prototype/index.html` in this repo is the source of truth — open it and measure from it; the notes below say what must hold true.

**Design system:** V2 Castillians (live). Nothing in the document is styled outside it.

---

## The document

- **A4 portrait**, ~0.5in margins, fixed page boxes. Five sheets for the reference engineer (Andrew Lowcock); real page count follows content.
- Every sheet carries the **logo band** (Castillians logo, linking to castillians.com, above a 2px brand-red rule) and a **footer** (engineer name · page number · castillians.com).
- Page 1's band carries the **"Vetted Engineer"** label with the blue vetted mark and the vetting note. Continuation sheets carry the engineer's name with the same label and mark.

## Header — two variants

| Variant | When | Header, left side |
|---|---|---|
| **Standard** | Downloading manager's Client entry has *Cobranded Client* = No, or no manager context (Zoho, SD-3529) | Castillians logo with tagline, 36px tall, linking to castillians.com |
| **Cobranded** | Downloading manager's Client entry has *Cobranded Client* = Yes (SD-3325) | **Client logo** first, then a 1px × 24px `#E5E5E5` divider, 12px gaps,, then the **Powered by Castillians** logo (linking to castillians.com) |

- *Outdated on 25 Sep. Previously: "Client logo: the PNG uploaded in SD-3325's Cobranded Platform Set Up card (exactly 157 × 56), rendered at 90 × 32 — same aspect, never cropped or recoloured. Powered by Castillians: assets/logo-powered-by-castillians.png (184 × 57), rendered at 84 × 26."*
- Client logo: the **SVG** uploaded in SD-3325's Cobranded Platform Set Up card (157 × 56 artboard), rendered at 90 × 32 as vector — same aspect, never cropped, recoloured or rasterised.
- Powered by Castillians: `assets/logo-powered-by-castillians.svg` (184 × 57 artboard), rendered at 84 × 26 as vector — slightly smaller than the client logo, so the client leads.
- Both logos stay vector in the output PDF, so they print sharp at any zoom.
- The variant applies to **every sheet**. Right side of the band (Vetted Engineer label, engineer name on continuation sheets), body and footer are identical in both.
- Prototypes: `prototype/index.html` (standard) and `prototype/cobranded.html` (cobranded). Reference PDFs: `prototype/reference.pdf` and `prototype/reference-cobranded.pdf`.

## Section order

1. **Profile header** — photo, name, rating, chips
2. **About**
3. **CV Summary** — Skills (years), Career history, Education
4. **Skills** — rating groups
5. **Manager & Client Reviews**

## Profile header

- **Profile picture**, circular, 76px, **cropped exactly as the profile page crops it** (cover, centred), with the blue vetted mark on the lower right. Initials avatar only when no picture is stored.
- Name in display type; the five rating bars and the score beside it; **"Based on n reviews"** in small body type.
- Chips, in this order: **availability** (green dot), **seniority level**, **country** (circular flag), **timezone group** (bold code + cities). Chip height ≈ 1.75× its type size — 10px text, 4px/8px padding, `--gray-50` fill, 1px `--gray-150` border, `--radius-md`.

## About

One block, printed **raw**: the stored value's own line breaks and spacing preserved (`white-space: pre-wrap`). No paragraph splitting, no added headings or bullets, nothing removed.

## CV Summary

- **Skills** — three-column grid of skill + years, hairline rule under each row; skill in semibold ink, years in grey.
- **Career history** — role title (display, 16px), then company and dates on one line beneath it, then bullets. A role block never splits across a page.
- **Education** — its own section, with the same red-bar section heading as the others.

## Skills (ratings)

- One card per group, in the profile page's order: **Technical Skills, AI Skills, Job-Specific Skills, Interpersonal Skills**.
- Card head: group name, "Sorted by highest to lowest rated", and an **Average Rating** pill carrying the group's own figure.
- Rows: skill name left, five rating bars right (green filled / grey empty), two columns when the group has more than six skills. Hairline rule under each row.

## Reviews

- One card per review: reviewer name and title (with initials avatar), rating bars + score on the right; **role / client / period** chips; then the **full review body**; then the review's skill tags.
- A review card never splits across a page.

## Print rules

- Cards, role blocks and review blocks: `break-inside: avoid`.
- A section heading never sits alone at the foot of a page (`break-after: avoid`).
- Long content reflows onto more sheets — type is never shrunk and content is never clipped.
- No screen-only affordances in the document: no tabs, no buttons, no hover states, no "View full review" link.

## What is deliberately absent

No strapline or headline, no confidentiality or disclaimer paragraph, no "prepared for {client}" line, no Verified chip on reviews, no engineer contact details.

## Reference

- `prototype/index.html` — the template.
- Profile-page captures (Skills / Reviews / CV Summary / About tabs) held with the design source.
