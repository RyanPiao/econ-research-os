# /status — Project Dashboard

## Procedure

1. **Identify project root**: Look for `_quarto.yml`, `CLAUDE.md`, or `outline.md` in current directory or parent directories.

2. **Scan and report each area**:

### Chapter progress
```bash
# Find all chapter files
for f in chapters/*.qmd; do
  title=$(grep -m1 "^title:" "$f" | sed 's/title: *//' | tr -d '"')
  status=$(grep -m1 "^status:" "$f" | sed 's/status: *//')
  words=$(wc -w < "$f" | tr -d ' ')
  echo "| $(basename $f) | $title | $status | ${words}w |"
done
```

Display as:
```markdown
### Chapters
| File | Title | Status | Words | Reviewed | Verified | Compiled |
|------|-------|--------|-------|----------|----------|----------|
```

Status badges: first-draft → reviewed → verified → compiled. Check for existence of corresponding review files in `./reviews/`.

### Knowledge base stats
```bash
total=$(ls literature/*.md 2>/dev/null | grep -v _index | wc -l)
detailed=$(grep -rl "status: detailed" literature/ 2>/dev/null | wc -l)
skim=$(grep -rl "status: skim" literature/ 2>/dev/null | wc -l)
notes=$(ls notes/*.md 2>/dev/null | wc -l)
bib_entries=$(grep -c "^@" references.bib 2>/dev/null || echo 0)
```

Display:
```markdown
### Knowledge base
- Papers read (detailed): [N]
- Papers skimmed: [N]
- Papers queued: [N] (from reading-list.md minus literature/)
- Atomic notes: [N]
- Bibliography entries: [N]
```

### Stale output detection
```bash
# For each Python script, find its output files and compare timestamps
for script in code/*.py; do
  # Extract output paths from the script (grep for common patterns)
  outputs=$(grep -oP '(output|figures|tables)/[^"'\'')\s]+' "$script" 2>/dev/null)
  for out in $outputs; do
    if [ -f "$out" ] && [ "$script" -nt "$out" ]; then
      echo "STALE: $script ($(date -r $script +%Y-%m-%d)) → $out ($(date -r $out +%Y-%m-%d))"
    fi
  done
done
```

### Bibliography health
```bash
# Citations used in chapters but missing from .bib
grep -rohP '@[\w]+' chapters/*.qmd 2>/dev/null | sort -u > /tmp/cited.txt
grep -oP '(?<=\{)\w+(?=,)' references.bib 2>/dev/null | sort -u > /tmp/inbib.txt
missing=$(comm -23 /tmp/cited.txt /tmp/inbib.txt)
if [ -n "$missing" ]; then
  echo "MISSING from .bib: $missing"
fi
```

### Next actions (auto-inferred)
Priority logic:
1. Stale outputs → "Re-run [script] to update output"
2. Missing bib entries → "Run /bib check to fix citations"
3. Chapter with "first-draft" status → "Run /self-review on [chapter]"
4. Chapter with "reviewed" but not "verified" → "Run /verify on [chapter]"
5. Queued papers in reading list → "Next paper to read: [title]"
6. All chapters verified → "Run /compile-draft for full manuscript"

### Output format
```markdown
## Project Status: [Project Name]
**Updated**: [YYYY-MM-DD HH:MM]

### Chapters
[table]

### Knowledge base
[stats]

### Alerts
- ⚠️ [Stale outputs]
- ⚠️ [Missing citations]
- ⚠️ [Unreviewed chapters]

### Next actions
1. [Highest priority action]
2. [Second action]
3. [Third action]
```

3. **Save**: Overwrite `./STATUS.md` each time. Also print to console.
