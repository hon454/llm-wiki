---
name: wiki-lint
description: Health check — detects broken wikilinks, orphan pages, missing index entries, incomplete frontmatter, and content contradictions
context: fork
---

# Wiki Lint

Validate the consistency and health of the llm-wiki knowledge base.

## Path Resolution

```bash
WIKI_ROOT=!`cat ~/.config/llm-wiki/root`
```

## CLAUDE.md Loading

```bash
!`WIKI_ROOT=$(cat ~/.config/llm-wiki/root); if [ "$(pwd)" != "$WIKI_ROOT" ]; then cat "$WIKI_ROOT/CLAUDE.md"; fi`
```

## Checks

Run all checks below and compile a report.

### 1. Broken Wikilinks

- Scan all `wiki/**/*.md` files for `[[wikilink]]` references
- For each link, verify a corresponding `.md` file exists in `wiki/`
- Report: list of broken links with the file they appear in

### 2. Index Coverage

- Read `wiki/index.md`
- Scan all `.md` files in `wiki/sources/`, `wiki/entities/`, `wiki/concepts/`, `wiki/synthesis/`
- Report: pages that exist but are not listed in `index.md`

### 3. Orphan Pages

- A page is an orphan if no other page links to it via `[[wikilink]]` AND it's not in `index.md`
- Report: list of orphan pages

### 4. Frontmatter Validation

- Every wiki page must have: `tags`, `sources`, `status`, `created`, `updated`
- `status` must be `active` or `needs-review`
- `created`/`updated` must be valid ISO dates (YYYY-MM-DD)
- Report: pages with missing or invalid frontmatter fields

### 5. Content Contradiction Detection

- For each entity/concept page, check if multiple sources describe it differently
- Flag cases where conflicting statements exist about the same topic
- This is a best-effort heuristic check — report suspected contradictions for human review

### 6. Slug Conflicts

- Collect all `.md` file basenames under `wiki/` (excluding `index.md` and `log.md`)
- Flag any basename that appears in more than one directory
- Report format:
  ```
  ### Slug Conflicts
  - `transformer`: wiki/entities/transformer.md, wiki/concepts/transformer.md
  ```

## Report Format

Present results as:

```markdown
# Wiki Lint Report

## Summary
- Total pages: N
- Broken links: N
- Missing from index: N
- Orphan pages: N
- Frontmatter issues: N
- Suspected contradictions: N
- Slug conflicts: N

## Details

### Broken Links
- `wiki/concepts/foo.md`: [[bar]] — target not found

### Missing from Index
- `wiki/entities/baz.md`

### Orphan Pages
- (none)

### Frontmatter Issues
- `wiki/sources/qux.md`: missing `tags` field

### Suspected Contradictions
- (none)

### Slug Conflicts
- `transformer`: wiki/entities/transformer.md, wiki/concepts/transformer.md
```

## Auto-fix

After presenting the report, offer to fix issues automatically:
- Add missing pages to `index.md`
- Fix frontmatter (add missing fields with defaults)
- For broken links: suggest creating stub pages or removing the link

Do NOT auto-fix contradictions — these require human judgment.

## Log

Append to `$WIKI_ROOT/wiki/log.md`:
```
## [YYYY-MM-DD] lint | 정기 검진
<summary of findings and fixes>
```
