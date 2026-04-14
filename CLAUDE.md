# LLM Wiki Rules

## Commit Convention

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <description>
```

Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `style`, `test`

- Subject line: imperative mood, lowercase, no period, under 72 characters
- Body (optional): explain *why*, not *what*

This file defines the rules and schema for the llm-wiki knowledge base. All wiki operations (ingest, query, lint) must follow these rules.

## Language

- All wiki page content (titles, body text, summaries) must be written in **Korean**
- All system-level files must be written in **English**: CLAUDE.md, SKILL.md, .gitignore, commit messages, frontmatter keys/values
- Frontmatter field names and values (tags, status) remain in English

## Directory Roles

| Path | Role | Who writes | Update pattern |
|------|------|-----------|----------------|
| `raw/` | Source storage (LLM does not modify content; deletion only via retract) | User + LLM | LLM saves during ingest, or user adds directly |
| `wiki/sources/` | Source summaries | LLM only | Created once per ingest, rarely modified |
| `wiki/entities/` | Proper noun pages (people, tools, orgs) | LLM only | Accumulated when mentioned in new sources |
| `wiki/concepts/` | Abstract concept pages (theories, frameworks) | LLM only | Updated as understanding deepens |
| `wiki/synthesis/` | Cross-cutting analysis | LLM only | Created when connections found, or query results saved |
| `wiki/index.md` | Full page catalog | LLM only | Updated every ingest/query-save |
| `wiki/log.md` | Operation history | LLM only | Append on every operation |

## Page Frontmatter

Every wiki page must include this frontmatter:

```yaml
---
tags: [tag1, tag2]
sources: [source-filename.md]
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

- `tags`: topic/domain tags (English, lowercase, hyphenated)
- `sources`: list of source filenames (in `raw/`) that contributed to this page
- `status`: `active` or `needs-review`
- `created`/`updated`: ISO date strings

## File Naming

- All filenames use **kebab-case** (e.g., `attention-is-all-you-need.md`)
- No spaces, no uppercase in filenames

## Cross-References

- Use `[[wikilink]]` syntax for all cross-references between wiki pages
- Use the filename without extension as the link target (e.g., `[[transformer]]`)
- When creating or updating a page, add backlinks to related existing pages

## Page Type Guidelines

### sources/
- One page per source document (1:1 mapping with `raw/` files)
- Summarize the key points, arguments, and contributions of the source
- Link to entities and concepts mentioned in the source

### entities/
- One page per proper noun (person, tool, organization, dataset, etc.)
- Accumulate new information from each source into the existing page
- Do NOT overwrite existing content — append new sections or update existing ones

### concepts/
- One page per abstract concept, theory, or framework
- Update as understanding deepens across multiple sources
- Include relationships to other concepts

### synthesis/
- Cross-cutting analysis combining multiple sources/concepts
- Comparisons, trend analyses, or query-result summaries
- Must reference the specific pages it synthesizes

## index.md Format

```markdown
# Wiki Index

## Sources
- [[source-name]] — one-line description

## Entities
- [[entity-name]] — one-line description

## Concepts
- [[concept-name]] — one-line description

## Synthesis
- [[synthesis-name]] — one-line description
```

## log.md Format

Append-only, grep-parseable. Each entry:

```markdown
## [YYYY-MM-DD] action | title
metrics: key=value, key=value
Description of what was done.
```

- `metrics:` is optional for simple single-source operations
- `metrics:` is required when an operation processes multiple sources or downstream lint checks depend on quantitative counts
- Use comma-separated `key=value` pairs (for example: `sources_processed=12, agents=5`)

Actions: `ingest`, `query`, `lint`, `research`, `retract`

## Cognitive Scaffolding

Before modifying files during ingest:
1. List all existing wiki pages that may need updating
2. Review related ADRs — check if the current ingest conflicts with any accepted ADR's decision
3. Write out the execution plan (which pages to create, which to update, what links to add)
4. Follow the plan page by page
5. After completion, verify the plan was fully executed

> Note: ADR revisit condition의 정량적 평가(소스 수, 단어 수 등)는 lint가 담당한다.

## Human-in-the-Loop (HITL) Rules

- **Contradictory information**: When new source contradicts an existing page, set `status: needs-review` on the existing page. Do NOT silently overwrite. Present both versions to the user.
- **Significant rewrites**: When deleting or substantially rewriting an entity/concept page, set `status: needs-review` and present both versions.
- **Minor additions**: Appending new information to an existing page may proceed without user confirmation.

## Multimodal Handling

- When ingesting sources with images, save images to `raw/assets/`
- Describe each image's content as text in the wiki page
- Reference images using relative markdown paths: `![description](../../raw/assets/image-name.png)`

## ADR (Architecture Decision Records)

- Review existing ADRs in `docs/adr/` before making design decisions to check for conflicts
- Write a new ADR when a new design decision or deferral/rejection is made
- When a deferred ADR's Revisit Conditions are met, notify the user
- ADR numbers are sequential, filenames use kebab-case
