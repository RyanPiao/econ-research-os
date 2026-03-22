## Git Workflow for Book Manuscripts
```bash
# Branch strategy
main        → stable, compiles cleanly
develop     → active writing
ch03-did    → feature branch per chapter
review-ch03 → review/revision branch

# Commit conventions
git commit -m "Ch 4: First draft of DiD identification section"
git commit -m "Ch 4: Add simulation code for parallel trends"
git commit -m "Ch 4: Address self-review issues (voice, eq walks)"
git commit -m "Notation: Add tau_ATT definition"
git commit -m "Bib: Add Callaway-SantAnna 2021"

# Tag milestones
git tag -a v0.1-outline -m "Complete outline"
git tag -a v0.5-first-draft -m "All chapters first draft"
git tag -a v1.0-review-ready -m "Full manuscript for review"

# .gitignore for book projects
_book/
*.pdf
*.docx
*.html
*_files/
.quarto/
```
