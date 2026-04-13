# ADR-0001: Type-based directory structure

- Status: accepted
- Date: 2026-04-12

## Context

The wiki needs a way to organize pages. Two main options were considered:

1. **Topic-based**: Group by subject area (e.g., `ai/`, `tools/`, `history/`). Requires deciding categories upfront and reclassifying as the wiki grows.
2. **Type-based**: Group by information type (`sources/`, `entities/`, `concepts/`, `synthesis/`). Aligns with how the LLM processes information — it naturally distinguishes between summarizing a source, describing an entity, explaining a concept, and synthesizing across sources.

## Decision

Use type-based directory structure with four categories:

- `sources/` — one page per source document (1:1 with `raw/`)
- `entities/` — one page per proper noun (person, tool, org)
- `concepts/` — one page per abstract concept or theory
- `synthesis/` — cross-cutting analysis combining multiple sources

## Consequences

- Domain-agnostic: works for any topic without predefined categories
- Each type has clear, differentiated update rules (sources: create once; entities: accumulate; concepts: deepen; synthesis: cross-reference)
- Cross-topic connections happen naturally through wikilinks rather than being forced by directory co-location
- Trade-off: a single entity directory may grow large if many proper nouns accumulate across diverse topics
