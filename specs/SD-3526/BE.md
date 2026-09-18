# BE — Engineer Profile: Download Profile Button

Backend notes for **SD-3526**. The button itself needs **no new backend work**; it consumes the endpoint specified in **SD-3527**.

## What the button needs from the backend

| Need | Where it is specified |
|---|---|
| Authenticated endpoint taking an engineer id, returning `application/pdf` | SD-3527 |
| `Content-Disposition` filename | SD-3527 |
| 401/403 when unauthenticated, 404 for unknown/archived, 5xx with no partial body on render failure | SD-3527 |
| Per-user rate limit so repeat clicks cannot storm the renderer | SD-3527 |

## Access

- **Any user logged in to the Manager dashboard may download any engineer's profile.** No role check, no bench check, and the button is never conditionally hidden.
- *Outdated on 18 Sep. Previously: "Enforcement mirrors access to the engineer profile page; open whether Viewers may download."*
- Server-side enforcement is authentication only (SD-3527).
- The endpoint exposes nothing the profile page does not: no rate, earnings or engineer-facing figures (BE-04, BE-08).

## Not in scope here

- Document content and layout — SD-3528.
- Render pipeline, filename format, audit — SD-3527.
