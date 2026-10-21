---
status: approved
---

# NyaySaathi — Product Requirements Document (PRD)

## The Core Journey
*Develops `scope.md > The Core Loop` and `scope.md > What "Working" Looks Like`.*

1. **Anonymous Arrival & Reassurance:** The user opens the responsive web interface and sees a clear notice that no account is required and the output is educational information, not legal advice.
2. **Situation Input:** The user describes a situation in plain language. The interface does not ask for a name, account, or identity document.
3. **Review & Grounded Triage:** The client sends the narrative to the local Python server. The server creates a case state, screens for safety, retrieves permitted official sources, verifies source relevance, and returns grounded information or an explicit abstention.
4. **Case Preparation & Handoff:** The user reviews and corrects the facts and evidence status, then can prepare a Case Summary Sheet. Any handoff requires explicit consent; no lawyer directory or live marketplace is connected.

## Features and Behavior

### 1. Confidential Intake & Case-State Review
*Develops `scope.md > The Core Loop (Step 1)`.*
- Accepts unstructured plain-language narratives without requiring identifying fields.
- Sends intake to `POST /api/case-state` and supports user-confirmed facts and evidence statuses through `POST /api/case-state/confirm`.
- Keeps narrative content out of request-path logs and does not write case narratives to a database.
- Allows the user to review and correct extracted facts before relying on them in a case packet.

### 2. Safety-Aware, Source-Grounded Triage
*Develops `scope.md > The Unique Kernel` and `scope.md > The Core Loop (Step 2)`.*
- Applies deterministic safety screening and identifies potential issue areas.
- Searches official England and Wales legislation through the configured legislation provider. Case-law search stays disabled unless the licence gate is satisfied.
- Ranks and filters sources using allow-listed URLs and source-passage relevance checks.
- Presents explanations only when supported by retrieved source passages; otherwise it returns an `insufficient_sources` result with no propositions.
- Displays source references, uncertainty, and relevant provision details when available.
- Does not claim that a legal provision is in force unless source metadata supports that claim, and does not provide win probabilities, cost predictions, or timelines.
- Shows practical risk signals and evidence gaps as preparation prompts, not as legal instructions.

### 3. UPL-Safe Story-Mode & Optional LLM Rephrasing
*Develops `scope.md > The Unique Kernel`.*
- Frames available procedural education around a clearly fictional parallel situation.
- Does not generate a story from an ungrounded or insufficient-source result.
- Uses the optional server-side Gemini integration only to rephrase verified propositions; the LLM must not introduce new citations or unsupported outcome predictions.
- Keeps deterministic output available when no provider key is configured.

### 4. Case Summary Sheet, Export & Consent
*Develops `scope.md > The Core Loop (Step 3)` and `scope.md > The POC Boundary`.*
- Produces a packet containing user-stated facts, a chronological timeline, candidate issues, missing information, evidence checklist status, verified sources, and questions for a lawyer.
- Labels allegations and user-confirmed information by provenance.
- Includes an educational-not-legal-advice disclaimer.
- Requires explicit consent for the handoff endpoint; the current application does not transmit the packet to a connected lawyer.

### 5. Validation, Errors & Limits
- Empty narrative input is rejected with an actionable validation message.
- Unavailable or irrelevant sources result in an explicit abstention rather than a success-shaped answer.
- Provider failures return an error state and do not silently substitute unverified authorities.
- The interface can explain that urgent danger needs immediate local emergency support; it does not replace emergency services.

## Look and Feel
- **Visual direction:** Calm, accessible civic-tech styling that reduces anxiety and makes uncertainty visible.
- **Layout:** Responsive intake and case-summary panels, with source passages and evidence gaps clearly separated from user statements.
- **Print:** The case summary is designed for review and printing without representing itself as a court filing or legal opinion.

## Product Decisions
1. **Grounded information over generic advice:** Only verified source-backed propositions are presented; insufficient evidence results in abstention.
2. **Licence-aware retrieval:** Live case-law retrieval stays disabled until written licence terms are received and reviewed.
3. **Human-correctable intake:** User-confirmed facts remain distinct from system-extracted information.
4. **Privacy by minimization:** No user account or persistent case database is part of this PoC; narratives are excluded from server request-path logging and external LLM calls.
5. **Legacy prototype separation:** The India-focused static dataset and hybrid RAG module are documented as deprecated and are not presented as active UK legal retrieval.

## The Proof-of-Concept Boundary (Now vs. Later)
- **Now:** Anonymous intake, safety signals, source-grounded official legislation search, explicit abstention, case-state confirmation, optional constrained LLM rephrasing, case-summary preparation, and offline scenario evaluation.
- **Later:** Licensed case-law retrieval, validated semantic/hybrid retrieval evaluation, multilingual intake, verified adviser directory, persistent user-controlled case storage, live handoff integrations, and court e-filing.
