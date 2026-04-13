# ADR-0005: Introduce wiki-research skill

- Status: accepted
- Date: 2026-04-14

## Context

The llm-wiki requires 100% manual source collection. Users must find URLs or files themselves and pass them to `/wiki-ingest`. This creates a growth bottleneck — the wiki can only grow as fast as the user manually searches.

Existing projects (nvk/llm-wiki) demonstrate parallel multi-agent web research where multiple agents search from different angles simultaneously.

## Decision

Add a `/wiki-research` skill with:

- **Two modes**: explore (topic-based) and thesis (claim-based), auto-detected from input
- **Parallel agents**: 3 default, 5 with `--deep` flag
- **Source curation**: agents collect sources, assign trust tiers (★★★/★★☆/★☆☆), present list to user for selection
- **Integration**: selected sources flow through existing `/wiki-ingest` pipeline
- **Output**: research report saved to `wiki/synthesis/research-<topic>.md`

Key design decisions within this feature:
- Auto-detection over explicit mode flags (LLM analyzes input form)
- 3/5 agents over nvk's 5/8/10 (sufficient for personal use, better cost efficiency)
- User selection before ingest (HITL philosophy — no auto-ingest of unvetted sources)
- Trust tiers for informed selection (not filtering — user sees everything, tiers guide choice)

## Consequences

- Wiki growth is no longer bottlenecked by manual source discovery
- Token cost increases per research session (3-5 parallel agent contexts + web searches)
- Research reports in synthesis/ provide audit trail of what was investigated and what was selected
- Existing ingest pipeline is reused unchanged
