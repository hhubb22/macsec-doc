# Cleanup TODOs

- Curate contributor, ballot, and participant listings from the OCR export and format them as appendices.
- Normalize tables of contents, list of figures, and list of tables; deduplicate repeated entries.
- Convert inline `<sup>` markers into Markdown footnotes in each clause. (Done 2025-11-04 — `docs/clauses/01-overview.md`, `docs/clauses/02-normative-references.md`, and `docs/clauses/03-definitions.md` now use Markdown footnotes instead of `<sup>` tags. Remaining clauses did not include `<sup>` markers.)
- Extract and clean Clauses 3–16, mirroring the approach used for Clauses 1 and 2.
- Cross-check Clause 3 references ([11], [B2]) against the final bibliography when Annex B is curated.
- Convert IEEE trademark markers (e.g., 8802.2™) in Clause 4 once a consistent style guide is agreed.
- Verify Clause 5 cross-references (e.g., Tables 10-3, clause citations) after corresponding sections are cleaned to ensure numbering remains accurate.
- Replace Clause 6 figure placeholder with a local asset or diagram once available.
- Add local diagrams or descriptive alternatives for Clause 7 figures (7-1 through 7-7) after sourcing clean artwork.
- Source or recreate MACsec diagrams for Clause 8 (Figure 8-1 and Figure 8-2) to replace placeholders.
- Process Annexes A–D and any additional annexes, ensuring numbering and cross-references remain accurate.
- Verify spellings and diacritical marks for participant names against authoritative sources.
- Reconcile any OCR transcription errors discovered during clause-by-clause review (e.g., SMIv2 vs. SNMPv2 typos).
- Recreate Clause 9 diagrams (Figures 9-1 to 9-4) or substitute descriptive graphics matching the SecTAG and MPDU layouts.
- Recreate Clause 10 figures (10-1 to 10-4) and transcribe Table 10-3 with validated performance bounds.

- Recreate Clause 11 figures (11-1 to 11-15); capture any system-specific tables if present.

- Convert Clause 13 tables and SMIv2 definitions into cleaned Markdown / code blocks; recreate Figure 13-1.

- Recreate Clause 12 Figure 12-1 (MACsec with EPON).
