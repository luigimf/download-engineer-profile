# BE — Engineer Profile PDF on the Zoho Engineer Record

Backend handoff for **SD-3529**. Puts the same PDF on the engineer's Zoho record, for every vetted engineer — including those not currently visible on Engineer Finder (e.g. while activated on a client's bench).

**One document, one generator.** This story adds a *surface*, not a variant: content and layout are SD-3528, generation is SD-3527. There is no Zoho-specific template and no Zoho-specific content rule.

---

## Option 1 — Download button on the Zoho record (preferred)

- A button/widget on the Zoho engineer record calls the platform generation endpoint with the engineer's **platform id** and hands the file to the Zoho user.
- The file is **live at the moment of the click** — same guarantee as the Manager dashboard.
- Zoho stores nothing. No attachment, no copy, no staleness.
- Authentication: the widget authenticates as a service (service token, or a short-lived signed URL minted by the platform). Never a shared credential, never an unauthenticated public URL.

## Option 2 — latest PDF attached to the record (fallback)

Only if a Zoho button cannot authenticate to the endpoint.

- The platform pushes a freshly generated PDF onto the Zoho engineer record as an attachment.
- **The latest replaces the previous** — the record never accumulates versions.
- The attachment (or a field beside it) carries the **generation timestamp**, so a user can see how current it is.
- Refresh triggers: on profile change (About, CV, skills, a newly approved review, availability, photo) **and** on a schedule as a safety net.
- A push failure is visible — the record must not silently keep a stale file with no date.

## Identity

- The Zoho engineer record must resolve to **exactly one** platform engineer id. An unmapped or ambiguous record cannot offer the download, and says so.
- Eligibility to feature on Engineer Finder is **irrelevant** here: availability of the download must not depend on it, nor on bench activation.

## Audit

Every generation or push is recorded in the same audit log as platform downloads (SD-3527), tagged with the **Zoho origin** and the acting Zoho user.

## Confidentiality

The document is the client-facing profile, unchanged: no rate, no earnings, no engineer-facing figures (BE-04, BE-08).

## Open — decides the implementation

**Can a Zoho button/widget authenticate to the platform endpoint (service token or signed URL)?** For Samuel. Yes → Option 1. No → Option 2. Nothing else blocks this story.
