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
