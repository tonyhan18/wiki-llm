---
title: GPT-6 Astra 능력 평가
created: 2026-09-08
updated: 2026-09-10
type: entity
tags: [인사이트]
sources: [raw/articles/gpt-6-astra-capabilities.md, raw/articles/build-vs-buy-ai.md, raw/articles/gpt6-astra-loop-transformer-20260910.md, raw/articles/reasoning-prefill-distillation-20260910.md]
confidence: high
---

# GPT-6 Astra 능력 평가

## 개요
2026년 9월 기준 GPT-6 Astra 모델의 다방면 능력 평가 결과. 한국어 추론 벤치마크 만점, 하네스(인프라)에 따른 성능 격차, 로봇 팔 제어, 실무 평가 지표 개선을 통해 모델 능력과 시스템 능력의 차이를 보여줌.

## 핵심 사실

### 수능 벤치마크 첫 만점
- 2026학년도 수능 LLM 평가에서 종합 450점 만점 (개인 개발자 운영 벤치마크)
- 국어, 수학 모든 선택과목, 영어, 한국사, 탐구 4과목 모두 정답
- 기존 모델들이 어려워했던 사회문화 8번 해결

### 하네스가 곧 제품
- 같은 GPT-6 Astra, 같은 ARC-AGI-3 High 설정에서 하네스에 따라 54.8% vs 99.9%
- 장기 실행 에이전트는 모델 + 메모리 + 도구 + 문맥 관리 + 제어 루프의 시스템 평가 필요
- 요청 간 이전 추론 상태 보존, 체계적 검증-수정 루프가 성능 격차를 만듦

### 로봇 팔 제어
- Robocurve 실험: 카메라 영상 → 로봇 팔 움직임 제어
- 블록 집어 그릇에 넣기: Astra 19/20 vs Claude Fable 5.1 8/20
- 회당 평균 2.5분, 호출 비용 $2.12

### AAII v4.2 평가 진화
- Artificial Analysis Intelligence Index v4.2: 복잡 업무 수행 + 장문 문서 분석 추가
- 에이전트 지식 업무, 전문 문서 추론 평가 포함
- 비공개 테스트 확대로 오염 방지

## 토큰 비용 현실 (2026-09-08 업데이트)
- OS월드 2.0 오프라인 72.6%, 오토메이션벤치 41.4%로 GPT-5.6 솔 능가 ^[raw/articles/gpt6-astra-token-cost-20260908.md]
- API 가격: 입력 $10/1M토큰, 출력 $50/1M토큰
- 에이전트형 코딩이 단순 채팅보다 최대 1,000배 토큰 소비 (스탠퍼드 연구)
- 가트너: 2028년 AI 코딩 비용이 개발자 연봉 초과 가능성
- 관련: [[gpt6-astra-token-economics-20260908|GPT-6 Astra 토큰 경제학]]

## 관계
- 관련 컨셉: [[taste-and-ai]] — AI 능력 향상이 인간의 안목 형성 기회를 어떻게 위협하는지와 연결
- 관련 컨셉: [[llm-cognitive-impact]] — 모델 능력 강화가 사회적 의존 심화와 맞물리는 지점
- 관련 컨셉: [[ai-org-transformation]] — 하네스(인프라)가 제품 경쟁력을 결정한다는 점에서 조직 인프라와 유사
- 관련 컨셉: [[build-vs-buy-ai]] — Claude Code에서 GPT-6 Astra를 메인으로 섞어 쓰는 사례; 하네스(인프라) 선택이 제품 경쟁력을 결정
- 관련 컨셉: [[science-vs-engineering-hype]] — AI 모델 능력 주장의 독립적 검증 필요성
- 관련 컨셉: [[gpt6-astra-token-economics-20260908]] — 토큰 비용 현실과 AI 코딩 비용 전망
- 관련 컨셉: [[gpt6-astra-loop-transformer-20260910]] — 루프형 트랜스포머 구조와 해석 가능성 저하 (2026-09-10 업데이트)
- 관련 컨셉: [[reasoning-prefill-distillation-20260910]] — 추론 프리필로 GPT-5.5 Pro 추론 패턴을 경량 모델에 전달, 하네스 수준 추론 관리 (2026-09-10 업데이트)