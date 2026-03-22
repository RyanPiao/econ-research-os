# /new-chapter — Scaffold a New Book Chapter

## Trigger
`/new-chapter [N] "[Title]"` — e.g., `/new-chapter 5 "Difference-in-Differences"`

## Procedure

### Step 1: Create the chapter file from template
```bash
# Generate filename
FILENAME="chapters/$(printf '%02d' $N)-$(echo "$TITLE" | tr ' ' '-' | tr '[:upper:]' '[:lower:]').qmd"

# Copy and customize template
cp templates/chapter-template.qmd "$FILENAME"
sed -i "s/\[N\]/$N/g; s/\[Chapter Title\]/$TITLE/g; s/\[YYYY-MM-DD\]/$(date +%Y-%m-%d)/g" "$FILENAME"
```

### Step 2: Create supporting directories
```bash
mkdir -p "code" "output/tables" "output/figures"
```

### Step 3: Update _quarto.yml
Add the new chapter to the chapters list in `_quarto.yml` in the correct position.

### Step 4: Update outline.md
Add entry for Chapter [N] with title and status "not started".

### Step 5: Update NOTATION.md
Prompt: "Will this chapter introduce any new notation? If so, I'll add it to NOTATION.md."

### Step 6: Confirm
```
Chapter [N]: [Title] scaffolded:
- [filename] created from template
- Added to _quarto.yml
- Added to outline.md

Ready to draft. Use:
  /book-chapter [N] "[Title]"  — to draft the full chapter
  /draft-section "[section topic]"  — to draft one section at a time
```
