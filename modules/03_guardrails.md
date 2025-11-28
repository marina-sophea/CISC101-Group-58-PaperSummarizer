> Change Log (Nov 28, 2025)
> - Added `evidence_mode` flag, including `"strict"` behavior for claims and results.
> - Defined standardized warning messages for missing and very short sections.
> - Clarified how guardrails interact with Modules 01 and 02.

# Module 03 — Guardrails & Evidence Checks

## Purpose

This module enforces safety, evidence, and quality constraints across the summarizer. It ensures the system only uses information from the provided text, handles missing or very short sections, and exposes its conservative behavior to the user through clear warning messages.

## Inputs

- Section-level metadata from Module 01:
  - whether a section is present, empty, or `< 50` words.
- Per-section text and intermediate summaries from Module 02.
- `evidence_mode`: string describing how strict the evidence policy should be:
  - `"strict"` → only include claims explicitly supported by the text.
  - `"default"` (or any non-strict value) → standard summarization behavior while still avoiding obvious hallucinations.

## Outputs

- Validated or adjusted summaries that:
  - Contain only text-supported claims in `"strict"` mode.
  - Include clear warnings when evidence is insufficient.
- Standardized warning messages for:
  - Missing or empty sections.
  - Very short sections (`< 50` words).
- Flags or notes that downstream modules (e.g., rendering) can display in the Checks & Warnings section.

## Guardrail Behaviors

### 1. Strict Evidence Mode

When `evidence_mode = "strict"`:

- Summaries must:
  - Only include claims, equations, and results that clearly appear in the section text.
  - Avoid inferring causes, implications, or interpretations that are not explicitly stated.
- If the system cannot find enough information to safely summarize a section, it must output a standardized message, such as:
  - “Strict evidence mode: the source text does not provide enough detail to summarize this section confidently.”
- This module should instruct Module 02 to:
  - Prefer quoting or closely paraphrasing key phrases instead of abstract interpretations.
  - Avoid filling gaps from outside general knowledge.

If `evidence_mode` is not `"strict"`, the module applies normal evidence checks (no obvious hallucinations, no invented citations), but allows slightly more abstraction while still staying faithful to the paper.

### 2. Missing or Empty Sections

For any section flagged as missing or with no usable text:

- Do **not** generate a fabricated summary.
- Instead, attach a standardized warning message, e.g.:
  - “Section skipped: no usable text was provided.”
- This message should be:
  - Added to the section’s output container.
  - Also propagated to the global Checks & Warnings block in the final rendering module.

### 3. Very Short Sections (< 50 Words)

For any section where `word_count < 50`:

- Allow Module 02 to generate a summary based strictly on the available text.
- Attach a standardized warning, such as:
  - “Section very short: summary may be incomplete.”
- The warning should:
  - Make it clear to the user that coverage may be partial.
  - Also appear in the global Checks & Warnings summary.

### 4. Citation and Content Validation

- Ensure the summarizer does not create new citations, references, or authors.
- If citations are referenced by Module 05 (Citation Extractor), this module:
  - Confirms that cited items appear in the original text.
  - Flags any malformed or incomplete citations for the Checks & Warnings section.

### 5. Interaction With Other Modules

- **Module 01 (Intake & Setup)**: Provides flags for missing, empty, and short sections.
- **Module 02 (Section Loop)**: Uses `evidence_mode` and the standardized warnings defined here when generating section-level summaries.
- **Module 04 (Rendering & Refinement)**: Displays warnings and strict-evidence messages clearly in the final output under Checks & Warnings, so the user understands why some sections are skipped or cautiously summarized.

Module 3 — Guardrails
Short Name: Validation Layer Description: This module applies quality control checks to prevent hallucinations and structural errors. It validates that all summaries are traceable to the paper, applies chunking for long documents, and ensures citations are only referenced if present.

Inputs:

Section summaries

Raw paper text

Outputs:

Verified summaries

Structural validation report

Key Constraints/Checks:

Hallucination detection (traceability enforced)

Chunking for long papers

Validate citations only if present

Detect misordered or missing sections
