---
tags: [concept, llm, information-retrieval]
sources: [karpathy-llm-wiki.md]
status: active
created: 2026-04-14
updated: 2026-04-14
---

# RAG (Retrieval-Augmented Generation)

검색 증강 생성. 질의 시 관련 문서 청크를 검색하여 LLM에 제공하고 답변을 생성하는 방식.

## 작동 방식

1. 문서를 청크로 분할하고 벡터 임베딩으로 인덱싱
2. 질의가 들어오면 관련 청크를 검색
3. 검색된 청크를 컨텍스트로 LLM에 전달하여 답변 생성

NotebookLM, ChatGPT 파일 업로드 등 대부분의 문서 QA 시스템이 이 방식을 사용한다.

## 한계

[[andrej-karpathy]]가 [[llm-wiki-pattern]]에서 지적한 RAG의 근본적 한계:

- **비상태적**: 매 질의마다 처음부터 지식을 재발견해야 함. 축적이 없음
- **합성 어려움**: 5개 문서를 종합해야 하는 미묘한 질문에서 매번 관련 조각을 찾아 엮어야 함
- **교차 참조 부재**: 문서 간 연결이 사전에 구축되지 않음
- **모순 미감지**: 문서 간 충돌을 사전에 발견하지 못함

## LLM Wiki와의 비교

[[llm-wiki-pattern]]은 RAG의 "매번 재발견" 문제를 해결하기 위해 LLM이 영구 위키를 점진적으로 구축·유지하는 대안을 제시한다.
