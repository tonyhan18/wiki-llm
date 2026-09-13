---
title: 에이전트 실행 기록과 로컬 우선 워크스페이스
created: 2026-09-08
updated: 2026-09-08
type: concept
tags: [커리어전략, 인사이트]
sources: [raw/articles/apache-maka-agent-workspace.md]
confidence: medium
---

# 에이전트 실행 기록과 로컬 우선 워크스페이스

## 개념
Apache Maka는 AI 에이전트가 수행한 작업을 실행 단계별로 기록하고, 그 기록을 재사용·복구·표시에 활용하는 **로컬 우선 워크스페이스**다. 에이전트의 '무엇을 했는가'를 투명하게 추적하는 것이 핵심이다.

## 핵심 원칙

### 1. 실행 기록 (Agent Log)
- 대화, 도구 호출, 권한 승인, 종료 상태를 순서대로 기록
- 블랙박스가 아닌 **투명한 실행 추적**
- 비정상 종료 후 기록 기반 복구 지원

### 2. 로컬 우선 (Local-First)
- 데스크톱/터미널에서 직접 실행
- 데이터 주권과 자율성 확보
- 관련: [[self-hosting-digital-autonomy|셀프 호스팅과 디지털 자립]]

### 3. 모델 유연성
- 원하는 AI 모델을 연결 가능
- 특정 플랫폼 종속성 회피

## AI 안전성 관점
- 에이전트 실행 기록 = [[ai-safety-alignment|AI 정렬과 안전성]]의 실천적 도구
- "AI가 무엇을 했는지 알 수 없다"는 문제를 구조적으로 해결
- 관련: [[ai-cognitive-management|AI 시대의 인지 관리]] — AI가 대신하는 작업의 투명성 확보

## 미해결 질문
- 실행 기록의 표준화 포맷은 필요한가?
- 로컬 우선 구조가 협업 환경에서 어떻게 확장되는가?
- 에이전트 기록이 AI 신뢰 문제를 얼마나 해결할 수 있는가?

## 관계
- 관련 컨셉: [[self-hosting-digital-autonomy|셀프 호스팅과 디지털 자립]], [[ai-safety-alignment|AI 정렬과 안전성]], [[ai-cognitive-management|AI 시대의 인지 관리]]
- 관련 엔티티: [[gpt-6-astra|GPT-6 Astra]] (에이전트로 활용 가능한 AI 모델)
- 관련 컨셉: [[ai-org-transformation|AI 조직 전환]] (조직 차원의 에이전트 도입 시 실행 기록의 역할)