---
title: "routar: 프론트엔드 API 스키마 자동 관리 (2026-09-09)"
created: 2026-09-09
updated: 2026-09-09
type: concept
tags: [커리어전략, 인사이트]
sources: [raw/articles/routar-typed-http-20260909.md]
confidence: medium
contested: false
---

# routar: 프론트엔드 API 스키마 자동 관리

OpenAPI 스펙이 없고 백엔드와 계약을 공유하기 어려운 REST 프로젝트에서, 프론트엔드가 API 스키마를 직접 정의하고 타입세이프하게 관리하는 HTTP 클라이언트 라이브러리. 사이드 프로젝트로 개발되어 GeekNews에 공유됨.

## 핵심 내용

- **문제:** 백엔드 스펙 부재 시 프론트엔드에서 API 응답 타입을 수동으로 관리해야 하는 비효율
- **해결:** 프론트엔드에서 API 스키마를 정의하고 이를 기반으로 타입세이프 HTTP 클라이언트 자동 생성
- **의의:** [[producer-focus|생산자의 몰입]] 관점에서, 실무 문제를 직접 해결하는 도구를 만드는 사이드 프로젝트의 좋은 사례

## 관련 개념

- [[build-vs-buy-ai|Build vs Buy]] — 개발 도구 의사결정의 맥락에서, 자체 도구 제작의 가치 평가
- [[design-system|디자인 시스템 구축]] — 인프런 사례처럼, 점진적 적용과 실무 필요성에서 출발한 도구 구축