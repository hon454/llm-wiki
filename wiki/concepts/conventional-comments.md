---
tags: [code-review, communication, specification, conventional-comments]
sources: [conventional-comments-spec.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# Conventional Comments

코드 리뷰 코멘트의 **의도와 차단 여부**를 라벨로 명시하는 표준 표기 사양. [Conventional Commits](https://www.conventionalcommits.org/) 사양에서 영감을 받아 2019년경 등장. CC BY 3.0 라이선스로 https://conventionalcomments.org/ 에 공개.

## 포맷

```
<label> [decorations]: <subject>

[discussion]
```

## 12개 라벨

코멘트의 **목적**을 분류:

| 라벨 | 한 줄 의미 |
|------|-----------|
| `praise` | 칭찬 |
| `nitpick` | 사소한 취향 차이의 요청 |
| `suggestion` | 개선 제안 |
| `issue` | 명확한 문제 지적 |
| `todo` | 작지만 꼭 필요한 변경 |
| `question` | 잠재적 우려, 확신 없음 |
| `thought` | 떠오른 생각 (행동 요구 아님) |
| `chore` | 머지 전 끝내야 할 잡일 |
| `note` | 항상 비차단, 알아두라는 뜻 |
| `typo` / `polish` / `quibble` | todo / suggestion / nitpick의 변형 |

## 3개 데코레이션

코멘트의 **차단 여부와 조건**을 명시:

| 데코레이션 | 의미 |
|-----------|------|
| `(blocking)` | 해결 전 머지 불가 |
| `(non-blocking)` | 머지 막지 않음 |
| `(if-minor)` | 변경이 작을 때만 처리 |

콤마로 결합 가능 — 도메인 태그(`ux`, `security`, `test`)와 함께 쓰는 경우가 많다:
- `issue (ux,non-blocking)`
- `suggestion (security)`
- `suggestion (test,if-minor)`

## 왜 라벨을 붙이는가

사양 본문이 직접 답한다:

> "Labeling comments encourages collaboration and saves hours of undercommunication and misunderstandings."

라벨이 없으면 모든 코멘트가 **암묵적 차단 요청**으로 해석될 수 있다. 작성자는 "이거 진짜 고쳐야 하나, 그냥 의견인가" 추측에 시간을 쓴다. 명시적 라벨은 그 비용을 제거한다.

## P0/P1 체계와의 관계

[[code-review-comment-conventions]] 에서 흔히 보이는 `P0`/`P1`/`P2` 라벨은 [[bug-priority-vs-severity]] 의 Priority 차원을 차용한 것. Conventional Comments는 같은 문제를 다른 방식으로 푼다:

| 접근 | 표현 |
|------|------|
| Priority 라벨 | `P0` / `P1` / `P2` (선형 등급) |
| Conventional Comments | 라벨(목적) + 데코레이션(차단성) 분리 |

Conventional Comments는 "차단 여부"와 "코멘트의 종류"를 직교 분리하므로 Priority 라벨보다 의미 손실이 적다. 하지만 라벨 종류가 12개라 학습 곡선이 있다.

## 하이브리드 사용

실무에서는 두 체계를 섞어 쓰는 경우도 흔하다:

```
P1 · Nit (strongly recommended follow-up)
```

- `P1` — 우선순위 (Bugzilla 계열)
- `Nit` — Conventional Comments 라벨
- `(strongly recommended follow-up)` — 데코레이션 변형

세 어휘를 합친 개인 표기법으로, 의미: "중요하지만 이번 PR을 막진 않겠다."

## 도입 사례

- Netlify (Feedback Ladders 가이드, 2020)
- Marmelab (2024 도입 후기)
- 일부 OSS 프로젝트 (CONTRIBUTING.md에 명시)

## 라이선스

Creative Commons CC BY 3.0 — 사양 자체를 자유롭게 차용·수정 가능.

## 관련 페이지

- [[conventional-comments-spec]] — 사양 원문 요약
- [[code-review-comment-conventions]] — 다양한 라벨 컨벤션 비교의 한 항목
- [[bug-priority-vs-severity]] — 이 사양이 우회하는 또 다른 분류 차원
