### /reading-list — Manage and prioritize your reading queue

**Trigger**: "show my reading list", "what should I read next", "update reading list", "prioritize my papers"

**Workflow**:
1. Scan `./literature/` for existing paper snapshots (check YAML `status` field)
2. Scan search results and citation chase files for recommended papers
3. Cross-reference to identify:
   - Papers recommended multiple times (high priority)
   - Papers already read (mark complete)
   - Papers in queue but not started

4. Generate prioritized reading list:

```markdown
## Reading List: [Project Name]
**Updated**: [YYYY-MM-DD]

### Status summary
- Completed: [N] papers
- In progress: [N] papers
- Queued: [N] papers
- Total unique papers identified: [N]

### Priority queue (read next)
| Priority | Authors (Year) | Title | Why | Source | Status |
|----------|---------------|-------|-----|--------|--------|
| 1 | | | Cited by 3 searches | /deep-search | queued |
| 2 | | | Closest comparison | /citation-chase | queued |

### Completed reads
| Authors (Year) | Title | Snapshot file | Date read |
|---------------|-------|--------------|-----------|

### Parked (lower priority)
| Authors (Year) | Title | Reason parked |
|---------------|-------|--------------|

### Stop rule check
[Have the last 5 reads produced new methods, mechanisms, or disputes?
If no → you may have saturated this area. Consider moving to writing.]
```

5. Save to `./projects/[project-name]/reading-list.md`