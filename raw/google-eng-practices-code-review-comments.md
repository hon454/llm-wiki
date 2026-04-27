---
source_url: https://google.github.io/eng-practices/review/reviewer/comments.html
fetched: 2026-04-27
note: Extracted summary of Google Engineering Practices "How to Write Code Review Comments" page. Original under Google open-source license (CC BY 3.0).
---

# How to Write Code Review Comments

## Summary

The guide emphasizes four core principles: "Be kind," "Explain your reasoning," balance between explicit directions and problem-pointing, and encourage developers to simplify code rather than merely explaining complexity.

## Courtesy

Reviewers should maintain respect while providing clear feedback. A critical distinction: comment on the code, not the person. The guide contrasts a poor approach ("Why did **you** use threads...") with a better one that focuses on the technical issue: "The concurrency model here is adding complexity to the system without any actual performance benefit."

## Explain Why

Providing reasoning behind comments helps developers understand the reviewer's intent and reference relevant best practices or code health improvements.

## Giving Guidance

"In general it is the developer's responsibility to fix a CL, not the reviewer's." However, reviewers should balance pointing out issues with offering guidance. The document notes that problem identification often promotes learning better than detailed solutions. Reviewers should also acknowledge code strengths, whether technical excellence or new insights gained.

## Label Comment Severity

Using explicit labels clarifies comment intent:

- **Nit:** Minor issues with low impact
- **Optional (or Consider):** Suggested but not required
- **FYI:** Informational; no action expected

Labels prevent misinterpretation of all comments as mandatory requirements.

## Accepting Explanations

When developers explain unclear code, they should rewrite it more clearly. Code review tool explanations don't serve future readers and are only appropriate when reviewing unfamiliar areas where normal readers would lack context.
