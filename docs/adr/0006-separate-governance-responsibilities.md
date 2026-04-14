# ADR-0006: Separate governance responsibilities between ingest and lint

- Status: accepted
- Date: 2026-04-14

## Context

CLAUDE.md's cognitive scaffolding requires ADR revisit condition checks before ingest, but revisit conditions are quantitative (100 sources, 400K words) — evaluating them on every ingest adds overhead with no value at current scale. Lint already scans the entire wiki, making quantitative checks a natural fit, but reliable monitoring requires structured metrics in `wiki/log.md` rather than free-form prose parsing.

## Decision

- Ingest's ADR review is scoped to "does this ingest conflict with any accepted ADR decision?"
- Quantitative evaluation of deferred ADR revisit conditions is lint's responsibility
- Lint relies on explicit `metrics:` lines in `wiki/log.md` for quantitative workflow monitoring; it must not infer counts from prose
- Add plan verification step to ingest completion

## Consequences

- Ingest stays lightweight
- Lint doubles as a governance gate
- Clear responsibility boundary between CLAUDE.md and skills
- Quantitative governance checks become auditable and machine-parseable

## Revisit Conditions

- Number of ADRs grows large enough that identifying relevant ADRs during ingest becomes difficult
