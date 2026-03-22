# /bib — Bibliography Management

## /bib add [input]

### Accepted inputs
- **DOI**: `10.1257/aer.20161385` → fetch from Crossref
- **URL**: `https://www.nber.org/papers/w28726` → extract DOI, then fetch
- **Title**: `"Difference-in-Differences with Multiple Time Periods"` → search Semantic Scholar
- **From snapshot**: `@CallawayaSantAnna2021` → read from `./literature/` YAML frontmatter

### Procedure
1. **Resolve metadata**:
   ```bash
   # If DOI provided, fetch from Crossref
   curl -s "https://api.crossref.org/works/[DOI]" | python3 -c "
   import sys, json
   data = json.load(sys.stdin)['message']
   authors = ' and '.join([f\"{a['family']}, {a.get('given','')}\" for a in data['author']])
   print(f\"@article{{{data['author'][0]['family']}{data['published-print']['date-parts'][0][0]},\")
   print(f\"  author = {{{authors}}},\")
   print(f\"  title = {{{data['title'][0]}}},\")
   print(f\"  journal = {{{data['container-title'][0]}}},\")
   print(f\"  year = {{{data['published-print']['date-parts'][0][0]}}},\")
   print(f\"  doi = {{{data['DOI']}}}\")
   print('}')
   "
   ```

2. **Check for duplicates**: Search `references.bib` by DOI match or fuzzy title match (>80% similarity).

3. **Generate citekey**: `AuthorYear` format. If conflict, append `a`, `b`, `c`.

4. **Determine entry type**:
   - Published journal article → `@article`
   - NBER/IZA/CEPR working paper → `@techreport` with `institution` and `number` fields
   - Book → `@book`
   - Book chapter → `@incollection`
   - Unpublished/preprint → `@unpublished`

5. **Append to references.bib**. Sort entries alphabetically by citekey.

6. **Confirm**: "Added @CiteKey to references.bib ([N] total entries)"

## /bib check [chapter-file]

### Procedure
1. Extract all `[@citekey]` patterns from the specified file
2. For each citekey:
   - Does it exist in `references.bib`? → PASS or MISSING
   - Is the entry complete? Check for: author, title, year, journal/series
   - Is the entry a working paper that might now be published?
3. Find entries in `references.bib` NOT cited in any chapter → report as "unused"

### Output
```markdown
### Bibliography check: [filename]
**Cited keys**: [N]
**All resolved**: [Yes/No]

| Citekey | Status | Issue |
|---------|--------|-------|
| @Card1994 | ✅ OK | |
| @Smith2023 | ❌ MISSING | Not in references.bib |
| @Jones2020 | ⚠️ INCOMPLETE | Missing journal field |
| @Brown2019wp | ⚠️ STALE | Working paper — check for published version |
```

## /bib dedup

### Procedure
1. Parse all entries in `references.bib`
2. Group by:
   - Exact DOI match
   - Title similarity > 85% (case-insensitive, stripped of punctuation)
   - Same author(s) + same year + similar title
3. For each duplicate group:
   - Identify the more complete entry (more fields filled)
   - Report both entries with line numbers
   - Offer to merge: keep the complete entry, update the citekey if needed, and find-replace the old citekey across all chapter files

### Output
```markdown
### Duplicate entries found: [N groups]

| Group | Entry 1 | Entry 2 | Keep |
|-------|---------|---------|------|
| 1 | @Card1994 (line 45, complete) | @CardKrueger94 (line 203, partial) | @Card1994 |
```

## /bib update-wp

### Procedure
1. Find all entries with type `@techreport` or `@unpublished` in `references.bib`
2. For each:
   - Search Semantic Scholar by title
   - Check if the paper now has a `journal` or `venue` field indicating publication
3. Report updates available

### Output
```markdown
### Working papers with published versions available: [N]

| Citekey | Original series | Now published in | Year |
|---------|----------------|------------------|------|
| @Callaway2020wp | NBER WP 28726 | Journal of Econometrics | 2021 |

Action: Run `/bib update-wp --apply` to update entries and change types to @article.
```
