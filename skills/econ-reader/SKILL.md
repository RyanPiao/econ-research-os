---
name: econ-reader
description: Read economics papers from PDFs/URLs, chunk long documents, extract knowledge into paper snapshots. Activate for /read-paper, /split-pdf, /handbook-reader, /corpus-qa.
---

# Economics paper reading and knowledge extraction

Routes:
- `/read-paper [path|URL]` → Read `read-paper.md`. Three-pass analysis. Output paper snapshot to `./literature/[AuthorYear-Title].md`. Append to `references.bib`.
- `/split-pdf [path]` → Chunk PDF into sections (10-15 pages each). Read each chunk sequentially. Consolidate into single snapshot. Prevents context window crashes on long documents.
- `/handbook-reader [path]` → Extension of split-pdf for textbook/handbook chapters. Preserves section hierarchy. Extracts definitions and key results.
- `/corpus-qa [question]` → Search across all paper snapshots in `./literature/`, synthesize answers with citations to specific files.

Flags: `--brief` for 1-page snapshot. `--deep` for full mastery pass with reconstruction.
Output template in `templates/paper-snapshot.md`.
