---
tags: [google, engineering-practices, code-review, documentation]
sources: [google-eng-practices-code-review-comments.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Google Engineering Practices

Google이 공개한 사내 엔지니어링 가이드 시리즈. https://google.github.io/eng-practices/ 에서 CC BY 3.0으로 배포된다. 가장 유명한 것은 **Code Review Developer Guide**.

## Code Review Developer Guide의 영향력

업계에서 가장 자주 인용되는 코드 리뷰 표준 중 하나. 다음 관행의 사실상 캐노니컬 출처:

- **"Nit:" 접두사 컨벤션** — 사소한 코멘트임을 명시
- **"Optional:" / "FYI:"** 라벨 — 비차단 표현
- **"What is the standard of code review?" 원칙** — "코드는 완벽해야 하는 게 아니라, 머지 후 시스템 건강이 개선되어야 한다"
- **Small CL 철학** — 리뷰 가능한 작은 단위로 변경 분할

## Conventional Comments와의 관계

[[conventional-comments]] 는 Google 가이드 이후에 등장한 외부 사양으로, **라벨을 더 풍부하게(12종)** 정의한다. 두 체계는 경쟁이라기보다 자매 관계:
- Google: 단순 (Nit / Optional / FYI), 사내 관행에 가까움
- Conventional Comments: 풍부 (praise/nitpick/suggestion/issue/question/...), 외부 도구 친화적

## 다른 Google 엔지니어링 문화 산출물과의 관계

- **Software Engineering at Google** (책, 2020) — 같은 사내 문화의 책 버전
- **Style Guides** (https://google.github.io/styleguide/) — 언어별 스타일 가이드 컬렉션
- **Site Reliability Engineering** 책 시리즈 — 운영 가이드

## 관련 페이지

- [[google-eng-practices-code-review-comments]] — 코멘트 작성 가이드 요약
- [[code-review-comment-conventions]] — Nit:/Optional:/FYI: 컨벤션의 정의
- [[conventional-comments]] — 외부 후속 표준
