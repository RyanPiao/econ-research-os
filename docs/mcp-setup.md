# MCP Server Setup

## What are MCP servers?

MCP (Model Context Protocol) servers are external tools that extend Claude Code's capabilities beyond reading and writing local files. They let Claude interact with APIs, databases, and web services directly from within a conversation.

For econ-research-os, MCP servers provide three key capabilities:

- **Academic paper search** via Semantic Scholar's API
- **GitHub integration** for repository management and collaboration
- **Cross-directory file access** for working across multiple project folders

Without MCP servers installed, you can still use most skills (reading, writing, analysis, verification), but you will not be able to search for papers directly from Claude Code.

## Installation

Run these commands in your terminal (not inside Claude Code).

### Semantic Scholar (strongly recommended)

Provides academic paper search, citation data, and metadata lookup. This is the backbone of the Discover stage (`/paper-search`, `/citation-chase`, `/deep-search`).

```bash
claude mcp add semantic-scholar -s user -- \
  uvx --from git+https://github.com/akapet00/semantic-scholar-mcp \
  semantic-scholar-mcp
```

Works without an API key. If you have a Semantic Scholar API key, you can set it as an environment variable for higher rate limits:

```bash
export SEMANTIC_SCHOLAR_API_KEY="your-key-here"
```

### GitHub (optional)

Enables pushing repos, managing pull requests, and other GitHub operations from within Claude Code.

```bash
claude mcp add github -s user -- \
  npx -y @modelcontextprotocol/server-github
```

Requires a GitHub personal access token set via `GITHUB_TOKEN`.

### Filesystem (optional)

Allows Claude to read and write files across multiple directories, useful if your papers, data, and project live in different locations.

```bash
claude mcp add filesystem -s user -- \
  npx -y @modelcontextprotocol/server-filesystem ~/projects ~/papers
```

Adjust the paths (`~/projects ~/papers`) to match your own directory structure.

## API keys

| Server | Key required? | How to get one |
|--------|--------------|----------------|
| Semantic Scholar | No (optional for higher rate limits) | [semanticscholar.org/product/api](https://www.semanticscholar.org/product/api) |
| GitHub | Yes (`GITHUB_TOKEN`) | [github.com/settings/tokens](https://github.com/settings/tokens) |
| Filesystem | No | N/A |

## Verifying installation

After adding MCP servers, restart Claude Code and run:

```bash
claude mcp list
```

You should see your installed servers listed. If a server fails to connect, check that the required dependencies (`uvx`, `npx`) are installed and that any required API keys are set.
