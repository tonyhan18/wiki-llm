# Knowledge Graph Schema

## Domain
투자 · 커리어 · 자산관리 지식 그래프. 
사용자의 개인 투자 철학, 멘토링 노트, 매매 일지, 매크로 인사이트를 노드로 구조화하고,
엔티티 간 관계를 wikilinks로 연결하여 새로운 인사이트를 발견하는 것을 목적으로 함.

## Conventions
- 파일명: 소문자, 하이픈, 공백 없음 (예: `samsung-electronics.md`)
- 모든 페이지는 YAML frontmatter로 시작
- `[[wikilinks]]`로 페이지 간 연결 (최소 2개 아웃바운드 링크)
- 페이지 업데이트 시 `updated` 날짜 갱신
- 새 페이지는 `index.md`에 추가
- 모든 액션은 `log.md`에 기록
- **Provenance:** 3개 이상 소스를 종합한 페이지에서는 `^[raw/articles/source.md]` 마커로 출처 표시

## Frontmatter
```yaml
---
title: 페이지 제목
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query
tags: [taxonomy에서 선택]
sources: [raw/articles/source-name.md]
confidence: high | medium | low
contested: false
---
```

## Tag Taxonomy

### 투자
- `투자철학` - 투자 원칙, 마인드셋
- `매매전략` - 구체적 매매 기법
- `자산배분` - 포트폴리오 구성
- `매크로` - 거시경제, 시장 흐름
- `섹터분석` - 산업/섹터 분석
- `리스크관리` - 위험 관리

### 커리어
- `커리어전략` - 진로 선택, 전략
- `취업준비` - 취업 관련 실전 팁
- `조직문화` - 회사 문화, 환경

### 자산
- `부동산` - 부동산 투자
- `주식` - 주식 투자
- `금` - 금/귀금속
- `달러` - 환율/달러
- `코인` - 암호화폐

### 인사이트
- `인사이트` - 개인적 깨달음
- `비교` - 비교 분석
- `시황` - 시장 상황 분석

## Page Thresholds
- **페이지 생성:** 엔티티/컨셉이 2개 이상 소스에 등장하거나 1개 소스에서 핵심일 때
- **기존 페이지 업데이트:** 소스가 기존 내용 언급 시
- **페이지 분할:** 200줄 초과 시 서브토픽으로 분할
- **아카이브:** 완전히 대체된 페이지 → `_archive/`로 이동

## Entity Pages
사람, 조직, 기업, 자산군, 산업 등. 포함:
- 개요 / 무엇인가
- 핵심 사실과 날짜
- 다른 엔티티와의 관계 (`[[wikilinks]]`)
- 소스 참조

## Concept Pages
투자 철학, 전략, 원칙 등. 포함:
- 정의 / 설명
- 현재 지식 상태
- 미해결 질문이나 논쟁
- 관련 컨셉 (`[[wikilinks]]`)

## Comparison Pages
비교 분석. 포함:
- 무엇을 비교하는지, 왜
- 비교 차원 (표 형식 권장)
- 결론 또는 종합
- 소스