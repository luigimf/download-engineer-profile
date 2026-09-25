# BE — Engineer Profile PDF: Document Content & Layout

Data handoff for **SD-3528**. What the renderer must put in the document, and where each value comes from. Layout is in `FE.md`; the service is SD-3527.

---

## Source of truth

The document is an **extraction** of the engineer's live Vetted Engineer Profile page. It introduces **no new fields** and has **no independent content model** — if the profile page's fields change, the document changes with them.

| Document section | Source |
|---|---|
| Profile header | Profile picture, name, overall rating + score, availability, seniority level, country, timezone group, Vetted badge, AI Ready label (with its own tooltip copy) |
| About | The About tab's stored free-text field |
| CV Summary | Experience (skill + years), roles (`{title} at {COMPANY}`, dates, bullets), Education |
| Skills | Technical / AI / Job-Specific / Interpersonal — per-skill rating + the group's stored "Average Rating" |
| Manager & Client Reviews | Reviewer name and title, role, client, period, score, **full** review body, skill tags |
| "Based on n reviews" | **Derived** — count of reviews held against the engineer |

## Header variant

- Resolve the **downloading manager's** Client entry. If *Cobranded Client* = Yes (SD-3325), render the **cobranded** header with that client's uploaded logo; otherwise the **standard** header.
- The logo is read **live** from the client record — a logo replaced in the Manage Client modal appears in the next download.
- Cobranded but no logo stored (should not happen — the field is required in SD-3325): fall back to the standard header rather than print an empty slot.
- The variant depends on the viewer, not the engineer: two managers from different clients downloading the same engineer can receive different headers. Content is identical.

## Verbatim rendering rules

1. **Every string is rendered as stored — character for character.** Engineer- and reviewer-authored text keeps its own wording, spelling, punctuation, casing and typos. No paraphrase, no correction, no trimming, no sentence-casing. Examples that must survive intact: "developers career path", "Entrerpise Architect", "Core java", "integration" (lower-case), "It was a please to work with Andrew", "Does the Code abstract and removes dependencies on specific platforms".
2. **About is output raw** — the stored value with its own line breaks and spacing, as one text block. No re-wrapping, no paragraph splitting, no added headings, labels, bullets or separators, and nothing removed, including rules the engineer typed themselves.
3. **Reviews print the full stored body** — the text the "View full review" modal returns, never the card's truncated preview. Still printed exactly as stored.
4. **Ratings are not computed.** Per-skill values are the stored ratings; group averages are the stored "Average Rating" figures. A per-skill score is never derived from an average, and an average is never recomputed from the rows.
5. **Ordering is the profile page's ordering** — skills sorted highest to lowest rated within each group; groups in page order; reviews in the page's order.
6. **The only computed value in the whole document is "Based on n reviews".**
7. **Profile picture** is the stored picture, cropped as the profile page crops it. Initials avatar only when none is stored.
   - The renderer receives it already **resized to ~300 × 300 px JPEG**, centre-cropped square (SD-3527) — never the original upload.
8. **Empty sections collapse.** A section with no stored data is omitted entirely — never a heading with placeholder text, never "N/A".
9. **No PDF-only copy.** Nothing is added that the profile page does not hold: no strapline, no disclaimer, no client-specific line.

## Live read

The renderer reads at request time (SD-3527). There is no snapshot of the profile for PDF purposes, and no "last generated" state to invalidate.

## Confidentiality

Only what the profile page exposes. No rate, earnings or engineer-facing figures (BE-04, BE-08); no contact details the page does not show.
