---
title: "KAIST-MS 신경 가치 정렬(NVA): 뇌파로 AI 목표 교정 (2026-09-10)"
created: 2026-09-10
updated: 2026-09-10
type: concept
tags: [인사이트]
sources: [raw/articles/kaist-ms-neural-value-alignment-20260910.md]
confidence: medium
---

# KAIST-MS 신경 가치 정렬(NVA): 뇌파로 AI 목표 교정

## 개요
KAIST와 마이크로소프트연구소 아시아 공동연구진이 사람의 뇌파를 피드백으로 활용해 AI의 목표와 행동을 조정하는 신경 가치 정렬(NVA, Neural Value Alignment)을 제안했다. 같은 행동에도 여러 목적이 있을 수 있다는 한계에 주목하여, 예상과 다른 결과를 마주했을 때 인간의 뇌파 반응("그게 아닌데")을 활용해 AI의 내재적 목표를 교정하는 방식이다.

## 핵심 사실
- **연구진:** KAIST + Microsoft Research Asia 공동
- **방법:** 뇌파(EEG)를 인간 피드백 신호로 활용
- **핵심 통찰:** 행동 기반 정렬의 한계 — 같은 행동, 다른 목적 가능
- **메커니즘:** 예상과 다른 결과에 대한 뇌파 반응 → AI 내재 목표 교정

## 분석

### 행동 기반 정렬의 한계
- 기존 RLHF는 인간의 행동(선택, 평가)을 피드백으로 사용
- 행동만으로 AI의 내재적 목표를 파악할 수 없음 — 같은 행동이라도 목적이 다를 수 있음
- 뇌파는 의식적 선택 이전의 직관적 반응("그게 아닌데")을 포착 → 더 근본적 정렬 신호

### AI 정렬 패러다임 확장
- [[ai-safety-alignment|AI 정렬과 안전성]]의 새로운 접근 — 행동 → 인지 신호로 확장
- [[anthropic-activist-surveillance-20260910|Anthropic 예측형 감시]]와 대조 — 안전성 연구의 방향성 차이
- 뇌파 기반 피드백은 인간의 직관/안목을 AI 정렬에 활용하는 경로

### 안목과 정렬의 연결
- [[taste-and-ai|안목(taste)과 AI 시대]] — 인간의 직관적 판단(안목)을 AI 교정에 활용
- "그게 아닌데" 반응은 숙련된 판단의 직관적 표현 → AI 정렬에서 인간 안목의 역할
- 과정 중심 정렬 — 결과만 보지 않고 인간의 반응 과정을 학습

## 미해결 질문
- 뇌파 피드백의 대규모 확장성은? (EEG 장비 → 비침습적 확장)
- "그게 아닌데" 신호가 개인차 없이 일관되게 나타나는가?
- 행동 기반 RLHF와 NVA를 결합한 하이브리드 정렬의 효과는?

## 관계
- [[ai-safety-alignment|AI 정렬과 안전성]] — 행동 기반 정렬에서 인지 신호 기반 정렬로 확장
- [[taste-and-ai|안목(taste)과 AI 시대]] — 인간 직관을 AI 교정에 활용하는 경로
- [[anthropic-activist-surveillance-20260910|Anthropic 예측형 감시]] — AI 안전성 접근 방식의 방향성 대조

^[raw/articles/kaist-ms-neural-value-alignment-20260910.md]