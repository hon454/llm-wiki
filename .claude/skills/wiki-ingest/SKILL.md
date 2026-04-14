---
name: wiki-ingest
description: Ingest a source (URL, file path, or scan raw/) into the wiki — summarizes, generates pages, cross-links, and lints
disable-model-invocation: true
---

# Wiki Ingest

Ingest sources into the llm-wiki knowledge base.

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

### Input Handling

**With argument** (`/wiki-ingest <URL or file path>`):

1. If the argument is a URL:
   - Fetch the content using WebFetch
   - Determine the target filename: `$WIKI_ROOT/raw/<kebab-case-title>.md`
2. If the argument is a file path:
   - Read the file
   - Determine the target filename in `$WIKI_ROOT/raw/`

3. **Raw file collision check** — before saving to `raw/`:
   - If the target filename already exists in `raw/` AND a corresponding `wiki/sources/` page exists (already ingested):
     ```
     raw/<filename>.md 는 이미 인제스트된 소스입니다.
     (1) 이름 변경 — 새 이름을 입력
     (2) 건너뛰기 — 이 소스를 처리하지 않음
     같은 소스를 갱신하려면 /wiki-retract 후 재인제스트하세요.
     ```
     Overwrite is **disallowed** for ingested sources.
   - If the target filename exists but has NOT been ingested (no `wiki/sources/` page):
     ```
     raw/<filename>.md 가 이미 존재합니다.
     (1) 이름 변경 — 새 이름을 입력
     (2) 건너뛰기 — 이 소스를 처리하지 않음
     raw/ 내용 교체는 LLM이 수행하지 않습니다. 직접 파일을 교체하거나 삭제 후 다시 시도하세요.
     ```
     Overwrite is **disallowed** for un-ingested sources as well.
   - If no collision, save the file normally.

**Without argument** (`/wiki-ingest`):

1. Scan `$WIKI_ROOT/raw/` for files
2. Check `$WIKI_ROOT/wiki/sources/` for existing summary pages
3. List sources that don't have a corresponding wiki/sources/ page yet
4. Process each unprocessed source (confirm with user before proceeding)

### Processing Steps

For each source to ingest:

1. **Read and summarize**: Read the source from `raw/`. Present a concise summary of key points to the user.

2. **Get feedback**: Ask the user if there's a specific direction or emphasis they'd like. Wait for response.

3. **Cognitive scaffolding** (per CLAUDE.md):
   - List all existing wiki pages that may need updating
   - Review related ADRs — check if the current ingest conflicts with any accepted ADR's decision
   - Write out the execution plan: which pages to create, which to update, what links to add
   - Present the plan to the user

4. **Slug uniqueness check** — before creating any wiki page:
   - Check all `wiki/**/*.md` for an existing file with the same basename as the page to be created
   - On collision, do not create the page; suggest alternatives:
     ```
     wiki/entities/transformer.md 가 이미 존재합니다.
     제안: transformer-architecture, transformer-model
     사용할 이름을 선택하거나 입력해주세요:
     ```

5. **Generate wiki pages**:
   - `wiki/sources/<source-name>.md` — source summary (1:1 with raw file)
   - `wiki/entities/<name>.md` — for each new entity. If page exists, append new info
   - `wiki/concepts/<name>.md` — for each new concept. If page exists, update
   - `wiki/synthesis/<name>.md` — only if meaningful cross-source connections found

6. **Cross-link**: Add `[[wikilinks]]` backlinks to related existing pages

7. **Update index**: Add new pages to `$WIKI_ROOT/wiki/index.md` under appropriate sections

8. **Update log**:
   - Append the per-source entry to `$WIKI_ROOT/wiki/log.md`:
     ```
     ## [YYYY-MM-DD] ingest | <source title>
     <summary of what was created/updated>
     ```
   - If this `/wiki-ingest` invocation processed more than one source, append one additional batch summary entry after all per-source entries:
     ```
     ## [YYYY-MM-DD] ingest | batch run
     metrics: sources_processed=<N>
     <summary of the batch>
     ```

9. **Lint**: Run `/wiki-lint` to verify consistency

10. **Plan verification** — re-read the execution plan from the cognitive scaffolding step:
    - Verify all planned page creates/updates were completed
    - Report any omissions before proceeding to commit

11. **Commit & Push**: Ask the user if they want to commit and push:
   ```
   위키 변경사항을 커밋하고 푸시할까요? (Y/n)
   ```
   If the user agrees:
   - `cd $WIKI_ROOT`
   - `git add raw/ wiki/`
   - Commit following Conventional Commits: `docs: ingest <source title>`
   - `git push origin HEAD`

### HITL Rules

- If new source contradicts an existing page: set `status: needs-review`, do NOT overwrite, present both versions
- If an existing entity/concept page needs significant rewriting: set `status: needs-review`, present both versions
- Minor additions to existing pages: proceed normally

### Frontmatter

All generated pages must include:

```yaml
---
tags: [relevant, tags]
sources: [source-filename.md]
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```
