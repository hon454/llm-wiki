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

### 7. ADR Revisit Condition Check

For each deferred ADR in `docs/adr/`:

1. Measure quantitative conditions:
   - ADR-0003: count `.md` files in `raw/` (threshold: 100), excluding `raw/assets/`; count total words in `wiki/` (threshold: 400K)
   - ADR-0004: scan `wiki/log.md` for the last 30 days; count entries with a `metrics:` line containing `sources_processed=<N>` where `N >= 10`. Alert if 3+ such entries exist in the window.
2. Report at 80% (warning) and 100% (alert) for threshold-based conditions.
3. For ADR-0004, use only explicit `metrics:` lines — do **not** infer source counts from free-form prose:
   ```
   ### ADR Revisit Alerts
   - ⚠ ADR-0003 (multi-wiki): 소스 82/100건 — 임계값 80% 도달
   - ⚠ ADR-0004 (automation): 최근 30일 내 10+ 소스 배치 인제스트 3회 감지
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
- ADR revisit alerts: N

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

### ADR Revisit Alerts
- ⚠ ADR-0003 (multi-wiki): 소스 82/100건 — 임계값 80% 도달
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
