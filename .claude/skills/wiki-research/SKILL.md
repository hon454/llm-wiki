---
name: wiki-research
description: Parallel multi-agent web research with source curation and wiki integration
---

# wiki-research

## Usage

```
/wiki-research <topic or thesis>
/wiki-research --deep <topic or thesis>
/wiki-research --mode explore|thesis <topic or thesis>
```

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

### Step 1: Parse Input

- If `--mode` flag is provided, use that mode
- Otherwise, analyze the input text:
  - Claim or question form (contains "인가", "일까", "보다", "vs", "?" etc.) → **thesis mode**
  - Topic or keyword form → **explore mode**
- If `--deep` flag is present, set agent count to 5; otherwise 3
- Present the detected mode and agent count to the user for confirmation:
  - "탐색형 리서치, 에이전트 3개로 진행합니다." or "논증형 리서치, 에이전트 3개로 진행합니다."

### Step 2: Dispatch Parallel Agents

Use the Agent tool to dispatch all agents in a **single message** (parallel execution).

Each agent receives a prompt with:
1. The research topic/thesis
2. Its assigned role (see Role Assignment below)
3. Instructions to use WebSearch to find 3-5 relevant sources
4. For each source found, return: title, URL, one-line summary in Korean, source type (academic/blog/news/social/official)

**Explore mode — 3 agents (default):**

| Agent | Role | Search Angle |
|-------|------|-------------|
| 1 | Academic / official | Search for papers, official documentation, RFCs, GitHub repos |
| 2 | Practitioner | Search for technical blog posts, tutorials, conference talks |
| 3 | Trends | Search for recent news, community discussions, social media posts |

**Explore mode — deep (+2 agents):**

| Agent | Role | Search Angle |
|-------|------|-------------|
| 4 | Historical | Search for foundational papers, historical context, origin stories |
| 5 | Adjacent | Search for related fields, cross-disciplinary perspectives |

**Thesis mode — 3 agents (default):**

| Agent | Role | Search Angle |
|-------|------|-------------|
| 1 | Supporting | Search for evidence and arguments supporting the thesis |
| 2 | Opposing | Search for evidence and arguments opposing the thesis |
| 3 | Neutral | Search for meta-analyses, systematic reviews, balanced overviews |

**Thesis mode — deep (+2 agents):**

| Agent | Role | Search Angle |
|-------|------|-------------|
| 4 | Mechanism | Search for underlying mechanisms, theoretical principles |
| 5 | Empirical | Search for empirical data, case studies, experiments |

### Step 3: Aggregate and Present Results

1. Collect all agent results
2. Deduplicate sources by URL
3. If an agent returns no results (e.g., WebSearch failure), log the failure and continue with remaining agents' results.
4. Assign trust tier to each source based on source type:
   - ★★★: academic papers, official docs, official repos, standards (arxiv, RFC, GitHub official)
   - ★★☆: technical blogs, conference talks, expert posts, tutorials
   - ★☆☆: news, social media, forums, community posts
5. Sort by tier (highest first), then by relevance
6. Present the source list to the user in this format:

```
## 리서치 결과: "<topic>" (<mode>, 에이전트 <N>개)

수집된 소스 <total>건:

 #  | ★   | 제목                          | 출처
----|-----|------------------------------|------------------
 1  | ★★★ | <title>                       | <domain>
 2  | ★★☆ | <title>                       | <domain>
 ...

인제스트할 소스 번호를 선택해주세요 (예: 1,2,4):
```

### Step 4: User Selection and Ingest

1. Parse user's selection (comma-separated numbers)
2. For each selected source, in sequence:
   a. Use WebFetch to download the content
   b. Save to `$WIKI_ROOT/raw/<kebab-case-title>.md`
   c. Invoke the full `/wiki-ingest` pipeline on that source (follow all CLAUDE.md rules: cognitive scaffolding, HITL, frontmatter, cross-references, index/log updates). For each source, invoke the full `/wiki-ingest` pipeline including its HITL feedback step. Each source gets its own ingest cycle.
3. If a source fails to fetch or ingest, log the failure and continue with remaining sources

### Step 5: Generate Research Report

After all ingests complete (or are skipped), create a synthesis page:

**File:** `$WIKI_ROOT/wiki/synthesis/research-<kebab-case-topic>.md`

**Explore mode report structure:**

```markdown
---
tags: [research, <topic-tags>]
sources: [<all-ingested-source-filenames>]
status: active
created: <today>
updated: <today>
---

# <주제> 리서치

## 개요
- 모드: 탐색형
- 에이전트: <N>개
- 수집: <total>건 / 선별: <selected>건

## 수집 소스 전체 목록
| # | ★ | 제목 | URL | 선별 |
|---|---|------|-----|------|
| 1 | ★★★ | ... | ... | ✓ |
| 2 | ★★☆ | ... | ... | - |

## 핵심 발견
- 발견 1: ... ([[related-page]])
- 발견 2: ...

## 소스별 요약
### [[source1]]
- 핵심 내용 요약

### [[source2]]
- 핵심 내용 요약
```

**Thesis mode adds these sections after 핵심 발견:**

```markdown
## 증거 정리
### 찬성 증거
- 증거 1: ... (출처: [[source1]])

### 반대 증거
- 증거 1: ... (출처: [[source3]])

### 중립/메타 분석
- ...

## 종합 판단
현재 증거를 종합하면 ...
```

### Step 6: Update Index and Log

1. Add the research report to `wiki/index.md` under the Synthesis section
2. Append to `wiki/log.md`:

```markdown
## [<today>] research | <topic>
<mode> 리서치. 에이전트 <N>개, <total>건 수집, <selected>건 선별 인제스트.
```

### Step 7: Run lint

Run `/wiki-lint` to validate the wiki after all changes.
