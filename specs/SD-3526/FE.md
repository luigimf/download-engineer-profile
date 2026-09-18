# FE — Engineer Profile: Download Profile Button

Angular handoff for **SD-3526**. Adds one control to the engineer profile page on the logged-in Manager dashboard.

**Design system:** V2 Castillians Design System (live). **Button design and breakpoints:** [Figma 5850-5344](https://www.figma.com/design/A4ZWwuOmoeUr7p72TYIiLV/castillians.com?node-id=5850-5344).

---

## Where it appears

The engineer profile page is reachable from several places, and the button belongs on **all** of them:

- Engineer Finder results → engineer profile (`/engineer-finder/engineer/{id}?...&view=PIPELINE&searchId={searchId}`)
- A Virtual Bench's Engineers section → engineer profile
- Pipeline / shortlist views → engineer profile

One component, one place in the profile-page template. If a route renders the profile in a card at a narrower breakpoint, the Figma node governs how the button sits on that card.

- The button never overlaps the profile card's badges, labels or the vetting mark.
- It never outranks the page's engagement CTA — it is a secondary action.
- Hit target ≥ 44px at touch breakpoints.

## Styling

- Taken from the **live design system** button component. No one-off button, no new colours, no bespoke hover.
- Hover is the design system's own state (solid primary darkens to `#BD0F24`; outline fills grey with ink border — whichever variant Figma specifies).
- Label: **Download Profile**. Title Case, per the brand's button-label convention.

## Behaviour

| State | What the user sees |
|---|---|
| Idle | Button enabled, label **Download Profile** |
| Requesting | Busy state (design-system spinner/disabled treatment), not clickable — **one click, one file** |
| Success | Browser download starts; platform success toast: **"Success. The profile has been downloaded to your device."** |
| Failure | Busy state clears, button usable again, platform error toast saying the download did not complete and can be retried |

- The FE renders no document, opens no print dialog, and fetches no profile fields for the file. It calls the endpoint (SD-3527) with the engineer id currently shown and hands the response to the browser.
- The **filename comes from the response** (`Content-Disposition`) — the FE never composes it.
- Nothing is cached client-side; a second click is a second live render.
- Keyboard reachable; busy state announced to assistive tech (`aria-busy`, disabled).
- **The button is shown to every logged-in Manager dashboard user, for every engineer.** It is never hidden or disabled by role, by bench membership, or by whether the engineer features on Engineer Finder.
- No change to the profile page's own content, tabs or layout in this story.

## Copy

- Button: **Download Profile**
- Success toast: **Success. The profile has been downloaded to your device.**
- Error toast: platform standard; states the download did not complete and can be tried again. Never exposes an endpoint, status code or stack.

## Email notifications in this flow

None.

## Reference

- Figma: node 5850-5344 — button on the profile card at each breakpoint.
- Endpoint contract: `specs/SD-3527/BE.md`.
- Document itself: `prototype/index.html`.
