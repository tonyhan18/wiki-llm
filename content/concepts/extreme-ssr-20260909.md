---
title: "극단적 SSR: JavaScript 없이 동적 화면 (2026-09-09)"
created: 2026-09-09
updated: 2026-09-09
type: concept
tags: [인사이트]
sources: [raw/articles/extreme-ssr-20260909.md]
confidence: medium
contested: false
---

# 극단적 서버 사이드 렌더링 (SSR) 실험

HTTP 응답을 끝내지 않고 HTML과 CSS를 계속 보내면, 프론트엔드 JavaScript 없이도 화면을 동적으로 변경할 수 있다는 실험적 접근. 브라우저의 즉시 렌더링 특성을 활용하고, 사용자 입력은 숨겨진 iframe으로 폼 제출하여 메인 페이지를 갱신.

## 핵심 내용

- **발상:** 서버가 HTML 스트림을 계속 전송 → 브라우저가 도착한 내용을 즉시 렌더링
- **입력 처리:** 숨겨진 iframe으로 폼 제출 → 메인 페이지는 서버 응답으로 갱신
- **의의:** JavaScript 의존도를 극단적으로 줄이는 대안적 아키텍처 탐색. [[self-hosting-digital-autonomy|셀프 호스팅과 디지털 자립]]의 맥락에서 경량 웹 인프라의 가능성

## 관련 개념

- [[libreoffice-ai-free-20260909|LibreOffice: AI 비탑재가 차별점]] — 기능 축소를 차별점으로 만드는 전략과 유사, JavaScript 없음을 특징으로 활용
- [[design-system|디자인 시스템 구축]] — 프론트엔드 아키텍처 선택이 개발 문화와 사용자 경험에 미치는 영향