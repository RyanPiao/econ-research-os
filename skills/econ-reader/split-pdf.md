# Split-PDF and Handbook Reader Reference

## `/split-pdf [path]`

Chunk PDF into sections (10-15 pages each). Read each chunk sequentially. Consolidate into single snapshot. Prevents context window crashes on long documents.

### When to Use
- Papers or documents longer than ~30 pages
- Any PDF that risks exceeding the context window in a single pass
- Government reports, working papers with long appendices

### Chunking Workflow

1. **Input Resolution**: Verify the file exists. Extract text content.
   - Use `pdftotext` (poppler-utils) or PyPDF2 as fallback:
   ```bash
   # Try pdftotext first (poppler-utils)
   pdftotext "[input_path]" /tmp/paper_text.txt

   # If unavailable, try python
   python3 -c "
   import sys
   try:
       import PyPDF2
       reader = PyPDF2.PdfReader(sys.argv[1])
       text = '\n'.join(page.extract_text() or '' for page in reader.pages)
       print(text)
   except ImportError:
       print('ERROR: No PDF extraction tool available.')
   " "[input_path]" > /tmp/paper_text.txt
   ```
2. **Determine page count** and divide into chunks of 10-15 pages each.
3. **Process each chunk sequentially** using the three-pass reading strategy from `read-paper.md`.
4. **Consolidate** all chunk outputs into a single paper snapshot using the template in `templates/paper-snapshot.md`.
5. **Save** the consolidated snapshot to `./literature/[AuthorYear-ShortTitle].md`.

---

## `/handbook-reader [path]`

Extension of split-pdf for textbook/handbook chapters. Preserves section hierarchy. Extracts definitions and key results.

### How It Extends Split-PDF
- **Preserves section hierarchy**: Maintains chapter/section/subsection structure during chunking rather than splitting by raw page count.
- **Extracts definitions and key results**: Identifies and highlights formal definitions, theorems, propositions, and key results from each section.
- **Suitable for**: Handbook of Labor Economics chapters, JEL survey articles, textbook chapters (e.g., Mas-Colell, Wooldridge).

### Workflow Differences from Split-PDF
1. **Section-aware chunking**: Instead of splitting at fixed page intervals, split at section or subsection boundaries to keep logical units intact.
2. **Hierarchy preservation**: Maintain the heading structure (Chapter → Section → Subsection) in the consolidated output.
3. **Definition/result extraction**: For each section, explicitly extract:
   - Formal definitions
   - Key theorems or propositions
   - Main empirical results or stylized facts
4. **Consolidation**: Merge into a single snapshot that preserves the original document's organizational structure.
