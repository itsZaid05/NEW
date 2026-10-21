---
status: approved
---

# NyaySaathi — Proof of Concept Scope

## The Idea & Intended User
**NyaySaathi** is a privacy-first, stigma-free legal triage and case-preparation companion for people in England and Wales who face urgent disputes such as housing problems, workplace issues, or consumer disputes. It helps people organize their account and understand relevant official information before speaking with a regulated adviser. It also helps legal clinics and solicitors review a clearer, structured case summary.

## Inspiration & Identity
People may delay seeking help because legal processes are confusing, stressful, and expensive. Lawyers and advice workers also spend time extracting basic facts and timelines from unstructured accounts. NyaySaathi provides a calm first step that turns a user's own account into organized preparation, while making clear that it is not a lawyer and does not decide legal merits.

## The Unique Kernel
NyaySaathi combines **source-grounded legal information**, **safety-aware intake**, and **structured case preparation**:
1. It screens for urgent safety signals and organizes user-stated facts without treating allegations as verified findings.
2. It retrieves and ranks official legislation, then only presents propositions supported by relevant source passages. If sources are missing or irrelevant, it explicitly abstains.
3. It highlights practical evidence gaps and produces a fictionalized, educational Story-Mode explanation and a lawyer-ready Case Summary Sheet.

Live case-law retrieval is disabled until the required computational-analysis licence is granted and its terms are reviewed. The legacy India-focused static-data RAG prototype is deprecated and is not used by the active UK server.

## The Core Loop
1. **Confidential Intake:** The user describes their situation in plain language without an account or identity fields. The browser keeps the working narrative locally.
2. **Grounded Triage:** The backend builds an editable case state, screens for safety, searches permitted official sources, filters and verifies relevant passages, and abstains when it cannot ground an answer.
3. **Case Preparation:** The user reviews facts, timeline, candidate issues, evidence status, source passages, practical risks, and questions to discuss with an adviser.
4. **Consent-First Handoff:** The user can prepare an exportable packet. Any handoff endpoint requires explicit consent; no lawyer marketplace is connected.

## What "Working" Looks Like
A user describes a housing or employment problem and submits it for triage. The app:
- identifies possible urgent safety signals;
- retrieves relevant official legislation and shows source passages and uncertainty;
- avoids fabricated citations and outcome predictions, and abstains if evidence is insufficient;
- lets the user correct or confirm extracted case-state details;
- provides a fictionalized educational story only when grounded source material is available; and
- prepares a structured Case Summary Sheet with a timeline, evidence gaps, and questions for a lawyer.

## The POC Boundary
The proof of concept is the existing Python standard-library HTTP server and vanilla HTML/CSS/JavaScript client. It supports official legislation retrieval for England and Wales, safety-aware case preparation, optional server-side Gemini rephrasing of verified propositions, deterministic fallback behavior, and offline evaluation scenarios. The active server does not use `models/hybrid_rag_model.py` or `data/nyaysaathi_complete_dataset.json`; those files are a deprecated India-focused prototype.

## Explicitly Cut
- **No accounts or persistent case database:** Requests are processed in memory; the browser may hold the current working session.
- **No live case-law search until licensed:** Case-law retrieval remains gated pending written licence approval and review of its conditions.
- **No lawyer marketplace, chat, calls, billing, or court e-filing:** The handoff stops at preparing a packet for the user to share.
- **No legal outcome prediction:** The app does not estimate chances of success, costs, or case duration.
- **No India-law advice from the active server:** The legacy India-focused dataset is not part of this PoC's active legal-information flow.
