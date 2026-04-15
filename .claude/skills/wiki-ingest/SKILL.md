---
name: wiki-ingest
description: Ingest a source (URL, file path, or scan raw/) into the wiki — summarizes, generates pages, cross-links, and lints
disable-model-invocation: true
---

# Wiki Ingest

Ingest sources into the llm-wiki knowledge base.

All user-facing messages must be presented in Korean.

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
     - Inform the user the source is already ingested. Offer: (1) rename, (2) skip. Suggest `/wiki-retract` then re-ingest to update.
     - Overwrite is **disallowed** for ingested sources.
   - If the target filename exists but has NOT been ingested (no `wiki/sources/` page):
     - Inform the user the file already exists. Offer: (1) rename, (2) skip. The user must replace or delete the file manually.
     - Overwrite is **disallowed** for un-ingested sources as well.
   - If no collision, save the file normally.

**Without argument** (`/wiki-ingest`):

1. Scan `$WIKI_ROOT/raw/` for files
2. Check `$WIKI_ROOT/wiki/sources/` for existing summary pages
3. List sources that don't have a corresponding wiki/sources/ page yet
4. Process each unprocessed source (confirm with user before proceeding)

### Processing Steps

For each source to ingest:

1. **Read and summarize**: Read the source from `raw/`. Present a concise summary of key points to the user.

2. **Source quality gate**: Evaluate the source against the criteria below and present a 4-line assessment (one per signal, with a pass/warn/fail indicator and one-line reason) followed by an overall recommendation.

   **INGEST signals** (reasons to proceed):

   | Signal | Check |
   |--------|-------|
   | Durability | Will this remain useful months from now? News, trending takes, release notes score low. |
   | Substance | Does it contain reasoning and evidence, not just conclusions? Listicles, SEO filler, summaries-of-summaries score low. |
   | Connection | Does it reinforce, extend, or challenge an existing wiki page? |
   | Originality | Does the wiki already cover this idea adequately? Duplicates add clutter, not knowledge. |

   **SKIP signals** (reasons to reject):

   | Signal | Description |
   |--------|-------------|
   | Ephemeral | Content that depends entirely on timeliness (breaking news, social media threads, release announcements). |
   | No source | Personal notes or TIL entries with no original source — ingest the source that triggered the learning instead. |
   | Just-in-case | Saving "because it might be useful someday" with no concrete connection to a goal or existing page. |
   | Low-signal | Unvetted, unattributed, or low-credibility material. |

   **One-sentence test**: If you cannot answer "Which wiki page does this update or create, and why does it matter?" in one sentence, recommend skipping.

   **Decision rules**:
   - All clear: proceed automatically
   - Any warning: present concerns, ask user to confirm
   - Any rejection signal: recommend skipping, explain why, suggest alternatives (e.g., find the primary source instead)
   - User always has final say — this gate is advisory, not a hard block

3. **Get feedback**: Ask the user if there's a specific direction or emphasis they'd like. Wait for response.

4. **Cognitive scaffolding** (per CLAUDE.md):
   - List all existing wiki pages that may need updating
   - Review related ADRs — check if the current ingest conflicts with any accepted ADR's decision
   - Write out the execution plan: which pages to create, which to update, what links to add
   - Present the plan to the user

5. **Slug uniqueness check** — before creating any wiki page:
   - Check all `wiki/**/*.md` for an existing file with the same basename as the page to be created
   - On collision, suggest alternative slugs and ask the user to choose

6. **Generate wiki pages**:
   - `wiki/sources/<source-name>.md` — source summary (1:1 with raw file)
   - `wiki/entities/<name>.md` — for each new entity. If page exists, append new info
   - `wiki/concepts/<name>.md` — for each new concept. If page exists, update
   - `wiki/synthesis/<name>.md` — only if meaningful cross-source connections found

7. **Cross-link**: Add `[[wikilinks]]` backlinks to related existing pages

8. **Update index**: Add new pages to `$WIKI_ROOT/wiki/index.md` under appropriate sections

9. **Update log**:
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

10. **Lint**: Run `/wiki-lint` to verify consistency

11. **Plan verification** — re-read the execution plan from the cognitive scaffolding step:
    - Verify all planned page creates/updates were completed
    - Report any omissions before proceeding to commit

12. **Commit & Push**: Ask the user if they want to commit and push.
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
