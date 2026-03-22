# MCP Server Setup Guide

## What are MCP servers?

MCP (Model Context Protocol) servers extend Claude Code's capabilities by connecting it to external tools and data sources. In econ-research-os, MCP servers provide the bridge between Claude and academic databases, file systems, and version control. Without them, skills like `/paper-search` and `/citation-chase` would have no way to query Semantic Scholar or access files outside the working directory.

Each MCP server runs as a local process that Claude Code communicates with. You install them once at the user level, and they are available across all your projects.

## Semantic Scholar MCP

This is the most important MCP server for the system. It powers `/paper-search`, `/citation-chase`, and `/deep-search` by providing direct access to the Semantic Scholar academic database.

```bash
claude mcp add semantic-scholar -s user -- \
  uvx --from git+https://github.com/akapet00/semantic-scholar-mcp \
  semantic-scholar-mcp
```

**Notes:**
- Works without an API key for moderate usage (up to ~100 requests per 5 minutes).
- For heavier usage, request a free API key at [Semantic Scholar API](https://www.semanticscholar.org/product/api) and set the environment variable `S2_API_KEY`.
- Provides paper search, citation graph traversal, author lookup, and paper metadata.

## GitHub MCP

Enables Claude Code to interact with GitHub repositories: push code, create pull requests, and manage issues directly from the conversation.

```bash
claude mcp add github -s user -- \
  npx -y @modelcontextprotocol/server-github
```

**Notes:**
- Requires a GitHub personal access token. Set `GITHUB_PERSONAL_ACCESS_TOKEN` in your environment.
- Optional for the research workflow. Most useful if you collaborate via GitHub or want Claude to manage git operations beyond local commits.

## Filesystem MCP

Allows Claude Code to read and write files outside the current working directory. Useful when your papers, data, and project live in different locations.

```bash
claude mcp add filesystem -s user -- \
  npx -y @modelcontextprotocol/server-filesystem ~/projects ~/papers
```

**Notes:**
- The paths at the end (`~/projects ~/papers`) define which directories the server can access. Adjust these to match your setup.
- Optional but convenient if you keep a central `~/papers/` directory separate from your project.

## Knowledge-RAG MCP (Optional)

> **Optional, recommended when knowledge base exceeds 30 papers.**

When your `literature/` directory grows beyond ~30 paper snapshots, the context window becomes the bottleneck. A knowledge-RAG MCP server indexes your markdown knowledge base and lets Claude retrieve relevant snippets on demand rather than loading everything into context.

This is an area of active development. Future documentation will cover:
- Recommended RAG server implementations compatible with Claude Code
- Indexing configuration for markdown paper snapshots
- Query patterns that work well with the econ-research-os knowledge base structure
- Performance tuning for large literature collections (100+ papers)

For now, the system works well without RAG for projects with up to ~30 papers in the knowledge base.

## Troubleshooting

### MCP server not connecting

**Symptom:** Claude Code does not recognize MCP tools, or skills like `/paper-search` report that Semantic Scholar is unavailable.

**Steps to diagnose:**
1. Check that the server is registered: `claude mcp list`
2. Verify the server process is running. MCP servers start on demand, so try invoking a skill that uses it.
3. Check that `uvx` (for Semantic Scholar) or `npx` (for GitHub/Filesystem) is installed and on your PATH.
4. Remove and re-add the server: `claude mcp remove semantic-scholar` then re-run the add command.

### Rate limits

**Symptom:** Paper searches return errors or empty results after working initially.

**Causes and fixes:**
- Semantic Scholar enforces rate limits on unauthenticated requests (~100 per 5 minutes). If you hit this, wait a few minutes or add an API key.
- Set `S2_API_KEY` for higher rate limits: `export S2_API_KEY=your_key_here` (add to your shell profile for persistence).
- GitHub MCP has its own rate limits tied to your personal access token tier.

### API key setup

**Where to set keys:**

| Service | Environment variable | How to get one |
|---------|---------------------|----------------|
| Semantic Scholar | `S2_API_KEY` | [semanticscholar.org/product/api](https://www.semanticscholar.org/product/api) (free) |
| GitHub | `GITHUB_PERSONAL_ACCESS_TOKEN` | GitHub Settings > Developer settings > Personal access tokens |

Add these to your shell profile (`~/.bashrc`, `~/.zshrc`, or equivalent) so they persist across sessions:

```bash
export S2_API_KEY="your_semantic_scholar_key"
export GITHUB_PERSONAL_ACCESS_TOKEN="your_github_token"
```

### Node.js or Python not found

The GitHub and Filesystem MCP servers require Node.js (for `npx`). The Semantic Scholar MCP server requires Python (for `uvx`). If either is missing:

```bash
# macOS
brew install node
brew install uv

# Linux
sudo apt install nodejs npm
pip install uv
```
