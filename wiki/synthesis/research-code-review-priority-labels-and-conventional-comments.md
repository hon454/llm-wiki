---
tags: [research, code-review, priority, severity, conventional-comments, labeling]
sources: [google-eng-practices-code-review-comments.md, conventional-comments-spec.md, bugzilla-understanding-a-bug.md, linear-priority-docs.md]
status: active
created: 2026-04-27
updated: 2026-04-27
---

# 코드 리뷰 우선순위 라벨과 Conventional Comments 리서치

## 개요
- 모드: 탐색형
- 에이전트: 3개 (academic / practitioner / trends)
- 수집: 20건 / 선별: 4건 (빠르게 모드)

## 수집 소스 전체 목록

| # | ★ | 제목 | URL | 선별 |
|---|---|------|-----|------|
| 1 | ★★★ | How to write code review comments (Google) | https://google.github.io/eng-practices/review/reviewer/comments.html | ✓ |
| 2 | ★★★ | The Standard of Code Review (Google) | https://google.github.io/eng-practices/review/reviewer/standard.html | - |
| 3 | ★★★ | Conventional Comments | https://conventionalcomments.org/ | ✓ |
| 4 | ★★★ | Jira Service Management — Priority levels | https://support.atlassian.com/jira-service-management-cloud/docs/what-are-priority-levels-in-jira-service-management/ | - |
| 5 | ★★★ | Jira Cloud — Manage priorities | https://support.atlassian.com/jira-cloud-administration/docs/manage-priorities/ | - |
| 6 | ★★★ | Bugzilla — Understanding a Bug | https://bugzilla.readthedocs.io/en/latest/using/understanding.html | ✓ |
| 7 | ★★★ | Bugzilla — Field Values | https://bugzilla.readthedocs.io/en/latest/administering/field-values.html | - |
| 8 | ★★★ | Linear — Priority | https://linear.app/docs/priority | ✓ |
| 9 | ★★☆ | Feedback Ladders — Netlify | https://www.netlify.com/blog/2020/03/05/feedback-ladders-how-we-encode-code-reviews-at-netlify/ | - |
| 10 | ★★☆ | Good Code Reviews — Pragmatic Engineer | https://blog.pragmaticengineer.com/good-code-reviews-better-code-reviews/ | - |
| 11 | ★★☆ | Code Review Comment Prefixes — C. Emmer | https://emmer.dev/blog/code-review-comment-prefixes/ | - |
| 12 | ★★☆ | Great Code Reviews — Shopify | https://shopify.engineering/great-code-reviews | - |
| 13 | ★★☆ | How to review code effectively — GitHub | https://github.blog/developer-skills/github/how-to-review-code-effectively-a-github-staff-engineers-philosophy/ | - |
| 14 | ★★☆ | My Case for Conventional Comments — A. Bos | https://aaronbos.dev/posts/case-for-conventional-comments | - |
| 15 | ★☆☆ | Don't Confuse Priority with Severity (HN) | https://news.ycombinator.com/item?id=22422570 | - |
| 16 | ★☆☆ | P labels for issues — Storybook discussion | https://github.com/storybookjs/storybook/discussions/14490 | - |
| 17 | ★☆☆ | Conventional Comments: Stop Fighting (DEV) | https://dev.to/this-is-learning/conventional-comments-stop-fighting-in-code-reviews-nia | - |
| 18 | ★★☆ | Crystal Clear Reviews — Marmelab | https://marmelab.com/blog/2024/01/05/crystal-clear-reviews-with-conventional-comments.html | - |
| 19 | ★★☆ | State of AI Code Review Tools 2025 | https://www.devtoolsacademy.com/blog/state-of-ai-code-review-tools-2025/ | - |
| 20 | ★★☆ | Blocking Comments vs Nitpicks — Graphite | https://graphite.dev/guides/blocking-comments-vs-nitpicks | - |

## 핵심 발견

### 발견 1: P0/P1 표기는 산업 표준이 아니다
[[bugzilla]] 의 Priority 필드(P1–P5)에서 출발해 Google 사내 등에서 "P0" 가 추가되며 퍼졌지만, **공식 표준은 없다**. 같은 `P1` 도 팀마다 정의가 다르다 ([[code-review-comment-conventions]] 참조).

### 발견 2: Priority와 Severity는 원리상 다른 차원
[[bug-priority-vs-severity]] — Bugzilla 공식 정의가 가장 명료:
> "Priority directs work allocation, while Severity measures impact scope."

분리하면 "심각하지만 안 급함" 같은 케이스를 표현할 수 있다. 합치면 마찰이 줄지만 표현력이 손실된다.

### 발견 3: 합치기 진영의 대표가 Linear
[[linear-app]] 은 "세분화 옵션을 의도적으로 제거"한다. No priority + 4단계만. [[bugzilla]] 의 P1–P5 + Severity 6단계와 정반대 디자인 결정.

### 발견 4: 코드 리뷰는 두 차원을 다시 합쳐서 차용
[[code-review-comment-conventions]] 에서 흔한 P0/P1/P2는 Bugzilla의 Priority 차원을 가져오면서, **Severity와 합쳐서** 하나의 "심각도+긴급도 합산" 라벨로 사용한다. Bugzilla의 원래 분리 의도와는 어긋나는 적용.

### 발견 5: Conventional Comments는 다른 축으로 푼다
[[conventional-comments]] 는 P0–P4 같은 선형 등급 대신 **라벨(목적) + 데코레이션(차단성)** 으로 직교 분리한다. `praise`/`nitpick`/`suggestion`/`issue` 등 12 라벨 + `(blocking)`/`(non-blocking)`/`(if-minor)` 3 데코.

### 발견 6: 하이브리드 표기는 흔하다
실무 PR 리뷰에서 `P1 · Nit (strongly recommended follow-up)` 같은 세 어휘 결합이 종종 보인다. 이는:
- `P1` (Bugzilla 계열 우선순위)
- `Nit` (Conventional Comments / Google 라벨)
- `(...)` (데코레이션 변형)

세 체계의 표현력을 한 줄에 압축한 개인 표기법이다.

## 소스별 요약

### [[google-eng-practices-code-review-comments]]
Google 사내 코드 리뷰 가이드. `Nit:` / `Optional:` / `FYI:` 라벨 컨벤션의 정통 출처. "코드에 대해 말하라, 사람에 대해 말하지 말라" 원칙 등 톤 가이드 포함.

### [[conventional-comments-spec]]
12개 라벨 + 3개 데코레이션을 정의하는 캐노니컬 사양. CC BY 3.0. `<label> [decorations]: <subject>` 포맷. 도구 친화적 (정규식 파싱 가능).

### [[bugzilla-understanding-a-bug]]
Priority(P1–P5)와 Severity(blocker→trivial) 분리의 캐노니컬 정의 출처. 현대 코드 리뷰의 P0/P1 라벨이 차용한 원류. Attachment Flag(`?`/`+`/`-`) 는 PR Approve의 조상격.

### [[linear-priority-docs]]
의도적 단순화 입장의 대표. 4단계 + 미설정만 제공, 커스텀 우선순위 불가. "specificity에 휩쓸리기 쉽다"는 디자인 철학 명시.

## 종합 — 사용자 질문 ("P0/P1이 일반적인 등급제인가?") 에 대한 답변

**아니다, 공식 표준은 없다.** 다만 다음 세 흐름이 사실상의 어휘를 형성:

1. **Bugzilla 계열** (P1–P5 + 분리된 Severity) — 정확성 우선의 분리주의
2. **Google 단순 라벨** (`Nit:`/`Optional:`/`FYI:`) — 텍스트 접두사로 강도만 표현
3. **Conventional Comments** (12 라벨 + 3 데코) — 목적과 차단성을 직교 분리

받으신 리뷰의 `P1 · Nit (strongly recommended follow-up)` 은 세 흐름을 모두 차용한 하이브리드. 의미는 "중요한 누락이지만 머지 막진 않겠다"로 해석되며, 리뷰어 ryumiel의 개인 표기 스타일.

**팀에서 도입한다면**: 하나만 골라 일관되게. 섞으면 추측 비용이 다시 생긴다. 차단 여부만은 항상 명시 (Conventional Comments의 `(blocking)`/`(non-blocking)` 데코는 어떤 체계와도 결합 가능).
