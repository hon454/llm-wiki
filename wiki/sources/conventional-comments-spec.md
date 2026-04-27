---
tags: [code-review, communication, specification, conventional-comments]
sources: [conventional-comments-spec.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Conventional Comments 사양

[[conventional-comments]] 라는 코드 리뷰 코멘트 표기 표준의 캐노니컬 사양 문서. CC BY 3.0 라이선스로 공개되어 있다.

## 포맷

```
<label> [decorations]: <subject>

[discussion]
```

라벨과 데코레이션을 명시하면 리뷰 코멘트의 **의도와 차단 여부**를 한눈에 구분할 수 있다.

## 라벨 12종

| 라벨 | 정의 (사양 인용) |
|------|------------------|
| `praise` | "Praises highlight something positive." |
| `nitpick` | "Nitpicks are trivial preference-based requests." |
| `suggestion` | "Suggestions propose improvements to the current subject." |
| `issue` | "Issues highlight specific problems with the subject under review." |
| `todo` | "TODO's are small, trivial, but necessary changes." |
| `question` | "Questions are appropriate if you have a potential concern but are not quite sure." |
| `thought` | "Thoughts represent an idea that popped up from reviewing." |
| `chore` | "Chores are simple tasks that must be done before acceptance." |
| `note` | "Notes are always non-blocking and highlight something to take note of." |
| `typo` | typo는 todo의 변형 (오타 전용). |
| `polish` | polish는 suggestion의 변형 (품질 향상). |
| `quibble` | quibble은 nitpick의 변형. |

## 데코레이션 3종

| 데코레이션 | 정의 |
|-----------|------|
| `(non-blocking)` | "Should not prevent acceptance under review." |
| `(blocking)` | "Should prevent subject acceptance until resolved." |
| `(if-minor)` | "Resolve only if changes are minor or trivial." |

데코레이션은 콤마로 여러 개 결합 가능 (예: `issue (ux,non-blocking):`).

## 사양에 실린 예시

- `suggestion: This is not worded correctly. Can we change this to match the wording of the marketing page?`
- `issue (ux,non-blocking): These buttons should be red, but let's handle this in a follow-up.`
- `question (non-blocking): At this point, does it matter which thread has won?`
- `suggestion (security): I'm concerned implementing our own DOM purifying function…`
- `suggestion (test,if-minor): Missing unit test coverage…`
- `nitpick: \`little star\` => \`little bat\``
- `chore: Let's run the \`jabber-walk\` CI job…`
- `praise: Beautiful test!`

## 도입 이유 (사양 본문)

> "Labeling comments encourages collaboration and saves hours of undercommunication and misunderstandings."

라벨 없는 리뷰 코멘트는 작성자가 **의도를 추측**해야 한다. 명시적 라벨은 그 추측 비용을 제거한다.

## 라이선스

Creative Commons CC BY 3.0 — 자유롭게 인용·차용 가능.

## 관련 페이지

- [[conventional-comments]] — 이 사양이 정의하는 프레임워크 자체
- [[code-review-comment-conventions]] — Conventional Comments를 포함한 코멘트 라벨링 관행 전반
- [[google-eng-practices-code-review-comments]] — Google이 제안한 더 단순한 라벨 체계 (Nit:/Optional:/FYI:)
