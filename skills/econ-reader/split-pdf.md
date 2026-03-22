# Split-PDF & Handbook Reader

## `/split-pdf [path]`
Chunk PDF into sections (10-15 pages each). Read each chunk sequentially. Consolidate into single snapshot. Prevents context window crashes on long documents.

## `/handbook-reader [path]`
Extension of split-pdf for textbook/handbook chapters. Preserves section hierarchy. Extracts definitions and key results.

---

## PDF Text Extraction (Step 0: Input Resolution)

For text extraction from PDFs, attempt:
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

If extraction fails, inform the user:
> "I couldn't extract text from this PDF. Please install `poppler-utils` (`brew install poppler` or `sudo apt install poppler-utils`) or provide a plain-text version."

## Input Resolution

1. **If local PDF path**: Verify the file exists. Extract text content.
2. **If web URL**: Use `curl -L -o /tmp/paper_download.pdf "[URL]"` to download. Then extract text.
3. **If directory path**: List all `.pdf` files with `ls [path]/*.pdf`. Process each sequentially. Generate one output per paper plus a batch index.
