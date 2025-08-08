# Intelligent Document Matcher — Text-Only Case Study (Redacted)

> **Summary:** System that parses long-form documents, extracts high-signal fields, aligns them with trusted reference data, and produces a structured output for downstream workflows.  
> **UI:** A Next.js web app with **Firebase Authentication** for secure access; users can upload files, preview extractions, review/approve suggestions, and export results.  
> **Note:** Public, text-only portfolio summary. No client names, screenshots, datasets, or production code.

---

## Problem (generalized)
Teams receive complex documents with inconsistent layouts and wording. Manually finding the right fields, normalizing values, and preparing a clean output is slow and error-prone.

## Outcome (generalized)
- Minutes → seconds per document for extraction and preparation
- Consistent, structured output for automated pipelines
- Review/approval UX so humans stay in control

---

## What it does (high level)
- **Layout/section-aware parsing** to target likely regions and ignore boilerplate
- **Multi-record extraction** when a document contains multiple items/rows
- **Normalization & validation** (codes, units, naming, canonical forms)
- **Reference matching** against trusted tabular data (CSV/Excel/DB)
- **Confidence scoring** with a review queue for low-confidence fields
- **Export** to a clean, machine-readable format

---

## My role
- Designed the **end-to-end pipeline** (ingest → extract → match → output)
- Implemented **semantic extraction** and **region routing** for higher precision
- Built **matching logic** to prefer trusted reference values and fill gaps from extraction
- Created the **export generator** and added structured logging/metrics
- Shipped a **Next.js UI** with **Firebase Auth** and a review/approve workflow

---

## Technical approach

### Retrieval & extraction strategy
- **Primary path (local):**  
  - Local **embeddings + vector index** to locate relevant spans and labels across diverse layouts  
  - Lightweight **rules/patterns** for high-precision tokens  
  - Per-field **confidence scoring** from multiple signals (pattern strength, semantic proximity, consistency checks)
- **Fallback path (LLM):**  
  - If confidence is below a threshold or fields conflict, call an **LLM for disambiguation** (few-shot prompts over the candidate spans)  
  - The LLM never sees more than the minimum redacted context required  
  - System can run **LLM-off** (local-only) by policy or environment flag

### Matching & normalization
- Prefer trusted **reference table** values when present and consistent  
- Normalize codes/units/names to canonical forms  
- Resolve conflicts with deterministic rules + confidence tie-breakers

### Frontend & access control
- **Next.js** single-page app  
- **Firebase Authentication** (email/password or federated)  
- Role-aware UI: upload → extract → review/approve → export  
- Inline diffs show “extracted vs reference” with accept/override controls

---


