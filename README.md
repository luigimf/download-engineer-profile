# Download Engineer Profile in PDF — handover

Epic **[SD-3525](https://castille-labs.atlassian.net/browse/SD-3525)** · release **Castillians Engine** (version 10335) · all items **P3**, priority Medium, assigned to Patrick Udochukwu.

A **Download Profile** button on the engineer profile page returns the engineer's Vetted Engineer Profile as a Castillians-branded A4 PDF, generated server-side from the live profile.

## What is in this repo

| Path | What it is |
|---|---|
| `prototype/index.html` | The document prototype, **standard** output — open it in a browser. Self-contained (styles, fonts, logo, marks, photo inlined), no build step. **This is the visual and structural source of truth for the PDF.** |
| `prototype/cobranded.html` | The same prototype opening on the **cobranded** output (client logo + Powered by Castillians). Example client: Exertis. |
| `prototype/reference.pdf` | Reference PDF for QA, **standard** output — A4, printed from the prototype. Compare generated output against it page for page. |
| `prototype/reference-cobranded.pdf` | Reference PDF for QA, **cobranded** output (example client: Exertis). |
| `specs/SD-3526/` | Download button — `FE.md`, `BE.md` |
| `specs/SD-3527/` | PDF generation service — `FE.md`, `BE.md` |
| `specs/SD-3528/` | PDF document content & layout — `FE.md`, `BE.md` |
| `specs/SD-3529/` | The same PDF on the Zoho engineer record — `FE.md`, `BE.md` |

The prototype is a **print template**, not a screen. It renders five fixed A4 sheets (210 × 297 mm) for the reference engineer; the real page count follows the engineer's content.

## Stories

| Key | Story | Owns |
|---|---|---|
| [SD-3526](https://castille-labs.atlassian.net/browse/SD-3526) | Download Profile Button | The control: placement across every engineer-profile entry point, Figma breakpoints, design-system styling and hover, busy state, success toast |
| [SD-3527](https://castille-labs.atlassian.net/browse/SD-3527) | PDF Generation Service | The endpoint: live read, server-side render, A4 file, filename, permissions, failures, audit |
| [SD-3528](https://castille-labs.atlassian.net/browse/SD-3528) | Document Content & Layout | What the document contains and how it is laid out — the extraction rules |
| [SD-3529](https://castille-labs.atlassian.net/browse/SD-3529) | PDF on the Zoho Engineer Record | The same document from the engineer's Zoho profile — button if Zoho can authenticate, otherwise the latest PDF kept on the record |

## Three rules

1. **The PDF is an extraction, not a new document.** Every value already exists on the engineer's profile page. The only computed figure is "Based on n reviews".
2. **It is always live.** Rendered at the moment of the click, from the profile as stored then.
3. **Generation is server-side.** The browser print path is never used, so every manager gets an identical file.

## Two outputs — standard and cobranded

The header depends on **who downloads**:

- **Standard** — the downloading manager's Client entry has *Cobranded Client* = No (or there is no manager context, e.g. the Zoho record). Header: the Castillians logo with tagline, as today.
- **Cobranded** — the manager's Client entry has *Cobranded Client* = Yes (SD-3325). Header: the **client's logo** (the one uploaded in the Cobranded Platform Set Up card — SVG, 157 × 56 artboard) top left, then the **Powered by Castillians** logo beside it. Nothing else in the document changes.

## Build order

1. **SD-3528** — pins what the renderer must output; the prototype is already the template.
2. **SD-3527** — the service that renders it.
3. **SD-3526** — the button, wired to the endpoint.
4. **SD-3529** — the Zoho surface, once the endpoint exists.

## Reference

- Button design and breakpoints: [Figma node 5850-5344](https://www.figma.com/design/A4ZWwuOmoeUr7p72TYIiLV/castillians.com?node-id=5850-5344)
- Example live profile route: `castillians.com/engineer-finder/engineer/{id}?viewIndex=0&candidateId={id}&view=PIPELINE&searchId={searchId}`
- Design system: V2 Castillians (live) — Bricolage Grotesque display, Montserrat body, brand red `#D01329`, ink `#141313`, 2px `#E5E5E5` hairlines, green rating bars.

## Settled (18 Sep)

- **Audit log is required** — who, which engineer, when, and the origin (Manager dashboard or Zoho). Recorded in SD-3527.
- **Access: any user logged in to the Manager dashboard may download any engineer's profile.** No role gate, no bench gate, no Engineer Finder dependency.
- **Engineers do not get this download** on their own dashboard.
- **Reference PDFs** for QA: `prototype/reference.pdf` (standard) and `prototype/reference-cobranded.pdf` (cobranded).

## Open questions

- **SD-3529 implementation**: can a Zoho button/widget authenticate to the platform endpoint (service token / signed URL)? Yes → live button; no → latest PDF kept on the record. For Samuel.
- **Where the audit log surfaces**: internal dashboard view, or log store only?
