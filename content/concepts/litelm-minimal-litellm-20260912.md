---
title: "Litelm: 비대함을 덜어낸 LiteLLM 경량 구현 (2026-09-12)"
created: 2026-09-12
updated: 2026-09-12
type: concept
tags: [인사이트]
sources: [raw/articles/litelm-minimal-litellm-20260912.md]
confidence: high
---

# Litelm: 비대함을 덜어낸 LiteLLM 경량 구현

## 정의
litelm은 LiteLLM의 핵심 기능인 모델 라우팅과 메시지 변환 호출 경로만 추려 약 2,900줄로 구현한 경량 LLM 통합 라이브러리. 기본 의존성 openai, httpx 2개만으로 19개 제공자를 `provider/model` 형식으로 통일 호출하며 스트리밍을 지원.

## 핵심 내용
- **경량화 철학:** 원본 LiteLLM의 비대함을 덜어내고 핵심 호출 경로만 유지
- **의존성 최소화:** openai, httpx 2개만 — 설치·유지보수 부담 최소
- **19개 제공자 통합:** `provider/model` 형식으로 통일된 호출 인터페이스
- **스트리밍 지원:** 실시간 응답 처리 기능 포함
- 약 2,900줄 — 전체 기능 대비 극소화된 코드베이스

## 의미
[[switchyard-llm-router-20260912]]의 라우팅 최적화와 같은 맥락에서, LLM 호출 인프라의 경량화·단순화 흐름. Switchyard가 라우팅을 통한 비용 최적화라면, litelm은 라이브러리 자체의 경량화로 개발자 진입 장벽을 낮춤. [[rtk-token-cost-benchmark-20260912]]의 토큰 비용 문제와 [[gpt6-astra-token-economics-20260908]]의 에이전트 토큰 폭증 시대에, 호출 인프라의 효율화는 비용 관리의 기본 층. [[build-vs-buy-ai]]의 "3년 차에 누가 패치하는가" 질문과 연결: 의존성 2개·2,900줄은 유지보수 부담을 극적으로 낮춰 Build 경로의 매력도를 높임.

## 쟁점
- 경량화 vs 기능 트레이드오프: 2,900줄에서 제외된 LiteLLM 기능들의 영향
- 19개 제공자 coverage가 실제 프로덕션 사용에 충분한가
- 원본 LiteLLM과의 호환성 수준
- 경량 라이브러리 생태계: 단일 개발자 의존도 리스크

^[raw/articles/litelm-minimal-litellm-20260912.md]