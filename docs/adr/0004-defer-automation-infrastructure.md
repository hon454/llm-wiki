# ADR-0004: Defer automation infrastructure

- Status: deferred
- Date: 2026-04-14

## Context

Some LLM wiki implementations (e.g., nashsu/llm_wiki) provide ingest queues with retry logic, real-time status tracking, and scenario templates. Evaluated against the current llm-wiki:

- **Ingest queue + retry**: llm-wiki is CLI-based; Claude Code processes sources sequentially by nature. The existing `/wiki-ingest` (no args) already scans `raw/` for unprocessed sources. Claude Code retries API errors internally.
- **Status tracking**: In a CLI, Claude Code already outputs progress. `wiki/log.md` provides post-hoc audit.
- **Scenario templates**: llm-wiki has a fixed schema in CLAUDE.md. Changing the schema means editing CLAUDE.md directly — a template abstraction adds unnecessary complexity for a single-user system.

## Decision

Defer all three. The current CLI-native behavior is sufficient.

## Consequences

- No additional infrastructure code to maintain
- Source processing remains simple and predictable
- Single-user CLI workflow is unaffected

## Revisit Conditions

- Batch ingest of 10+ sources becomes a regular workflow and failures cause data loss
- Multiple users or automated pipelines feed sources into the wiki
