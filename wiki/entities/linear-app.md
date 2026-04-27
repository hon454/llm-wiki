---
tags: [issue-management, tool, project-management, saas]
sources: [linear-priority-docs.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Linear

2019년 출시된 SaaS 이슈 트래커 / 프로젝트 관리 도구. Karri Saarinen, Tuomas Artman, Jori Lallo가 공동 창업. "빠르고 단순한 Jira 대안"을 표방하며 스타트업·중견 SaaS 회사들에 빠르게 확산.

## 디자인 철학

- **속도 우선**: 키보드 단축키 중심, 즉각 반응하는 UI
- **의도적 단순화**: 옵션을 줄여 결정 마찰을 낮춤 ([[linear-priority-docs]] 참조)
- **Opinionated**: 우선순위 4단계 + 미설정만 제공, 커스텀 우선순위 불가

## 우선순위 시스템

Linear는 [[bug-priority-vs-severity]] 의 분리 전통을 거부하고, 단일 우선순위 4단계로 통합:
- No priority / Low / Medium / High / Urgent

> "We don't have the option to set custom priorities or more granular priorities since it's easy to get carried away with specificity."

이는 [[bugzilla]] 의 P1–P5 + Severity 6단계 모델과 정반대 방향.

## 한국 사용자

한국 SaaS 개발자 사이에서 사내 이슈 트래킹 도구로 채택률이 높음.

## 관련 페이지

- [[linear-priority-docs]] — 공식 우선순위 문서 요약
- [[bug-priority-vs-severity]] — Linear가 분리하지 않는 이유
- [[bugzilla]] — 분리주의 진영의 원류
