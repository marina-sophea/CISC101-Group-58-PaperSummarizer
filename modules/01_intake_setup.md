Module 1 — Intake & Setup
Short Name: Intake Description: This module initializes the summarization process by loading the academic paper, its section metadata, and the target audience level. It normalizes the section list to ensure order consistency and checks for missing or incomplete sections. All PS2 constraints are applied at this stage to guarantee compliance before processing begins.

Inputs:

Full paper text split into labeled sections

Section metadata (titles + order)

Target audience level

Outputs:

Clean structured input package

Validation report of missing/empty sections

Key Constraints/Checks:

Section order must match metadata

Detect sections <50 words or empty

Apply PS2 rules: neutral tone, no external info, technical terms preserved
