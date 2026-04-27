---
tags: [bug-tracking, tool, mozilla, open-source]
sources: [bugzilla-understanding-a-bug.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Bugzilla

Mozilla 재단이 1998년부터 개발해 온 오픈소스 버그 트래킹 시스템. Mozilla, Apache, GNOME, Linux 커널 등 대형 오픈소스 프로젝트의 표준 트래커였다.

## 코드 리뷰 라벨링 역사에서의 위치

[[bug-priority-vs-severity]] 분리 모델의 **사실상 캐노니컬 정의 출처**. 공식 문서가 두 필드를 명시적으로 구분한다:

> "Priority directs work allocation, while Severity measures impact scope."

- **Priority**: P1–P5 (담당자가 언제 처리할지 결정)
- **Severity**: blocker / critical / major / normal / minor / trivial / enhancement (문제의 기술적 심각도)

오늘날 코드 리뷰에서 흔히 보이는 `P0`, `P1`, `P2` 라벨은 Bugzilla의 Priority 필드 표기를 차용한 것이다. (Bugzilla 자체는 P0이 없고 P1부터 시작하지만, "최우선 = P0" 관행이 Google 사내 등에서 추가되었다.)

## 기술 스택

- Perl로 작성
- MySQL/PostgreSQL/SQLite 백엔드
- Mozilla Public License 2.0

## 영향력

- **GitHub Issues, GitLab Issues, Linear, Jira** 등 현대 트래커 모두 Bugzilla의 필드 모델을 직간접으로 계승
- Attachment Flag (`?`/`+`/`-`)는 현대 PR Approve / Request Changes의 조상격
- "Component", "Milestone", "Severity vs Priority" 같은 용어는 Bugzilla가 대중화

## 관련 페이지

- [[bug-priority-vs-severity]]
- [[bugzilla-understanding-a-bug]] — 공식 문서 요약
- [[linear-app]] — 우선순위를 의도적으로 단순화한 후속 세대
