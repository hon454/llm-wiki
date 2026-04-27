---
tags: [bug-tracking, bugzilla, severity, priority, issue-management]
sources: [bugzilla-understanding-a-bug.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Bugzilla "Understanding a Bug" 공식 문서

[[bugzilla]]의 5.2+ 공식 문서 중 한 페이지. 버그 트래커가 보여주는 모든 필드를 정의한다. 이 페이지가 중요한 이유는 **`P1`–`P5` 표기와 Severity/Priority 분리의 캐노니컬 정의 출처** 중 하나이기 때문.

## Importance — Priority와 Severity의 분리

Bugzilla는 두 필드를 명시적으로 구분한다:

### Priority 필드
- 기본값: **P1, P2, P3, P4, P5**
- 의미: "bug prioritization by assignees or managers" — **언제 처리할지** 결정
- 누가 설정: 담당자 또는 매니저

### Severity 필드
- 값 범위: **blocker → trivial** ("application unusable" → "minor cosmetic issue")
- 의미: 문제의 **기술적 심각도**
- enhancement(기능 요청)도 이 필드로 표시

### 핵심 구분 (Bugzilla 공식 문장)

> "Priority directs work allocation, while Severity measures impact scope."

이 한 문장이 [[bug-priority-vs-severity]] 의 정통 정의다. 두 차원은 서로 독립적이라:
- `Severity=blocker, Priority=P3` ("앱이 죽는 심각한 버그지만 거의 안 쓰는 기능이라 나중에")
- `Severity=trivial, Priority=P1` ("사소한 오타지만 CEO 데모에서 보이니까 즉시")

같은 조합도 정상이다.

## 그 외 주요 필드

- **Summary**: 한 줄 요약
- **Status / Resolution**: 상태(unconfirmed → fixed → verified)
- **Product / Component**: 계층 분류
- **Assigned To / QA Contact**: 담당자
- **Target Milestone**: 목표 버전
- **Dependencies**: depends-on / blocks 관계
- **Flags**: `?` (요청), `+` (긍정), `-` (부정) — 코드 리뷰 요청도 여기 사용

## Flags — 리뷰 요청의 원류

Bugzilla의 Attachment Flags는 "patch에 대해 리뷰를 요청"하는 데 쓰인다. `?` 로 리뷰를 청하고 `+`/`-` 로 승인/거부. 현대 GitHub의 PR Approve/Request changes의 조상격.

## 관련 페이지

- [[bug-priority-vs-severity]] — 두 필드를 분리하는 사고 모델
- [[bugzilla]] — 도구 자체
- [[code-review-comment-conventions]] — Bugzilla의 P 라벨이 코드 리뷰로 차용된 흐름
