# BE — Engineer Profile: PDF Generation Service

Backend handoff for **SD-3527**. The service behind the **Download Profile** button.

---

## Shape

One authenticated endpoint. Takes an engineer id, returns `application/pdf`.

- **Server-side render.** Headless Chrome (or equivalent) renders the document template with the engineer's values at A4 and returns the bytes. The browser's print path is never involved, so output does not vary by client, OS or paper settings.
- **The template is `prototype/index.html`.** It is a print template, not a mock: A4 sheets, design-system fonts and assets, print-colour accurate. Content and layout rules are in `specs/SD-3528/`.
- **No caching, no storage.** Every call renders fresh. Nothing is written to disk or to a bucket, nothing is pre-built on profile save.

## Live content

- The profile is read **at request time**, from the same stores the live profile page reads. No snapshots, no copies, no nightly extract.
- An engineer's edit to their About, a newly approved review, a changed availability — all present in a file downloaded a moment later.
- **The written review is the full stored body** — the same text the profile page's "View full review" modal returns, never the card's truncated preview.
- The **profile picture** is the engineer's stored picture; fall back to the platform's initials avatar only when none is stored.

## Output

| Property | Value |
|---|---|
| Format | PDF, A4 portrait |
| Text | Selectable and searchable — not a page of rasterised images |
| Fonts | Embedded (Bricolage Grotesque, Montserrat) |
| Colour | Print-colour accurate (`print-color-adjust: exact`) |
| Pages | As many as the content needs; never truncated to fit a budget |
| Filename | `Castillians-{Engineer-Name}-Profile-{YYYY-MM-DD}.pdf`, in `Content-Disposition` |

## Failure and abuse

| Case | Response |
|---|---|
| Unauthenticated request | **401/403**, no body |
| Unknown or archived engineer id | **404** |
| Render failure or timeout | **5xx**, no partial body — nothing half-rendered reaches the user |
| Repeat clicks / concurrent requests | Per-user rate limit; concurrent requests for the same engineer are safe and independent |

- Generation should complete comfortably inside the button's busy state. If a render can exceed that, return a streamed response rather than making the user wait on a blank state — but do not introduce a job queue or an email-me-the-file flow without agreeing it first.

## Access

- **Any user logged in to the Manager dashboard may download any engineer's profile.** No per-role gate, no per-bench gate, and no dependency on whether the engineer features on Engineer Finder.
- Authentication is still enforced server-side: an unauthenticated request gets 401/403 and no file.
- *Outdated on 18 Sep. Previously: "Server-side enforcement mirrors access to the engineer profile page; a user who may not view that profile receives 403."*
- **Engineers do not have this download** on their own dashboard. The surfaces are the Manager dashboard (SD-3526) and the Zoho engineer record (SD-3529).

## Audit

- **Required.** Record each generation: **who** requested it, **which engineer**, **when**, and the **origin** (Manager dashboard or Zoho record).
- Queryable by engineer and by requester.
- **Open:** where it surfaces — an internal dashboard view, or the log store only.

## Confidentiality

- The endpoint returns exactly what the profile page exposes. No rate, no earnings, no retained margin, no engineer-facing figures (BE-04, BE-08).
- No contact details beyond what the profile page itself shows.

## Not in scope

- What the document contains and how it looks — **SD-3528**.
- The button, its states and its toast — **SD-3526**.
