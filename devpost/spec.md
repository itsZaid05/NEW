---
status: approved
---

# NyaySaathi — Technical Specification

## Technical Overview & Stack
- **Backend runtime:** Python 3 standard-library HTTP server (`main.py`, `server/app.py`); no web framework is required.
- **Core pipeline:** Case-state extraction and safety signals (`server/core.py`), source allow-listing and passage verification (`server/pipeline.py`), legislation lookup and CLML parsing (`server/legislation.py`), and transparent ranking (`server/app.py`).
- **Providers:** Legislation and gated case-law providers (`server/providers.py`); optional Gemini client and constrained text polishing (`server/gemini.py`, `server/llm.py`).
- **Story and packet construction:** UPL-safe educational story and structured packet builders (`server/story.py`).
- **Frontend:** Vanilla HTML (`index.html`), CSS (`style.css`), and JavaScript (`app.js`).
- **Data and evaluation:** Evaluation scenarios (`evaluation/scenarios.json`) and offline evaluation runner (`server/evaluate.py`). `models/hybrid_rag_model.py` and `data/nyaysaathi_complete_dataset.json` are a deprecated India-focused prototype, not active dependencies of the production server.
- **Testing:** Pytest unit tests (`tests/test_core.py`) and a CLML test fixture (`tests/fixtures/sample_clml.xml`).

## Where It Runs & Run Instructions
Run from the repository root with Python 3:

```bash
pip install -r requirements.txt
python -m server.app
# Open http://localhost:8000
```

Set `PORT` to change the HTTP server port. Optional provider configuration belongs in a local `.env`; never commit credentials. `python main.py` starts a separate legacy interactive RAG CLI and is not the browser server.

Run the tests and offline evaluation with:

```bash
python -m pytest -q
python -m server.evaluate
```

## Architecture & Components

### 1. Browser Intake & Case Preparation (`index.html`, `style.css`, `app.js`)
Renders the intake, source-grounded results, user-correctable case state, evidence checklist, story, and case packet. Sends requests to the local API and keeps UI state in the browser.

### 2. HTTP API & Triage Orchestration (`server/app.py`, `server/core.py`, `server/pipeline.py`)
`server/app.py` serves static assets and JSON endpoints. `server/core.py` creates and confirms case-state data and content-free audit events. `server/pipeline.py` applies retrieval allow-lists, relevance checks, ranking, and explicit abstention.

### 3. Official Sources & Legislation (`server/providers.py`, `server/legislation.py`)
Legislation retrieval uses official sources and OGL attribution. CLML provisions are parsed and ranked where the source provides suitable data. Case-law search is blocked unless both licence-related configuration gates are enabled.

### 4. Story, LLM & Case Packet (`server/story.py`, `server/llm.py`, `server/gemini.py`)
Story construction requires grounded propositions. Optional Gemini polishing receives verified proposition data rather than the original narrative and is rejected if it adds unsupported citations or outcome probabilities. Packet construction distinguishes case facts, verified sources, and unknowns.

## API Surface
- `GET /api/health` — reports server and provider status.
- `GET /api/search/legislation?q=...` — searches legislation.
- `GET /api/search/caselaw?q=...` — searches case law only when the licence gate allows it.
- `GET /api/legislation/provisions?url=...&q=...` — retrieves and ranks provisions.
- `GET /api/lawyers` — returns the explicit no-directory notice.
- `GET /api/audit` — returns recent content-free audit events.
- `POST /api/case-state` — creates a case state from a narrative and optional category.
- `POST /api/case-state/confirm` — confirms user facts and evidence statuses.
- `POST /api/explain` — runs retrieval, verification, explanation, and optional constrained polishing.
- `POST /api/story` — builds a story only from grounded output.
- `POST /api/packet-v2` — builds a structured case packet.
- `POST /api/handoff` — records a consented handoff intent in memory; no lawyer service receives the case.

## Data & Privacy Model
- Intake is not saved in a persistent server database.
- The server suppresses request-path logging and records only content-free audit metadata.
- The optional LLM receives verified propositions, not the original narrative.
- A handoff packet is not transmitted to an external recipient by this PoC.
- The deprecated static India-focused corpus must not be treated as validated legal authority.

## Constraints & Resolved Decisions
- **Legal accuracy:** A retrieved source must pass URL allow-list and passage relevance checks; otherwise the result abstains.
- **Case-law licensing:** The feature remains off until written permission and licence terms are confirmed.
- **LLM safety:** LLM use is optional and may only polish grounded propositions; deterministic behavior remains the default.
- **Legacy code:** `main.py` starts a separate interactive CLI backed by the deprecated model. The active browser server is `python -m server.app`; the launch-path ambiguity should be resolved before claiming `python main.py` launches the web UI.
