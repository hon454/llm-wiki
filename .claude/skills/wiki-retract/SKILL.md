---
name: wiki-retract
description: Retract a source from the wiki — deletes raw file, removes source page, updates affected pages, runs lint
---

# Wiki Retract

Retract a source from the llm-wiki knowledge base.

## Usage

```
/wiki-retract <source-name> --reason "reason"
/wiki-retract --dry-run <source-name>
/wiki-retract
```

- `<source-name>`: raw filename or source page slug (with or without `.md`)
- `--reason "reason"`: required; reject invocation if missing
- `--dry-run`: blast radius analysis only, no changes
- No arguments: show raw source list for selection

## Path Resolution

```bash
WIKI_ROOT=!`cat ~/.config/llm-wiki/root`
```

## CLAUDE.md Loading

If not already in the wiki workspace, load the wiki rules:

```bash
!`WIKI_ROOT=$(cat ~/.config/llm-wiki/root); if [ "$(pwd)" != "$WIKI_ROOT" ]; then cat "$WIKI_ROOT/CLAUDE.md"; fi`
```

## Workflow

### Step 1: Identify Target

1. If no arguments provided, list all files in `$WIKI_ROOT/raw/` and ask the user to select one
2. Normalize identifiers:
   - `RAW_FILE`: add `.md` if missing
   - `SOURCE_SLUG`: basename of `RAW_FILE` without `.md`
3. Verify `raw/<RAW_FILE>` exists; abort if not found
4. If `--reason` is missing (and not `--dry-run` only), reject:
   ```
   --reason "사유" 를 반드시 입력해주세요.
   예: /wiki-retract karpathy-llm-wiki --reason "outdated content"
   ```

### Step 2: Blast Radius Analysis

Scan `wiki/**/*.md` for references to the source:

1. **Deletion target**: `wiki/sources/<SOURCE_SLUG>.md` (if exists)
2. **Affected (frontmatter)**: pages with `sources:` frontmatter listing `<RAW_FILE>`
3. **Affected (body)**: pages with `[[<SOURCE_SLUG>]]` wikilinks in the body text
4. **Sole source warning**: pages where this is the only entry in `sources:` frontmatter — warn prominently

### Step 3: Preview

Present the blast radius to the user. If `--dry-run`, stop here.

```
raw/<RAW_FILE> 철회 영향:
- 삭제: wiki/sources/<SOURCE_SLUG>.md
- 영향: wiki/entities/andrej-karpathy.md (sources 참조)
- ⚠ 유일 소스: wiki/concepts/llm-wiki-pattern.md

사유: "<reason>"
진행할까요? (Y/n)
```

### Step 4: Execute

After user confirms:

1. Delete `raw/<RAW_FILE>`
2. Delete `wiki/sources/<SOURCE_SLUG>.md` (if exists)
3. **Affected pages (frontmatter)**: remove the file from `sources:` frontmatter, set `status: needs-review`
4. **Affected pages (body)**: convert `[[<SOURCE_SLUG>]]` wikilinks to plain text (`<SOURCE_SLUG>`), set `status: needs-review`
5. Remove the deleted source page entry from `wiki/index.md`
6. Append to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] retract | <SOURCE_SLUG>
   사유: <reason>. 영향 페이지: <count>건.
   ```

Entity/concept page body text is only modified to convert wikilinks to the deleted source page into plain text. All other body content is preserved. `needs-review` status signals the user to review the page's claims manually.

### Step 5: Lint

Run `/wiki-lint` for consistency verification.

### Step 6: Commit & Push

Ask the user if they want to commit and push:

```
위키 변경사항을 커밋하고 푸시할까요? (Y/n)
```

If the user agrees:
1. `cd $WIKI_ROOT`
2. `git add raw/ wiki/`
3. Commit following Conventional Commits: `docs: retract <source-slug>`
4. `git push origin HEAD`
