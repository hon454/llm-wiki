---
tags: [tool, ai-coding, openai]
sources: [claude-code-100-hours-vs-codex-20-hours.md]
status: active
created: 2026-04-14
updated: 2026-04-14
---

# Codex (OpenAI)

OpenAI가 개발한 CLI 기반 AI 코딩 에이전트. GPT-5.4 모델을 사용하며, 터미널에서 코드 작성 및 수정을 수행한다.

## 주요 특성

- **모델**: GPT-5.4
- **작업 방식**: 비동기적, 신중하고 체계적
- **강점**: 코드 품질, 지시 준수(AGENTS.md 절대 무시 안 함), 자율적 리팩터링, 적절한 파일 팩터링
- **약점**: Claude Code 대비 3~4배 느림
- **설정 파일**: `AGENTS.md` — 프로젝트별 규칙 정의

## 실전 평가

[[claude-code-100-hours-vs-codex-20-hours]]에 따르면, "주니어급 시니어(5~6년 경력)"에 비유된다. 작업 도중 스스로 멈추고 코드를 더 깨끗하게 재작업하며, 때로는 저자가 생각하지 못한 개선을 자발적으로 수행한다.

[[claude-code]]와 비교했을 때 코드 품질과 자율성에서 우위이나, 속도에서 열세. "실행시키고 결과만 리뷰하면 되는" 수준의 신뢰도를 보여줌.

## 요금

Codex Pro ×5 플랜이 Claude ×20 플랜과 유사한 사용량 상한을 가짐.

## 관련 문서

- [[claude-code]] — Anthropic의 경쟁 도구
- [[ai-coding-agent-comparison]] — 에이전트 간 비교
- [[agentic-coding-workflow]] — 에이전틱 코딩 워크플로
