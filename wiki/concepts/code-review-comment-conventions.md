---
tags: [code-review, communication, conventions, labeling]
sources: [google-eng-practices-code-review-comments.md, conventional-comments-spec.md, bugzilla-understanding-a-bug.md, linear-priority-docs.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# 코드 리뷰 코멘트 라벨링 컨벤션

코드 리뷰에서 **코멘트의 의도(차단인가 권고인가, 필수인가 의견인가)** 를 명시하기 위한 라벨링 관행 전반. 산업 표준은 없고 여러 컨벤션이 공존.

## 왜 필요한가

라벨이 없는 리뷰 코멘트는 작성자가 매번 추측해야 한다:
- "이거 안 고치면 머지 못 하나?"
- "단순한 의견인데 강하게 들리는 건가?"
- "저자 톤이 차가워 보이는데 화난 건가?"

명시적 라벨은 이 추측 비용을 0으로 만든다. [[google-engineering-practices]] 가이드의 표현:

> "Labeling comments [prevents] all comments [from being] interpreted as mandatory requirements."

## 주요 컨벤션 4계열

### 1. Google 스타일 — 단순 3개

[[google-eng-practices-code-review-comments]] 에서 정의:

| 라벨 | 의미 |
|------|------|
| `Nit:` | 사소한 이슈, 영향 적음 |
| `Optional:` 또는 `Consider:` | 권고이지만 필수 아님 |
| `FYI:` | 단순 정보, 행동 불요 |

Google 사내 관행에서 출발해 가장 널리 퍼진 베이스 라인. 외부 도구 없이 텍스트 접두사만으로 작동.

### 2. Conventional Comments — 풍부한 12 + 3

[[conventional-comments]] 사양:
- 12개 라벨 (`praise`, `nitpick`, `suggestion`, `issue`, `todo`, `question`, `thought`, `chore`, `note` + 변형 3개)
- 3개 데코레이션 (`blocking`, `non-blocking`, `if-minor`)

Google 단순 3종이 표현하지 못하는 차원(차단 여부 vs 코멘트 종류 분리)을 명시화. 학습 곡선 있음.

### 3. P0/P1/P2 — Bugzilla에서 차용

[[bug-priority-vs-severity]] 의 Priority 차원을 코드 리뷰로 가져온 형태:

| 라벨 | 일반적 의미 |
|------|-------------|
| `P0` | 머지 차단, 즉시 수정 필수 |
| `P1` | 강력 권고, 머지 전 또는 즉시 follow-up |
| `P2` | nit / 의견, 본인 판단 |
| `P3` | 시간 날 때 |
| `P4` | wishlist (거의 안 씀) |

원래 [[bugzilla]] 의 Priority는 P1부터 시작하지만 (`P0` 없음), 코드 리뷰에서는 "최우선 = P0" 관행이 추가되었다 (Google 사내 영향 추정).

**주의**: 같은 `P1` 이라도 팀마다 정의가 다르다. 팀 가이드에 명시 필요.

### 4. 이모지 / 커스텀 약어

일부 팀:
- 🔴 = blocker, 🟡 = nit, 🟢 = praise, 💭 = thought
- `[!]` = blocker, `[?]` = question, `[+]` = praise

표준 없음. 팀 내부 합의로 동작.

## 하이브리드 표기

실무에서 위 체계들이 섞이는 경우가 흔하다:

```
P1 · Nit (strongly recommended follow-up)
```

읽는 법:
- `P1` (Bugzilla 계열) — 우선순위 높음
- `Nit` (Conventional Comments) — 비차단 라벨
- 괄호 — "강력 권고하지만 이번 PR을 막진 않겠다"

세 라벨이 모순적으로 보이지만, **"중요하다는 신호 + 비차단성 + 후속 PR 가능"** 을 한 줄에 압축한 개인 표기법으로 해석 가능.

## 컨벤션 비교

| 차원 | Google | Conventional Comments | P0–P4 |
|------|--------|----------------------|-------|
| 라벨 수 | 3 | 12 + 3 데코 | 5 |
| 차단 여부 표현 | 명시 안 함 (Nit이 암묵적 비차단) | 명시 (`blocking`/`non-blocking`) | 명시 (P0/P1 = 차단) |
| 코멘트 의도 표현 | 약함 | 강함 (라벨로 분류) | 없음 (우선순위만) |
| 학습 곡선 | 낮음 | 중간 | 낮음 |
| 도구 친화성 | 텍스트 검색만 | 정규식 파싱 가능 | 정규식 파싱 가능 |

## AI 코드 리뷰어와의 관계

[[ai-coding-agent-comparison]] 시대에 들어 CodeRabbit, Greptile, Gemini Code Assist 등 자동 리뷰어가 `Critical/High/Medium/Low` 또는 `P0/P1/P2` 라벨을 자동 출력하는 흐름이 표준화되고 있다. 결과적으로 `P0–P4` 표기는 사람-AI 공통 어휘로 굳어지는 중.

## 권장 — 팀이 채택할 때

1. **하나만 골라서 일관되게 사용**. 섞으면 추측 비용 다시 발생
2. **CONTRIBUTING.md에 명시**. 신규 멤버 위해
3. **라벨 정의를 짧게 유지**. P0–P4를 5문장 이상 정의하면 아무도 안 봄
4. **Conventional Comments의 `blocking`/`non-blocking` 데코레이션은 어떤 체계와도 결합 가능** — 차단 여부만은 항상 명시

## 관련 페이지

- [[google-eng-practices-code-review-comments]]
- [[conventional-comments]]
- [[bug-priority-vs-severity]]
- [[bugzilla]] / [[linear-app]] — P 라벨 vs 단순화 진영
