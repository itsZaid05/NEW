# NyaySaathi

NyaySaathi is a proof-of-concept legal information and case-preparation web app for England and Wales. It helps users organize a plain-language account, review relevant official legislation, and prepare questions and a case summary for a qualified adviser.

It is an educational tool, not a law firm, legal adviser, or emergency service. It does not determine legal rights, predict outcomes, or provide a connected lawyer marketplace.

## License

This project is licensed under the [MIT License](./LICENSE).

## Run locally

Use Python 3.10 or later. From the repository root:

```bash
python -m server.app
```

Then open [http://localhost:8000](http://localhost:8000). Set the `PORT` environment variable to use a different port.

The browser app is served by `server.app`. `python main.py` starts a separate legacy command-line prototype and is not the web-app entry point. No Node.js build step is used.

## What the proof of concept does

- Accepts a plain-language narrative without requiring an account.
- Builds a case state with possible issue areas, safety signals, a timeline, and information gaps.
- Searches official legislation through legislation.gov.uk and checks retrieved source passages before presenting grounded propositions.
- Abstains explicitly when it cannot verify relevant source material.
- Keeps live case-law retrieval disabled unless the required computational-analysis licence has been granted and confirmed in configuration.
- Lets users confirm case details and prepare a structured case packet with evidence status and questions for a lawyer.
- Offers fictionalized educational Story-Mode output only when there is grounded source material.
- Supports optional server-side Gemini rephrasing of verified propositions; deterministic behavior remains available without an API key.

## Important limits

- Case-law retrieval is licence-gated and off by default. Do not enable it until written permission and its terms have been reviewed.
- The app does not connect users to lawyers, transmit a case packet to a lawyer, file documents in court, or provide outcome probabilities.
- Intake data is not stored in a server database, but the browser keeps case state in local storage. Avoid entering unnecessary identifying or highly sensitive details, especially on a shared device.
- `models/hybrid_rag_model.py` and `data/nyaysaathi_complete_dataset.json` are a deprecated India-focused prototype and are not used by the active web-server flow.

## Configuration

The active server reads provider configuration from its process environment; it does not automatically load `.env`. Use `.env.example` as a reference when configuring your shell or deployment environment. Never commit API keys. Gemini is optional; case-law flags must remain `false` until licensed.

Relevant environment variables:

| Variable | Purpose |
| --- | --- |
| `PORT` | HTTP server port (default: `8000`) |
| `GEMINI_API_KEY` | Optional Gemini rephrasing provider |
| `LLM_MODEL_EXPLANATION` | Optional Gemini model selection |
| `CASELAW_LIVE_ENABLED` | Must remain `false` until case-law use is authorized |
| `CASELAW_COMPUTATIONAL_LICENCE_CONFIRMED` | Must remain `false` until the licence is confirmed |

## Tests and evaluation

Install the test runner if it is not already available, then run:

```bash
python -m pytest -q
python -m server.evaluate
```

The evaluation runner uses offline fixtures; it does not verify live provider availability. Live legislation connectivity should be checked in the target deployment environment.

## Project map

- `index.html`, `style.css`, `app.js` — browser interface.
- `server/app.py` — standard-library HTTP server and JSON API.
- `server/core.py`, `server/pipeline.py` — case-state handling, safety signals, source verification, and abstention.
- `server/providers.py`, `server/legislation.py` — official-source retrieval and provision parsing.
- `server/story.py`, `server/llm.py`, `server/gemini.py` — grounded Story-Mode output and optional constrained rephrasing.
- `evaluation/scenarios.json`, `server/evaluate.py`, `tests/` — offline evaluation and tests.
- `devpost/` — project scope, requirements, technical specification, build checklist, and app map.

The existing Git history is preserved; removed development notes may still be visible in earlier commits.
