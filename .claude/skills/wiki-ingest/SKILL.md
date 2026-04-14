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
   - Save the raw content to `$WIKI_ROOT/raw/<kebab-case-title>.md`
2. If the argument is a file path:
   - Read the file
   - Copy it to `$WIKI_ROOT/raw/` if not already there

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
   - Write out the execution plan: which pages to create, which to update, what links to add
   - Present the plan to the user

4. **Generate wiki pages**:
   - `wiki/sources/<source-name>.md` — source summary (1:1 with raw file)
   - `wiki/entities/<name>.md` — for each new entity. If page exists, append new info
   - `wiki/concepts/<name>.md` — for each new concept. If page exists, update
   - `wiki/synthesis/<name>.md` — only if meaningful cross-source connections found

5. **Cross-link**: Add `[[wikilinks]]` backlinks to related existing pages

6. **Update index**: Add new pages to `$WIKI_ROOT/wiki/index.md` under appropriate sections

7. **Update log**: Append entry to `$WIKI_ROOT/wiki/log.md`:
   ```
   ## [YYYY-MM-DD] ingest | <source title>
   <summary of what was created/updated>
   ```

8. **Lint**: Run `/wiki-lint` to verify consistency

9. **Commit & Push**: Ask the user if they want to commit and push:
   ```
   위키 변경사항을 커밋하고 푸시할까요? (Y/n)
   ```
   If the user agrees:
   - `cd $WIKI_ROOT`
   - `git add raw/ wiki/`
   - Commit following Conventional Commits: `docs: ingest <source title>`
   - `git push origin main`

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
