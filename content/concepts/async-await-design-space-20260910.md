---
title: "async/await 설계 공간: 언어별 실행 의미 차이 (2026-09-10)"
created: 2026-09-10
updated: 2026-09-10
type: concept
tags: [커리어전략, 인사이트]
sources: [raw/articles/async-await-design-space-20260910.md]
confidence: high
---

# async/await 설계 공간: 언어별 실행 의미 차이

## 정의
Brown 대학 연구는 async/await가 언어마다 실행 의미가 달라, 같은 백그라운드 로그 예제가 7개 런타임(Asyncio, C#, Tokio, Swift, JavaScript, Smol, Trio)에서 4가지 출력(AC, ACB, C, ABC)을 만든다는 것을 보임. 차이는 성능이 아닌 관찰 가능한 동작을 바꾸는 설계 선택에서 발생.

## 핵심 사실

### 9개 설계 차원
1. **즉시성(Eagerness):** hot(즉시 실행) vs cold(await 전까지 대기) vs lazy(코루틴 반환)
2. **중단(Suspension):** await 지점에서 실행 중단 보장 여부 (static vs dynamic)
3. **수명 범위(Extent):** indefinite(런타임 종료까지) vs dynamic(생성 스코프 종료 시)
4. **참조 강도:** strong vs weak
5. **정리 방식:** awaited(완료 대기) vs cancelled(취소) vs terminated
6. **예외 전파:** destructive vs never
7. **취소 인지:** unaware vs aware
8. **취소 방향:** top-down vs bottom-up vs simultaneous
9. **취소 지속성:** transient vs persistent

### 실행 결과 분기
- Swift: Dynamic 수명 + Cancelled 정리 → **AC** (태스크 취소)
- Trio: Dynamic 수명 + Awaited 정리 → **ABC** (완료 대기)
- JavaScript: **ACB** (비동기 함수 즉시 태스크 생성)
- Smol: **C** (가장 먼저 출력)

## 분석

### 설계 선택의 절충
- 각 설계 차원에는 성능·메모리·사용 편의성·실행 의미의 절충 존재
- 유일한 정답 없음 — 언어별 설계 근거가 다름
- 형식 의미론과 실행 추적으로 런타임별 결과가 갈리는 지점 확인 가능

### 개발자 인사이트
- 작은 비동기 프로그램도 정확히 이해하려면 호출 즉시 실행 여부, await 중단 보장, 태스크 수명·참조 강도, 예외 전파, 취소 방식까지 알아야 함
- [[programming-as-art-20260909|프로그래밍은 예술이다]] — 언어 설계 선택을 이해하는 것이 프로그래밍 역량의 핵심
- [[harness-engineering|하네스 엔지니어링]] — 비동기 코드의 실행 의미 이해가 테스트·검증의 기반

## 미해결 질문
- C++ 코루틴이 포함되었을 때 결과가 어떻게 달라지는가?
- 지연 실행(lazy)이 Rust에서는 컴파일러 경고로 안전하지만, Python에서는 최악의 기본값인 이유는?
- 9개 설계 차원 외에 새로운 차원이 추가될 가능성은?

## 관계
- [[programming-as-art-20260909|프로그래밍은 예술이다]] — 언어 설계 이해가 코딩 역량의 핵심
- [[harness-engineering|하네스 엔지니어링]] — 비동기 실행 의미 이해가 검증의 기반
- [[extreme-ssr-20260909|극단적 SSR]] — 비동기 패러다임의 대안적 접근

^[raw/articles/async-await-design-space-20260910.md]