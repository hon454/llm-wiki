---
tags: [code-review, google, engineering-practices, communication]
sources: [google-eng-practices-code-review-comments.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Google 코드 리뷰 코멘트 작성 가이드

[[google-engineering-practices]] 시리즈의 일부로, Google 사내 코드 리뷰에서 리뷰어가 코멘트를 어떻게 작성해야 하는지 규정한 공식 문서.

## 핵심 원칙 4가지

1. **친절할 것 (Be kind)**
2. **이유를 설명할 것 (Explain your reasoning)**
3. **명시적 지시 vs 문제 지적의 균형**
4. **복잡함을 설명하기보다 코드를 단순화하도록 유도할 것**

## 코드에 대해 말하라, 사람에 대해 말하지 말라

> 나쁜 예: "Why did **you** use threads here..."
> 좋은 예: "The concurrency model here is adding complexity to the system without any actual performance benefit."

리뷰어는 사람의 의도를 추궁하는 대신 **기술적 사실**을 지적해야 한다.

## 코멘트 심각도 라벨링 (Label Comment Severity)

이 문서가 [[code-review-comment-conventions]]에서 가장 자주 인용되는 부분. 모든 코멘트가 머지 차단으로 오해되지 않도록 라벨을 명시할 것을 권한다:

- **Nit:** 영향이 적은 사소한 이슈 (수정해도 좋고 안 해도 됨)
- **Optional (또는 Consider):** 권고이지만 필수 아님
- **FYI:** 단순 정보 공유, 행동 불요

라벨이 없으면 모든 코멘트가 "필수 수정"으로 해석되어 마찰이 생긴다.

## 가이던스 제공 원칙

> "In general it is the developer's responsibility to fix a CL, not the reviewer's."

리뷰어는 해결책을 직접 쓰기보다 **문제를 가리키는 것**이 학습 효과가 더 크다고 말한다. 단, 강점도 인정할 것.

## 설명 받아들이기

코드가 불명확해서 작성자가 PR 댓글로 설명한다면, 그 설명은 미래 독자에게 도달하지 않는다. **코드 자체를 더 명확하게 다시 쓸 것**.

## 관련 페이지

- [[code-review-comment-conventions]] — Nit:/Optional:/FYI: 라벨 컨벤션의 원류
- [[conventional-comments]] — 대안적 라벨 표준 (Google 가이드 이후 등장한 외부 사양)
- [[bug-priority-vs-severity]] — 코드 리뷰의 P0/P1 라벨이 차용한 원류 개념
