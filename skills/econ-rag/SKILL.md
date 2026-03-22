## ============================================================
## FILE: skills/econ-rag/SKILL.md
## ============================================================
---
name: econ-rag
description: RAG (Retrieval-Augmented Generation) integration for the economics knowledge base. Auto-indexes paper snapshots into vector database, enables semantic search across all reading notes, and powers /corpus-qa. Activate for /index, /corpus-qa, /search-kb, /rag-status. Requires knowledge-rag MCP server.
---

# Economics Knowledge Base RAG Layer

This skill connects your `./literature/` and `./notes/` folders to a local vector database via the knowledge-rag MCP server. It enables semantic search across all your reading notes instead of loading files sequentially.

**Requires**: knowledge-rag MCP server installed and running. See `setup.md` for installation.

Routes:
- `/index [file|folder]` → Read `index.md`. Add paper snapshots to the RAG database. Auto-runs after /read-paper.
- `/corpus-qa [question]` → Read `corpus-qa.md`. Semantic search across knowledge base, answer with citations to specific papers.
- `/search-kb [query]` → Quick semantic search. Returns top results without generating a full answer. Good for "do I already have a paper on X?"
- `/rag-status` → Show indexing stats: how many documents, last indexed, any unindexed files.

Flags: `--reindex` to rebuild the entire database. `--collection [name]` to search a specific project.


## ============================================================
## FILE: skills/econ-rag/setup.md
## ============================================================

# RAG Setup Guide for macOS

## Option A: Symlink your knowledge base (recommended)

```bash
# 1. Make sure your knowledge base folders exist
mkdir -p ~/econ-knowledge-base/literature
mkdir -p ~/econ-knowledge-base/notes

# 2. Navigate to knowledge-rag
cd /path/to/knowledge-rag

# 3. Create symlinks using ABSOLUTE paths
# The $HOME variable ensures the path is fully resolved
ln -sf "$HOME/econ-knowledge-base/literature" ./documents/literature
ln -sf "$HOME/econ-knowledge-base/notes" ./documents/notes

# 4. Verify the links work
ls ./documents/literature/
ls ./documents/notes/
```

**If symlinks fail**, check:
- Source folder must exist BEFORE creating the symlink
- Use absolute paths (not ~/), or use "$HOME" which expands correctly
- The target (./documents/literature) must NOT already exist as a real folder — delete it first if it does: `rm -rf ./documents/literature`

## Option B: Edit knowledge-rag config (no symlinks needed)

If symlinks give you trouble, edit `knowledge-rag/mcp_server/config.py` and change the documents directory to point directly at your knowledge base:

```python
# In config.py, find the DOCUMENTS_DIR setting and change it:
DOCUMENTS_DIR = os.path.expanduser("~/econ-knowledge-base")
```

This makes knowledge-rag index everything under `~/econ-knowledge-base/` directly — your `literature/`, `notes/`, `topics/`, and `fleeting/` folders.

## Option C: Multiple project support

If you have multiple projects each with their own `literature/` folder, create a master symlink directory:

```bash
mkdir -p /path/to/knowledge-rag/documents

# Link each project's literature
ln -sf "$HOME/project-a/literature" ./documents/project-a
ln -sf "$HOME/project-b/literature" ./documents/project-b
ln -sf "$HOME/econ-knowledge-base/literature" ./documents/main-kb
```

Knowledge-rag treats each subfolder as a separate collection, so you can search within one project or across all of them.

## MCP Server Configuration

After setting up the document directory, add knowledge-rag to Claude Code:

```bash
# Add to Claude Code (user-level, available in all projects)
claude mcp add knowledge-rag -s user -- \
  /path/to/knowledge-rag/venv/bin/python -m mcp_server.server
```

Or manually add to your Claude Code MCP config (`~/.claude.json`):

```json
{
  "mcpServers": {
    "knowledge-rag": {
      "type": "stdio",
      "command": "/path/to/knowledge-rag/venv/bin/python",
      "args": ["-m", "mcp_server.server"],
      "cwd": "/path/to/knowledge-rag"
    }
  }
}
```

## Verify it works

In Claude Code:
```
> Search my knowledge base for "difference-in-differences"
```

If knowledge-rag is running, Claude will use the `search_documents` MCP tool. If you see results from your paper snapshots, it's working.


## ============================================================
## FILE: skills/econ-rag/index.md
## ============================================================

# /index — Add Documents to RAG Database

## When this runs

**Automatically** after every `/read-paper` completion:
After generating a paper snapshot in `./literature/`, the system should immediately index it.

**Manually** when you want to:
- Index an entire folder: `/index ./literature/`
- Re-index everything after changes: `/index --reindex`
- Index notes: `/index ./notes/`

## Procedure

### Auto-index after /read-paper

When a paper snapshot is saved to `./literature/[AuthorYear-Title].md`:

1. Use the knowledge-rag MCP `add_document` tool:
```
Use the knowledge-rag add_document tool to index the file at
./literature/[AuthorYear-Title].md
```

2. Verify indexing:
```
Use the knowledge-rag search_documents tool to search for
"[paper title keyword]" and confirm the new paper appears.
```

### Batch index a folder

When the user runs `/index ./literature/`:

1. List all `.md` files in the folder
2. For each file, use the knowledge-rag `add_document` tool
3. Report progress: "Indexed [N] of [M] files"
4. Report any failures

### Full reindex

When the user runs `/index --reindex`:

1. Use the knowledge-rag `reindex_documents` tool with `force=true`
2. This re-reads and re-embeds all documents — needed when:
   - You've edited existing paper snapshots
   - The embedding model changed
   - You suspect the index is stale

## What gets indexed

| Folder | What's in it | Chunk strategy |
|--------|-------------|----------------|
| `literature/` | Paper snapshots with YAML + sections | Split by ## headers → each section is a chunk |
| `notes/` | Atomic permanent notes (your ideas) | Each note is usually one chunk |
| `topics/` | Hub/structure notes by theme | Split by ## headers |
| `fleeting/` | Quick captures, brainstorms | Each file is one chunk |

The markdown-aware chunker in knowledge-rag splits on `##` and `###` headers, so each section of a paper snapshot (Executive Summary, Identification, Key Result, etc.) becomes its own searchable chunk. This means a search for "parallel trends assumption" will find the specific identification section of the relevant paper, not the whole 3-page snapshot.

## YAML frontmatter as metadata

Knowledge-rag stores metadata with each chunk. The YAML frontmatter in your paper snapshots provides:
- `citekey` → used to identify which paper a chunk comes from
- `tags` → used for filtered searches ("find all DiD papers")
- `methodology` → used for method-specific searches
- `relevance` → used to boost high-relevance papers in results
- `year` → used for recency-weighted searches


## ============================================================
## FILE: skills/econ-rag/corpus-qa.md
## ============================================================

# /corpus-qa — Ask Questions Across Your Knowledge Base

## Trigger
`/corpus-qa "What do papers say about [topic]?"` or any question that requires searching across multiple paper snapshots.

## Procedure

### Step 1: Semantic search
Use the knowledge-rag `search_documents` MCP tool with the user's question:

```
Search the knowledge base for: "[user's question]"
```

The MCP tool returns the top matching chunks with:
- Text content
- Source file
- Relevance score
- Metadata (citekey, tags, etc.)

### Step 2: Assess coverage
From the results:
- How many distinct papers are represented?
- Are the results from the right topic area?
- Are any obviously relevant papers missing? (If so, run a second search with different keywords)

**Agentic search pattern**: If the first search doesn't cover the topic well, run 2-3 additional searches with keyword variations. For example:
- First search: "minimum wage employment effects"
- Second search: "monopsony labor market"
- Third search: "wage floor teen employment"

This mimics how a researcher does multiple Google Scholar searches to triangulate.

### Step 3: Synthesize an answer
From the retrieved chunks, compose an answer that:
- Cites specific papers: "According to @Card1994, [finding]..."
- Notes points of agreement and disagreement
- Identifies if the knowledge base has gaps on this question
- Uses only information actually present in the retrieved chunks — do NOT hallucinate beyond what's in the knowledge base

### Step 4: Output format

```markdown
## Answer: [Question]

[Synthesized answer with citations to specific paper snapshots]

### Sources used
| Paper | Section matched | Relevance |
|-------|----------------|-----------|
| @Card1994 | Identification Strategy | 0.92 |
| @CallawaySantAnna2021 | Key Result | 0.87 |
| @GoodmanBacon2021 | Executive Summary | 0.84 |

### Knowledge base gaps
[If the question can't be fully answered from existing snapshots,
note what's missing: "No papers in the knowledge base cover [X].
Consider running /paper-search [X] to find relevant literature."]
```

## Difference from /read-paper and /gap-finder

| Skill | What it does | Uses RAG? |
|-------|-------------|-----------|
| `/read-paper` | Reads ONE paper deeply, generates snapshot | No (reads the PDF directly) |
| `/corpus-qa` | Answers questions across ALL papers | **Yes** — searches vector database |
| `/gap-finder` | Finds research gaps across ALL papers | **Yes** — searches for gap-related sections |
| `/lit-review` | Drafts lit review from relevant papers | **Yes** — finds relevant papers first, then reads full snapshots |


## ============================================================
## FILE: skills/econ-rag/search-kb.md
## ============================================================

# /search-kb — Quick Semantic Search of Knowledge Base

## Trigger
`/search-kb "parallel trends"` or "do I already have a paper on [topic]?"

## Procedure

1. Use the knowledge-rag `search_documents` MCP tool with the query
2. Display results as a compact table:

```markdown
### Knowledge base search: "[query]"
**Results**: [N] chunks from [M] papers

| # | Paper | Section | Relevance | Preview |
|---|-------|---------|-----------|---------|
| 1 | @Card1994 | Identification | 0.94 | "DiD comparing NJ and PA..." |
| 2 | @CallawaySantAnna2021 | Key Result | 0.88 | "Group-time ATTs..." |
| 3 | @GoodmanBacon2021 | Executive Summary | 0.82 | "TWFE decomposes into..." |
```

3. If the user was checking whether they already have a paper: answer "Yes, you have [N] papers on this topic" or "No papers found — consider running /paper-search [topic]"

This is the lightweight version of /corpus-qa — it returns search results without synthesizing an answer. Use it for quick lookups.


## ============================================================
## FILE: skills/econ-rag/rag-status.md
## ============================================================

# /rag-status — RAG Database Status

## Procedure

1. Use the knowledge-rag `get_stats` MCP tool (or equivalent)
2. List all `.md` files in `./literature/` and `./notes/`
3. Compare against what's indexed in the database
4. Report:

```markdown
### RAG Status
**Database**: knowledge-rag (ChromaDB)
**Total indexed chunks**: [N]
**Total indexed documents**: [N]
**Last indexing**: [timestamp]

### Coverage
| Folder | Files on disk | Files indexed | Unindexed |
|--------|--------------|---------------|-----------|
| literature/ | 34 | 31 | 3 (new) |
| notes/ | 67 | 67 | 0 |
| topics/ | 8 | 8 | 0 |

### Unindexed files (need /index)
- literature/NewPaper2026-Title.md (created today)
- literature/AnotherPaper2025-Title.md (created today)
- literature/ThirdPaper2024-Title.md (modified yesterday)

### Action
Run `/index ./literature/` to index 3 new files.
```


## ============================================================
## How other skills use RAG (integration points)
## ============================================================

These existing skills should check for the knowledge-rag MCP server
and use it when available. If the MCP server is not connected,
they fall back to the old behavior (reading files sequentially).

### /corpus-qa (econ-reader)
OLD: Read all files in ./literature/ sequentially
NEW: Use knowledge-rag search_documents → retrieve top 10 chunks → synthesize

### /gap-finder (econ-synthesize)
OLD: Scan all paper snapshots linearly, read Gap Analysis sections
NEW: Search knowledge-rag for "gap", "limitation", "future work" → aggregate

### /lit-review (econ-synthesize)
OLD: Read all tagged snapshots
NEW: Search knowledge-rag by topic → get top 15 relevant papers → read full snapshots only for those

### /paper-search (econ-discover)
OLD: Only searches external databases (Semantic Scholar, NBER, etc.)
NEW: ALSO searches local knowledge base first: "Do I already have papers on this?"
     Shows local matches before external results.

### /reading-list (econ-discover)
OLD: Cross-references search results with file existence in ./literature/
NEW: Also queries knowledge-rag to check which papers are already indexed

### /verify Lens 4: Citations (econ-verify)
OLD: Grep references.bib for citekeys
NEW: Also search knowledge-rag for cited claims to verify characterization accuracy
```