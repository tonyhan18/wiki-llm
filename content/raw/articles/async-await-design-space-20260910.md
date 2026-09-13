---
title: "async/await의 설계 공간 탐색"
source: GeekNews
url: https://news.hada.io/topic?id=33475
pub_date: 2026-09-10
collected: 2026-09-10
tags: [AI, 개발, 기술]
---

# async/await의 설계 공간 탐색

## 요약
Brown 대학 연구「A Design Space Exploration of Async/Await」는 async/await가 언어마다 실행 의미가 달라 같은 백그라운드 로그 예제가 7개 런타임에서 4가지 출력을 만든다는 것을 보임. 차이는 성능이 아닌 관찰 가능한 동작을 바꾸는 설계 선택에서 발생.

## 핵심 내용
- **순차형 비동기성(straight-line asynchrony):** Python, Rust, Swift가 공통적으로 동시성 프로그램을 순차 코드처럼 작성하도록 설계
- **9개 설계 차원:** 즉시성(Eagerness), 중단(Suspension), 수명 범위(Extent), 참조 강도, 정리 방식, 예외 전파, 취소 인지, 취소 방향, 취소 지속성
- **실행 결과 분기:** Asyncio/C#/Tokio/Swift=AC, JavaScript=ACB, Smol=C, Trio=ABC
- **형식 의미론:** 설계 공간을 핵심 계산 체계의 형식 의미론으로 옮기고 실행 추적으로 결과 분기 추적
- **절충:** 각 설계에는 성능·메모리·사용 편의성·실행 의미의 절충이 있어 유일한 정답 없음

## 관련
- 개발/기술: [[harness-engineering]], [[programming-as-art-20260909]]