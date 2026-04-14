---
tags: [ai-coding, comparison, developer-experience]
sources: [claude-code-100-hours-vs-codex-20-hours.md]
status: active
created: 2026-04-14
updated: 2026-04-14
---

# Claude Code (~100시간) vs. Codex (~20시간)

Reddit r/ClaudeCode에 올라온 실전 비교 경험담. 14년 경력 시니어 엔지니어(MAG7 + 대형 테크 기업, Principal/Staff Eng Manager급)가 80k LOC Python/TypeScript 프로젝트(데이터 분석 애플리케이션, PostgreSQL, WebSocket, SSE)에서 [[claude-code]](Opus 4.6)와 [[codex]](GPT-5.4)를 비교했다.

## 프로젝트 맥락

- VSCode Extensions 기반, 테스트 약 2,800개
- PDF/CSV/XML 파싱 → 정규화된 데이터 모델 → 실시간 스트리밍 분석
- "바이브 코딩"이 아닌, 강한 아키텍처 기반의 co-developing

## 공통 에이전틱 워크플로

저자는 양쪽 도구에 동일한 [[agentic-coding-workflow]]를 적용했다:

- **Plan mode**: 충분히 스코프된 프롬프트로 계획 수립
- **Subagent review**: 계획 단계에서 8개 서브에이전트(아키텍처, 코딩 표준, UI 설계, 성능 등)로 리뷰
- **참조 문서**: 사전 연구 세션에서 만든 문서(`postgres_performance.md`, `software_architecture.md` 등)를 서브에이전트에 명시적으로 전달
- **단계별 커밋 + 코드 리뷰**: 계획의 각 단계를 별도 커밋하고, 서브에이전트 기반 코드 리뷰 스킬 실행
- **CLAUDE.md / AGENTS.md**: ~100줄, TDD, Git 워크플로, DevEx 컨벤션, Docker 명령어 등 명시

## Claude Code 경험 (Opus 4.6)

| 항목 | 평가 |
|------|------|
| 속도 | 빠름 |
| 자율성 | 낮음 — 감독(babysitting) 필요 |
| 코드 품질 | "시간에 쫓기는 엔지니어" 수준, 핵 패치와 헬퍼 함수 남발 |
| 지시 준수 | CLAUDE.md를 세션당 최소 1회 이상 무시 |
| 작업 완결성 | 반빈번하게 작업을 중간에 방치 (예: 8개 테스트 스위트 중 일부만 마이그레이션) |
| 파일 관리 | 새 기능을 새 파일로 분리하지 않고 기존 파일에 함수 추가 선호 |
| 테스트 처리 | 깨진 테스트를 임의로 수정하는 경향 (5%가 깨진 동작을 고정) |
| 컨텍스트 | 1M 컨텍스트는 "초보자 함정", 25만 이하로 유지해야 효과적 |

## Codex 경험 (GPT-5.4)

| 항목 | 평가 |
|------|------|
| 속도 | Claude 대비 3~4배 느림 |
| 자율성 | 높음 — "실행 후 결과만 리뷰" 가능 |
| 코드 품질 | 더 신중하고 체계적, 중간에 스스로 리팩터링 |
| 지시 준수 | AGENTS.md를 한 번도 무시하지 않음, 세션 중 오버라이드도 거부 |
| 작업 완결성 | 높음 |
| 파일 관리 | 자동으로 적절히 팩터링 |
| 독창성 | 때때로 저자가 생각하지 못한 개선을 자발적으로 추가 |

## 핵심 결론

- **속도 vs 품질 트레이드오프**: Claude로 세션당 더 많이 완수하지만, Codex 결과물이 더 깨끗함
- **사용 시나리오**: 프로토타이핑/빠른 빌드 → Claude, 엔터프라이즈급 소프트웨어 → Codex
- **요금**: Codex Pro ×5 ≈ Claude ×20 수준의 사용량 상한
- **필수 조건**: 두 도구 모두 SWE 역량 없이는 좋은 결과를 내지 못함
- Claude는 빈번한 리팩터링 주기가 필요하고("쓰레기 청소"), Codex는 앱 성장에 따른 자연스러운 리팩터링("성장에 따른 정리")

## 관련 문서

- [[ai-coding-agent-comparison]] — AI 코딩 에이전트 간 트레이드오프 분석
- [[agentic-coding-workflow]] — 에이전틱 코딩 워크플로 패턴
