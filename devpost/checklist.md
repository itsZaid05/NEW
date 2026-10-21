---
status: approved
---

# NyaySaathi — Build Checklist & Verification Log

## Build Mode
- Mode: `fast`
- Scope: source-grounded, privacy-conscious legal triage and case preparation for England and Wales.

## Build Slices
- [x] **Slice 1: Core contracts, source controls & legislation**
  - Case-state and safety helpers in `server/core.py`.
  - Source verification pipeline in `server/pipeline.py`.
  - Legislation retrieval and CLML parsing in `server/legislation.py`.
- [x] **Slice 2: Story-Mode, provider controls & case packet**
  - UPL-safe story and packet generation in `server/story.py`.
  - Optional provider integration and deterministic behavior in `server/llm.py`, `server/gemini.py`, and `server/providers.py`.
- [x] **Slice 3: HTTP API & evaluation**
  - JSON routes in `server/app.py`.
  - Scenario evaluation in `evaluation/scenarios.json` and `server/evaluate.py`.
  - Unit tests in `tests/test_core.py`.
- [x] **Slice 4: Browser intake & case summary**
  - Frontend in `index.html`, `style.css`, and `app.js`.

## Verification & Hands-On Checkpoints
- [x] **Unit tests:** `python -m pytest -q` — 13 passed.
- [x] **Offline scenario evaluation:** `python -m server.evaluate` — 50 scenarios; issue recall 1.00, safety detection 1.00, correct abstention 1.00.
- [ ] **Browser end-to-end review:** Not run during this documentation update.
- [ ] **Live legislation provider smoke test:** Requires network access and remains a deployment check.
- [ ] **Case-law provider:** Intentionally gated pending written licence approval.

## Final Review
- [x] Documented the active England and Wales source-grounded flow and its abstention behavior.
- [x] Clearly marked the India-focused static-dataset prototype as deprecated.
- [x] Documented privacy, consent, and provider/licensing boundaries.
- [ ] Complete browser review and deployment smoke tests before describing those paths as verified.

## Existing Git History
This work preserves the repository's existing history rather than rewriting it:
- `1953429` — Add NyaySaathi application
- `039e2d6` — Initial commit

## Code Tour and App Map
See [`app-map.html`](./app-map.html) for a visual map of the browser-to-server flow.
