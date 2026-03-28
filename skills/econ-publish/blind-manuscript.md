# /blind-manuscript — Create anonymized version for double-blind review

## Trigger
"blind the manuscript", "create anonymized version", "remove author info for review"

## What to remove

### Title page
- Author name(s)
- Affiliation(s)
- Email address(es)
- Date (optional — some journals keep it)
- Acknowledgements mentioning colleagues or institutions
- "Corresponding author" footnote

### Manuscript body — check for:
- [ ] Self-citations: "Piao (2023)" → "Author (2023)" or "[redacted]"
- [ ] University name: "Northeastern University" → "[University]"
- [ ] Lab or center names: "[Author's] Research Lab" → "[redacted]"
- [ ] GitHub URLs with author name: "github.com/RyanPiao/..." → "[repository URL redacted for review]"
- [ ] Acknowledgements section: remove entirely or replace names with "[redacted]"
- [ ] "In our previous work, we showed..." → "Previous work has shown..."
- [ ] Conference presentations: "presented at [University] seminar" → remove
- [ ] Funding statements mentioning PI name

### How to create (Quarto projects)

```bash
# 1. Backup original config
cp _quarto.yml _quarto.yml.bak

# 2. Remove author from Quarto config
sed '/  author:/d' _quarto.yml.bak > _quarto.yml
# Or for multi-line author blocks:
# Manually edit to remove author: section

# 3. Compile blinded PDF
quarto render --to pdf

# 4. Save blinded version
cp _book/[output].pdf submission/[journal]/manuscript-blinded.pdf

# 5. Restore original
cp _quarto.yml.bak _quarto.yml
rm _quarto.yml.bak

# 6. Recompile with author (for the non-blinded version)
quarto render --to pdf
```

### For standalone .qmd files (not book projects)
Remove author info from the YAML frontmatter:
```yaml
# BEFORE (identified)
author:
  - name: "Richeng Piao"
    affiliation: "Northeastern University"
    email: "r.piao@northeastern.edu"

# AFTER (blinded)
# (remove entire author block)
```

## Verification checklist
- [ ] Title page has no author name
- [ ] Title page has no affiliation
- [ ] Title page has no email
- [ ] Body text has no self-citations by name
- [ ] No GitHub URLs with author username
- [ ] No acknowledgements with identifying names
- [ ] No "at [University]" references
- [ ] PDF metadata doesn't contain author name (check File > Properties in PDF reader)
