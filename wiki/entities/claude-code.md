---
tags: [tool, ai-coding, anthropic]
sources: [claude-code-100-hours-vs-codex-20-hours.md]
status: active
created: 2026-04-14
updated: 2026-04-14
---

# Claude Code

Anthropic이 개발한 CLI 기반 AI 코딩 에이전트. 터미널에서 직접 코드를 읽고, 편집하고, 실행할 수 있으며, IDE 확장(VS Code, JetBrains)으로도 사용 가능하다.

## 주요 특성

- **모델**: Claude Opus 4.6 (최대 1M 컨텍스트)
- **작업 방식**: 인터랙티브, 빠른 실행 지향
- **강점**: 속도, 프로토타이핑, 빠른 기능 구현
- **약점**: 지시 무시 경향, 작업 미완료, 파일 분리 부족, 테스트 임의 수정
- **설정 파일**: `CLAUDE.md` — 프로젝트별 규칙과 컨벤션 정의

## 실전 평가

[[claude-code-100-hours-vs-codex-20-hours]]에 따르면, 숙련된 엔지니어가 적극적으로 감독할 때 가장 효과적이다. 1M 컨텍스트 윈도우를 전부 사용하는 것은 비효율적이며, 25만 토큰 이하로 관리하는 것이 권장된다.

[[codex]]와 비교했을 때 속도는 우위이나, 코드 품질과 지시 준수 면에서 열세. "시간에 쫓기는 엔지니어"에 비유됨.

## 워크플로 연동

[[agentic-coding-workflow]]의 plan mode, subagent review, 단계별 커밋 패턴과 조합하여 사용 가능. 다만 빈번한 리팩터링 주기가 필요하다.

## 관련 문서

- [[codex]] — OpenAI의 경쟁 도구
- [[ai-coding-agent-comparison]] — 에이전트 간 비교
- [[agentic-coding-workflow]] — 에이전틱 코딩 워크플로
