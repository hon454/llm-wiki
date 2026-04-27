# Wiki Log

## [2026-04-14] ingest | Karpathy의 LLM Wiki
Karpathy의 LLM Wiki gist를 인제스트. 소스 요약 1개, 엔티티 2개(andrej-karpathy, obsidian), 개념 3개(llm-wiki-pattern, rag, memex) 생성. 인덱스 업데이트 완료.

## [2026-04-14] ingest | Claude Code vs Codex 실전 비교
Reddit r/ClaudeCode 경험담 인제스트. 소스 요약 1개, 엔티티 2개(claude-code, codex), 개념 2개(agentic-coding-workflow, ai-coding-agent-comparison) 생성. 인덱스 업데이트 완료.

## [2026-04-14] lint | 정기 검진
전체 11개 페이지 검사 완료. 깨진 링크 0, 인덱스 누락 0, 고아 페이지 0, 프론트매터 이슈 0, 모순 의심 0. 위키 건강 상태 양호.

## [2026-04-27] research | code review priority labels and conventional comments
metrics: sources_collected=20, sources_processed=4, agents=3
탐색형 리서치, 에이전트 3개(academic/practitioner/trends), 20건 수집 후 사용자 "빠르게" 선택으로 4건 선별 인제스트 (Google eng-practices, Conventional Comments spec, Bugzilla "Understanding a Bug", Linear Priority docs). 소스 4, 엔티티 3(bugzilla, linear-app, google-engineering-practices), 개념 3(code-review-comment-conventions, bug-priority-vs-severity, conventional-comments), 종합 1건 생성.

## [2026-04-27] lint | research 후 검진
metrics: pages_total=15, broken_links=2, missing_from_index=0, orphans=0, frontmatter_issues=0, slug_conflicts=0, missing_source_refs=0, adr_alerts=0
이번 리서치로 추가된 14개 페이지 + 기존 11개 = 25개 (단, 본 커밋에는 이전 세션의 미커밋 페이지 제외) 검사. 2건 깨진 링크 발견·수정: `[[conventional-commits]]`(존재하지 않는 페이지)는 외부 링크로 전환, `[[user_profile]]`(위키 외부 메모리 파일)은 위키링크 제거. ADR-0003·ADR-0004 임계값 여유.
