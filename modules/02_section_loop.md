Module 2 — Section Loop
Short Name: Section Summarizer Description: This module iterates through each section of the paper, generating a concise summary between 50–100 words. It enforces the no-added-information rule and ensures each summary captures the section’s purpose, key ideas, and findings. Warnings are logged if constraints are violated.

Inputs:

Individual section text

Metadata (section title)

Outputs:

Section summary (50–100 words)

Warning logs for constraint violations

Key Constraints/Checks:

Section title precedes summary

Word count ≤100 words

No external information introduced
