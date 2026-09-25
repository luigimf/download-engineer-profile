repo: luigimf/download-engineer-profile
branch: main
path: .

## Last sync

date: 2026-09-25T14:45:00Z

### Updated in this project
- Added the cobranded output (client logo + Powered by Castillians): `prototype/cobranded.html`.
- Created the handover bundle: prototype + per-story FE/BE specs for epic SD-3525.
- Bundled the `Engineer Profile PDF` canvas as a self-contained `prototype/index.html`.
- Replaced the PNG pixel targets with reference PDFs: `prototype/reference.pdf`, `prototype/reference-cobranded.pdf`.
- Added SD-3529 (Zoho engineer record) and settled access + audit rules on SD-3526/SD-3527.
- Cobranded header now uses SVG logos (client logo + Powered by Castillians) instead of PNG; `reference-cobranded.pdf` regenerated with them.
- Profile photo resized to 300 × 300 JPEG before render (SD-3527); reference PDFs regenerated with the resized photo (~3 MB; text outlined by the print tool).

## Screen map

| Deliverable | Built from |
|---|---|
| `prototype/index.html` | `Engineer Profile PDF.dc.html` (this project) |
| `specs/SD-3526`, `specs/SD-3527`, `specs/SD-3528`, `specs/SD-3529` | Jira SD-3525 epic + the profile-page captures in `uploads/` |
| `prototype/reference.pdf`, `prototype/reference-cobranded.pdf` | A4 print of `Engineer Profile PDF.dc.html`, standard and cobranded |
