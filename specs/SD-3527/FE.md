# FE — Engineer Profile: PDF Generation Service

Frontend notes for **SD-3527**. There is **no UI in this story**; it exists so the contract the button consumes is written down in one place.

## What the FE does with the response

- Calls the endpoint with the engineer id currently shown, expecting `application/pdf`.
- Hands the response to the browser download. **The filename comes from `Content-Disposition`** — the FE never composes or guesses it.
- Never renders, paginates or previews the document, and never opens a print dialog.
- Never caches the file: a second click is a second live render.

## Status handling

| Response | FE behaviour |
|---|---|
| 200 + PDF | Download starts, success toast (SD-3526) |
| 403 | Error toast; no retry loop — the user cannot fix it by clicking again |
| 404 | Error toast; the profile is gone or archived |
| 5xx / timeout | Error toast; the button returns to idle so the user can retry |

Error copy never exposes status codes, endpoints or stack traces.

## Reference

- Contract: `specs/SD-3527/BE.md`.
- Button states and copy: `specs/SD-3526/FE.md`.
