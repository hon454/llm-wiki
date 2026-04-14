---
name: wiki-query
description: Query the wiki knowledge base — reads relevant pages to answer questions, optionally saves result as synthesis
context: fork
---

# Wiki Query

Answer questions using the llm-wiki knowledge base.

## Path Resolution

```bash
WIKI_ROOT=!`cat ~/.config/llm-wiki/root`
```

## CLAUDE.md Loading

```bash
!`WIKI_ROOT=$(cat ~/.config/llm-wiki/root); if [ "$(pwd)" != "$WIKI_ROOT" ]; then cat "$WIKI_ROOT/CLAUDE.md"; fi`
```

## Workflow

1. **Read index**: Read `$WIKI_ROOT/wiki/index.md` to identify all available pages

2. **Find relevant pages**: Based on the user's question, identify which wiki pages are most relevant. Read those pages.

3. **Generate answer**: Using the content from the wiki pages, generate a comprehensive answer in Korean. Cite sources using `[[wikilinks]]`.

4. **Return answer**: Present the answer to the main conversation. The answer should:
   - Be written in Korean
   - Reference specific wiki pages with `[[wikilinks]]`
   - Clearly indicate if the wiki doesn't have enough information to fully answer

5. **Save prompt**: After presenting the answer, ask: "이 답변을 위키에 저장할까요?" (Save this answer to the wiki?)

6. **Slug uniqueness check** — if the user confirms saving:
   - Determine the target filename: `wiki/synthesis/<kebab-case-question>.md`
   - Check all `wiki/**/*.md` for an existing file with the same basename
   - On collision, do not create the page; suggest alternatives:
     ```
     wiki/concepts/agentic-coding-workflow.md 가 이미 존재합니다.
     제안: codex-vs-claude-query, agentic-coding-workflow-comparison
     사용할 이름을 선택하거나 입력해주세요:
     ```

7. If the user confirms:
   - Save to `$WIKI_ROOT/wiki/synthesis/<chosen-kebab-case-question>.md` with proper frontmatter
   - Update `$WIKI_ROOT/wiki/index.md`
   - Append to `$WIKI_ROOT/wiki/log.md`:
     ```
     ## [YYYY-MM-DD] query | <question summary>
     synthesis/<filename>.md로 저장.
     ```
