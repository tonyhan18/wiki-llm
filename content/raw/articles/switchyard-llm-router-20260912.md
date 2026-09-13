# Switchyard: OpenAI/Anthropic API 그대로 모델을 바꿔 쓰는 LLM 라우터

**소스:** GeekNews (https://news.hada.io/topic?id=33566)
**수집일:** 2026-09-12

## 요약
NVIDIA NeMo에서 공개한 LLM 라우터 Switchyard. 모든 요청을 비싼 모델에 맡기는 대신 작업을 처리할 수 있는 더 저렴한 모델을 선택하는 것이 목표.

## 핵심 포인트
- OpenAI Chat Completions/Responses와 Anthropic Messages 형식 지원
- 기존 클라이언트의 연결 주소를 프록시로 바꾸기만 하면 됨
- 라우팅을 통한 비용 최적화: 작업 복잡도에 따라 모델 선택
- 드롭인 교체(drop-in replacement) 방식으로 기존 코드 수정 불필요