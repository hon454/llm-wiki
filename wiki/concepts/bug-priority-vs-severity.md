---
tags: [bug-tracking, priority, severity, issue-management, triage]
sources: [bugzilla-understanding-a-bug.md, linear-priority-docs.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Priority vs Severity — 버그 트래킹의 두 차원

소프트웨어 결함을 분류할 때 **얼마나 나쁜가**(Severity)와 **언제 고칠 것인가**(Priority)는 원리상 독립된 두 차원이라는 사고 모델.

## 캐노니컬 정의 — Bugzilla

[[bugzilla]] 공식 문서의 한 문장이 이 구분의 정통 정의:

> "Priority directs work allocation, while Severity measures impact scope."

| 차원 | 질문 | 누가 결정 | 예시 값 |
|------|------|-----------|---------|
| **Severity** | 기술적으로 얼마나 나쁜가? | 보고자 / QA | blocker, critical, major, normal, minor, trivial |
| **Priority** | 언제 고칠 것인가? | 매니저 / 담당자 | P1, P2, P3, P4, P5 |

## 두 차원 분리의 실용적 의미

분리하면 다음 같은 조합이 자연스럽게 표현됨:

| Severity | Priority | 사례 |
|----------|----------|------|
| blocker | P3 | 거의 안 쓰는 기능에서 앱이 죽음 |
| trivial | P1 | 사소한 오타지만 CEO 데모에서 보임 |
| critical | P1 | 메인 플로우에서 데이터 손실 (전형적 incident) |
| minor | P5 | 다음 분기쯤 손볼 가벼운 UX 흠 |

분리하지 않으면 "P1 = 진짜 심각하고 급한 거"로 합치게 되고, "심각하지만 안 급함" / "안 심각하지만 급함" 같은 케이스를 표현할 어휘가 사라진다.

## 합치기 진영 — Linear

[[linear-app]] 은 의도적으로 두 차원을 합치고 단계도 5개로 줄였다 (No priority / Low / Medium / High / Urgent). 명시적 근거:

> "We don't have the option to set custom priorities or more granular priorities since it's easy to get carried away with specificity."

이 입장의 장점:
- 결정 마찰 감소
- 중복 분류 작업 감소
- 매니저-개발자 간 우선순위 합의가 쉬워짐

단점:
- "심각하지만 안 급함" 같은 케이스를 표현할 수 없음
- 예외는 라벨이나 워크플로 상태로 우회

## 코드 리뷰로의 차용

[[code-review-comment-conventions]] 에서 자주 보이는 `P0`, `P1`, `P2` 라벨은 이 Bugzilla식 Priority를 코드 리뷰 코멘트에 가져온 것이다. 다만 코드 리뷰에서는 보통:
- **P0** = 머지 차단 (blocking)
- **P1** = 강력 권고, 머지 전 또는 즉시 follow-up
- **P2** = nit / suggestion, 본인 판단

식으로 **Priority와 Severity를 다시 합쳐서** 사용한다. Bugzilla의 원래 분리 의도와는 다른 적용.

## 혼동의 일반적 원인

1. 두 차원의 정의를 모르는 사람이 둘을 같은 의미로 사용
2. 트래커마다 라벨 이름이 다름 (Jira의 "Priority"는 Bugzilla의 "Severity"에 가까움)
3. 인시던트 대응에서 둘이 거의 동시에 결정되어 시각적으로 같이 보임

## 관련 페이지

- [[bugzilla]] — 캐노니컬 분리 모델의 출처
- [[linear-app]] — 의도적 합치기 입장
- [[code-review-comment-conventions]] — 이 차원이 코드 리뷰로 차용된 양상
- [[conventional-comments]] — Priority/Severity 대신 라벨 + 데코레이션 (`blocking`/`non-blocking`)으로 표현하는 대안
