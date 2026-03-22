# /context-save — Persist Session State to MEMORY.md

## Trigger
`/context-save` — run at the end of every working session

## Procedure

### Step 1: Summarize current session
Reflect on what was done in this conversation:
- What tasks were completed?
- What decisions were made (and why)?
- What issues were encountered?
- What files were created or modified?

### Step 2: Determine next actions
Based on the session's work, what should be done next?
- What's the logical next step in the pipeline?
- Are there any blockers or open questions?
- What should the next session start with?

### Step 3: Update MEMORY.md
Read existing `MEMORY.md`. Prepend the new session entry above the "Session history" line.

Format:
```markdown
## Last session
**Date**: [YYYY-MM-DD]
**Duration**: ~[estimate] hours
**Focus**: [Brief description — e.g., "Reading DiD papers + drafting lit review"]

### What was accomplished
- [Task 1 with specific outcome]
- [Task 2 with specific outcome]

### Decisions made
- [Decision + rationale]

### Files created/modified
- [file path] — [what was done]

### Issues encountered
- [Issue + resolution or "left open"]

### Next actions (start here next session)
1. [ ] [Most important — be specific]
2. [ ] [Second priority]
3. [ ] [Third priority]

### Open questions
- [Anything that needs resolution]
```

### Step 4: Update CLAUDE.md
Update the "Current state" section of CLAUDE.md:
- Active chapter
- Current phase
- Last session date and description

### Step 5: Stage for commit
```bash
git add MEMORY.md CLAUDE.md STATUS.md
echo "Session state saved. Run 'git commit -m \"Session: [brief description]\"' to persist."
```

### Step 6: Recommend session hygiene
If the session was long (many tasks), recommend:
- "Consider running /status to see the full project state"
- "The next session should start with: 'Read MEMORY.md and tell me where I left off'"
