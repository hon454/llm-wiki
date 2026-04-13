# ADR-0002: Plain markdown only, no vector store

- Status: accepted
- Date: 2026-04-12

## Context

Most LLM knowledge base tools use vector stores (embeddings + similarity search) for retrieval. Karpathy's LLM Wiki pattern proposes an alternative: the LLM compiles knowledge into plain markdown files and uses index files + wikilinks for navigation, with no embedding or vector database.

At small-to-medium scale (~100 articles, ~400K words), the LLM can effectively read index files and follow wikilinks to find relevant content without semantic search.

## Decision

Use plain markdown files only. No vector store, no embeddings, no database.

- Storage: markdown files in `wiki/` directory
- Navigation: `wiki/index.md` catalog + `[[wikilink]]` cross-references
- Retrieval: LLM reads index, identifies relevant pages, reads them directly
- Viewer: Obsidian (graph view, backlinks, search)

## Consequences

- Zero infrastructure: no database to run, no embeddings to compute, no dependencies beyond git
- Fully portable: clone the repo and the entire knowledge base is accessible
- Human-readable: every page is a standard markdown file viewable in any editor
- Obsidian-compatible: wikilink syntax works natively with Obsidian graph and backlinks
- Trade-off: at very large scale (1000+ articles), index-based navigation may become a bottleneck. At that point, consider semantic search or multi-wiki partitioning (see ADR-0003).
