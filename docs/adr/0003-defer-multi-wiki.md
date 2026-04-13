# ADR-0003: Defer multi-wiki architecture

- Status: deferred
- Date: 2026-04-14

## Context

Some LLM wiki implementations (e.g., nvk/llm-wiki) use topic-based sub-wikis with a hub registry. This separates unrelated domains and reduces the scope of what the LLM reads per query.

Evaluated against the current llm-wiki:

- The existing type-based structure + tag system already provides topic separation
- Splitting into sub-wikis would break cross-topic wikilinks — the core value of the wiki
- Karpathy noted the pattern works well at ~100 articles / ~400K words
- Migration from single to multi-wiki is non-destructive (directory moves + index splits)

## Decision

Defer. Maintain the single wiki structure.

## Consequences

- Wikilinks remain simple and global
- Tags provide sufficient topic filtering
- No hub/registry complexity

## Revisit Conditions

- Wiki exceeds 100 source documents or 400K total words
- Query performance degrades noticeably (LLM struggles to find relevant pages)
- User has genuinely unrelated knowledge domains that pollute each other's results
