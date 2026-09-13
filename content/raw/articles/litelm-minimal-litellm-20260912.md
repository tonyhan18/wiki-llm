# Litelm - 비대함을 덜어낸 LiteLLM

**소스:** GeekNews (https://news.hada.io/topic?id=33580)
**수집일:** 2026-09-12

## 요약
litelm은 LiteLLM의 모델 라우팅과 메시지 변환 등 호출 경로만 추려 약 2,900줄로 구현한 경량 라이브러리. 기본 의존성은 openai, httpx 2개만. 19개 제공자를 provider/model 형식으로 호출하며 스트리밍을 지원.

## 핵심 포인트
- LiteLLM의 핵심 기능(모델 라우팅, 메시지 변환)만 추려 경량화
- 약 2,900줄 구현 — 원본 LiteLLM의 비대함 대비 극소화
- 기본 의존성 2개: openai, httpx
- 19개 제공자를 `provider/model` 형식으로 통일 호출
- 스트리밍 지원
- 비용 최적화 및 모델 선택 유연성 목적