# FE — Engineer Profile PDF on the Zoho Engineer Record

Front-of-house notes for **SD-3529**. There is no platform UI in this story; the surface is inside Zoho.

## Option 1 — button on the Zoho record

- Label: **Download Profile** — the same words as the platform button, so the two surfaces read as one feature.
- Placement: on the engineer (candidate) record, near the profile summary, not buried in a related list.
- States: idle → generating (the user must see that something is happening; generation is a live render) → file delivered, or a visible failure message.
- The filename comes from the response — never composed in Zoho.

## Option 2 — attachment on the record

- The attachment is labelled so its freshness is obvious, e.g. **Vetted Engineer Profile — generated 18 Sep 2026**.
- Only ever one such attachment per engineer; a new push replaces it.
- If the last push failed, the record says so rather than presenting an undated file.

## Not in scope

- Any change to the document itself (SD-3528) or to the platform button (SD-3526).
