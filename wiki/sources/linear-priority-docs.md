---
tags: [linear, issue-management, priority, product-design]
sources: [linear-priority-docs.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Linear Priority 공식 문서

[[linear-app]] 의 공식 문서. 우선순위 시스템 설계를 의도적으로 작게 유지한다는 철학이 명시되어 있다.

## 4단계 + 미설정

Linear가 제공하는 우선순위:
- **No priority** (미설정, 기본값)
- **Low**
- **Medium**
- **High**
- **Urgent**

[[bugzilla]] 의 P1–P5나 Atlassian의 Highest/High/Medium/Low/Lowest 와 비교하면 단순하지만, 이게 의도된 디자인.

## 디자인 철학 — 의도적 단순화

> "We don't have the option to set custom priorities or more granular priorities since it's easy to get carried away with specificity."

세분화된 우선순위는 결정 마찰을 늘리고 **수확 체감**을 일으킨다는 입장. 더 많은 단계가 필요하다고 느낀다면, 우선순위가 아니라 **워크플로 상태나 라벨**로 표현하라는 권고.

이는 [[bug-priority-vs-severity]] 논쟁에 대한 한 가지 답변이기도 하다 — Linear는 두 차원을 분리하는 대신 우선순위 하나로 합치고 그 단계 수를 최소화했다.

## 사용성 디테일

- 키보드 단축키: 이슈 선택 후 `P` → 단계 선택
- 정렬: 우선순위 기준 정렬 시 드래그앤드롭으로 같은 단계 내 순서 조정 가능 (글로벌 영구 저장)
- 미설정 항목은 정렬 시 마지막에 표시
- **Urgent** 설정 시 담당자에게 알림 + 이메일 자동 발송

## 비교 시사점

| 시스템 | 단계 | 차원 | 설계 입장 |
|--------|------|------|-----------|
| Bugzilla | P1–P5 + Severity 6단계 | 분리 | 정확성 우선 |
| Atlassian/Jira 기본 | 5단계 | 통합 (커스터마이즈 가능) | 유연성 |
| Linear | 4단계 | 통합 | 단순성 우선 |

## 관련 페이지

- [[linear-app]] — 도구 자체
- [[bug-priority-vs-severity]] — Linear가 의도적으로 분리하지 않는 이유
- [[bugzilla]] — 더 정밀한 분리주의의 반례
