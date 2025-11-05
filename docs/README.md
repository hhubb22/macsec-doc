# MACsec Standard Documentation Cleanup

This workspace contains an OCR-derived copy of IEEE Std 802.1AE-2018 that we are progressively organizing into a clean set of Markdown references.

## Cleanup Goals

- Preserve the structure of the published standard (front matter, clauses, annexes).
- Normalize headings, numbering, and lists so the text renders well in Markdown.
- Split the monolithic OCR export into smaller files that are easier to navigate and review.
- Track open cleanup tasks and unresolved OCR artifacts for future passes.

## Target Layout

```
docs/
  README.md                # This file
  front-matter.md          # Title page, notices, introduction
  clauses/
    01-overview.md         # Clause 1
    02-normative-references.md
    ...
    16-using-mib-modules.md
  annexes/
    annex-a-pics-proforma.md
    annex-b-bibliography.md
    ...
  references/
    tables.md              # (Optional) consolidated tables list
    figures.md             # (Optional) consolidated figures list
```

As we continue organizing the material, the placeholder entries above will be replaced with fully cleaned content.

## Immediate Priorities

1. Extract front matter (title pages, notices, introduction) into `front-matter.md` and fix obvious OCR issues.
2. Normalize headings and lists for Clause 1 and Clause 2, saving them under `docs/clauses/`.
3. Record outstanding anomalies (mis-numbered annex entries, duplicated table of contents, etc.) for future cleanup.
4. Once the early clauses are stable, repeat the pattern for the remaining clauses and annexes.

## Notes

- The source OCR file is `ieee-standard-for-local-and-metropolitan-area-networksmedia-acce_MinerU__20251104084531.md`. It remains untouched during the restructure for traceability.
- Footnotes in cleaned clauses now use Markdown footnotes. Remaining source material will be converted as it is curated.
- Many tables and figures appear twice in the OCR export. We plan to reconcile duplicates during section-by-section cleanup.
