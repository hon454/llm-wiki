# llm-wiki

Karpathy의 LLM Wiki 패턴을 기반으로 한 개인 지식 베이스.

소스를 투입하면 LLM이 읽고, 요약하고, 기존 지식과 연결하고, 위키 페이지를 생성/업데이트한다.

## 구조

```
raw/          # 원본 소스 (불변)
wiki/         # LLM이 생성·관리하는 위키
  sources/    # 소스별 요약
  entities/   # 사람, 도구, 조직
  concepts/   # 개념, 이론, 프레임워크
  synthesis/  # 종합 분석, 비교
  index.md    # 전체 카탈로그
  log.md      # 작업 이력
```

## 사용법

### 초기 설정 (1회)

```
/wiki-setup
```

다른 워크스페이스에서도 접근할 수 있도록 글로벌 설정을 등록한다.

### 소스 투입

```
/wiki-ingest <URL 또는 파일경로>
/wiki-ingest                        # raw/에서 미처리 소스 스캔
```

### 질의

```
/wiki-query <질문>
```

위키를 참조해 답변을 생성하고, 원하면 결과를 저장한다.

### 건강 검진

```
/wiki-lint
```

깨진 링크, 고아 페이지, 인덱스 누락 등을 점검한다.

## 사용 예시

### 1. 소스 투입 (`/wiki-ingest`)

Karpathy의 LLM Wiki gist를 인제스트하는 예시:

```
사용자: /wiki-ingest https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

LLM:    소스를 읽었습니다. 핵심 요약:
        - RAG와 달리 LLM이 영구 위키를 점진적으로 구축·유지
        - 3계층: Raw sources → Wiki → Schema
        - 3가지 연산: Ingest, Query, Lint
        특별히 강조할 방향이 있을까요?

사용자: 전체적으로 정리해줘

LLM:    실행 계획:
        - 생성: wiki/sources/karpathy-llm-wiki.md (소스 요약)
        - 생성: wiki/entities/andrej-karpathy.md, wiki/entities/obsidian.md
        - 생성: wiki/concepts/llm-wiki-pattern.md, wiki/concepts/rag.md, wiki/concepts/memex.md
        - 업데이트: wiki/index.md, wiki/log.md

        ✓ 6개 페이지 생성, 인덱스 업데이트 완료
```

한 번의 인제스트로 소스 요약 1개, 엔티티 2개, 개념 3개가 생성되고, 모두 `[[wikilink]]`로 상호 연결된다.

### 2. 질의 (`/wiki-query`)

위키에 축적된 지식을 기반으로 질문에 답변:

```
사용자: /wiki-query RAG와 LLM Wiki의 차이가 뭐야?

LLM:    [[rag]]는 매 질의마다 원본에서 관련 청크를 검색하여 답변을 생성합니다.
        지식이 축적되지 않고, 매번 처음부터 재발견해야 합니다.

        반면 [[llm-wiki-pattern]]은 LLM이 영구 위키를 점진적으로 구축합니다.
        교차 참조가 미리 만들어져 있고, 모순은 이미 표시되어 있으며,
        소스가 추가될수록 위키가 복리로 성장합니다.

        이 답변을 위키에 저장할까요?

사용자: 응

LLM:    ✓ wiki/synthesis/rag-vs-llm-wiki.md로 저장 완료
```

답변이 마음에 들면 `wiki/synthesis/`에 저장하여 지식으로 축적할 수 있다.

### 3. 건강 검진 (`/wiki-lint`)

위키의 일관성을 점검:

```
사용자: /wiki-lint

LLM:    # Wiki Lint Report

        ## Summary
        - Total pages: 6
        - Broken links: 0
        - Missing from index: 0
        - Orphan pages: 0
        - Frontmatter issues: 0
        - Suspected contradictions: 0

        ✓ 모든 검사를 통과했습니다.
```

깨진 링크, 고아 페이지, 인덱스 누락, 프론트매터 오류, 내용 모순 등을 자동으로 감지하고 수정을 제안한다.

## Obsidian

이 레포지토리를 Obsidian vault로 열면 `[[wikilink]]` 그래프를 탐색할 수 있다.
- `raw/`: 읽기/쓰기 (Web Clipper 등으로 소스 수집)
- `wiki/`: 읽기 전용 (LLM만 수정)

### 권장 플러그인

| 플러그인 | 유형 | 용도 |
|---------|------|------|
| [Dataview](https://github.com/blacksmithgu/obsidian-dataview) | 커뮤니티 | frontmatter 기반 동적 테이블/리스트 (예: `status: needs-review` 페이지 목록) |
| [Web Clipper](https://obsidian.md/clipper) | 브라우저 확장 | 웹 기사를 마크다운으로 변환하여 `raw/`에 저장 |

#### 설치 방법

- **Dataview**: Obsidian → 설정 → 커뮤니티 플러그인 → 제한 모드 해제 → "Dataview" 검색 → 설치 → 활성화
- **Web Clipper**: [obsidian.md/clipper](https://obsidian.md/clipper)에서 브라우저에 맞는 확장 프로그램 설치 → 클리핑 저장 경로를 이 vault의 `raw/` 폴더로 지정하면 `/wiki-ingest`로 바로 투입할 수 있다

## License

이 프로젝트는 [MIT License](LICENSE)로 배포됩니다.

Copyright (c) 2026 Jihoon Jeon
