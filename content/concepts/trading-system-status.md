---
title: "🚦 프로젝트 진행현황 — 트레이딩 시스템"
tags: [진행현황, trading-api, 전략, 다른PC]
date: 2026-10-31
---

# 🚦 프로젝트 진행현황 — AI 주식투자 시스템

> **다른 PC 에이전트용 실시간 진행현황 페이지.** 새 작업 세션이 시작될 때마다 갱신된다.
> 저장소 접근 권한이 있으면 **`AGENTS.md`**(루트, 5분 요약) → `docs/HANDOFF.md`(시스템 전체) → `docs/STRATEGY-LEDGER.md`(전략 계보) 순으로 읽으면 된다.

**최종 갱신**: 2026-10-31 (KST) · **작성**: Hermes Agent(봉순, macOS)
**최신 커밋**: `c70b098` (docs: AGENTS.md + WEL-v1 최종 보고서)
**DevLog**: 29개 (000~028) · **백테스트**: 16건 · **전략 세대**: GEN 11

---

## ⚡ 30초 요약 — 지금 가장 중요한 것

| 항목 | 결론 (실측, 데이터 기준일 2026-10-29) |
|------|------|
| 확정 전략 | **WEL-v1 (Wave Exit Ladder)** — 사용자 24타점 매수 불변 + 래더 매도 + A20 재진입 20일 금지 |
| 3.68년 · QQQ 집행 | **CAGR 24.92% · MDD −10.97% · Sharpe 1.63** (QQQ 보유 26.48% / −22.77% / 1.28) |
| 3.68년 · QLD 집행 | **CAGR 46.23% · MDD −22.71% · Sharpe 1.47** (QLD 보유 45.52% / −42.18% / 1.14) |
| 10년 · 기계버전 M2+R3+A20 | CAGR 14.62% · MDD −30.51% · **Sharpe 0.90 < QQQ 보유 0.95** → **열세** |
| 한 줄 판정 | 전략의 가치는 **"같은 수익, 절반의 낙폭"**. 3.68년 수치는 구간 특혜를 포함하므로 10년 결론을 대신하지 않는다 |

**다음 우선순위**: ① 1w 전량매도 → 50% 완화 측정 ② L(손절가)="두 번째 바닥 Low" 정확 산출 ③ 중간 레버리지 1.25~1.5배 측정 ④ 재진입 규칙 확정(사용자 결정)

---

## 시스템 한눈에

```
EC2 3.38.96.46 (t3.small — SSH: ssh -i ~/.ssh/n8n-key.pem ubuntu@3.38.96.46)

trading-api  :8900  NestJS (order·dashboard·heatmap)  /api/status 200
collector-api: 8899  RSS/YouTube 수집 (Docker, network host)  200
n8n          :5678  수집 스케줄러 (4h → /run 202 async)        200
Qdrant       :6333  RAG 벡터                                    200
ollama       :11434 로컬 LLM

GitHub: tonyhan18/trading-api (private) — Mac ~/trading-api 는 clone본
        commit+push → webhook → EC2 자동재배포 (~45초, image digest 고정)
배포 원본: 저장소 pipeline/ (install_pipeline.py 가 ~/scripts 로 덮어씀 — ~/scripts 직접 수정 금지)
```

## ✅ 데이터 (실측 2026-10-31)

| 테이블 | 건수 |
|---|---|
| rss_articles | 1,903 |
| telegram_messages | 572 |
| stock_prices | 41,523 |
| mirae_reports / club_reports | 298 / 486 |
| ai_trace | 186 |
| run_log | 698 |
| backtests | 16 |

`invest.db` 19MB · **디스크 97% (698M 여유) — docker prune 승인 대기**

## ⏱️ 자동 파이프라인 (크론탭 KST)

| 시각 | 작업 |
|------|------|
| 16:30 | 주가 수집 (pykrx + yfinance, 34종목) |
| 16:40 | daily_insight — 섹터 집계 → LLM 전망 → 히트맵 → 추천 |
| 16:45 | strategy_engine — MTF·모멘텀·웅덩이 신호 → signal_log |
| 16:50 | pitbuy — 웅덩이 매매법 (90일 고점 −15% & RSI7<30) |
| 17:10 | ai_trader — LLM 매매 의견 |
| 매 4h | n8n → collector /run |

## 📈 전략 계보 (현역 — 상세는 `docs/STRATEGY-LEDGER.md`)

| 전략 | 구간 | CAGR | MDD | Sharpe | 판정 |
|------|------|------|-----|--------|------|
| **WEL-v1+A20 → QQQ** | 3.68년 | 24.92% | **−10.97%** | **1.63** | ✅ 실전 후보 1순위 |
| WEL-v1+A20 → QLD(2배) | 3.68년 | 46.23% | −22.71% | 1.47 | OBSERVE (고위험) |
| M2+R3+A20 (완전 기계) | 10년 | 14.62% | −30.51% | 0.90 | ⚠️ QQQ 보유에 열세 |
| 모멘텀 TOP2 (5종 자산) | 3년 | 45.3% | −20.2% | 1.56 | ✅ PASS |
| MTF v2 (감쇠 분할매도) | 10년 | 21.6% | −16.1% | 1.32 | OBSERVE (방어 파트) |
| MTF+모멘텀 50/50 | 3년 | 30.1% | −15.5% | 1.41 | OBSERVE (구 본선) |

**통과 기준**: Sharpe ≥ 1.5 & MDD ≥ −35% · **계산 법칙**: 현금=가치보존(SHY 3%), 재진입 수량 = cash_value / 현재가
⚠️ **구간 없는 CAGR 인용 금지** — 같은 전략이 3.68년과 10년에서 정반대 결론이 난다.

## 🆕 최근 작업 (10-27 → 10-31)

| 일자 | 커밋 | 작업 |
|------|------|------|
| 10-27 | — | 섹터 선별 × 파동 결합 가능성 평가 (설계 검토) |
| 10-28 | `5ff1fd0` | 1w=전량매도 고정, 1d/2d 비율 441조합 최적화 |
| 10-28 | — | 사용자 24타점 정식 평가 · 레버리지(QLD) 비교 · MTF 메인프레임 |
| 10-29 | `9452b0b` | **WEL-v1 확정** (래더 매도 + A20 재진입 금지) |
| 10-29 | `1bcb8bd` | 진입 타점 유의성 검증 (무작위 대조군) |
| 10-29 | `39e0ad0` `c6f57a9` | SCHD 파킹 / TLT·GLD 하락장 방어 비교 |
| 10-30 | `01ff3ae` | **WEL-v1-M2 완전 기계판** — 10년 검증 |
| 10-30 | `8a9908d` | 매수 타점 명세 대조 · 정의 역산 |
| 10-30 | `e998c2a` | **QLD(2배) 집행 평가** — 신호 QQQ / 체결 QLD |
| 10-31 | `c70b098` | **AGENTS.md 신설** + WEL-v1 최종 보고서 + HANDOFF/LEDGER 갱신 |

## 🚧 진행중 / 대기

| # | 작업 | 상태 |
|---|------|------|
| 1 | 1w 전량매도 → 50% 완화 측정 | 대기 (다음 전략 세션 최우선) |
| 2 | L = "두 번째 바닥 Low" 정확 산출 | 대기 |
| 3 | 중간 레버리지 1.25~1.5배 측정 | 대기 (현재 수치는 추정) |
| 4 | 디스크 97% — docker prune | **승인 대기** |
| 5 | Next.js 빌드·배포 | 코드 done — t3.small swap 필요 |
| 6 | 쇼츠 자동화 (섹터전환 → shorts_script → TTS) | 미착수 |
| 7 | TG 섹터알림 실발송 · 실계좌 키 등록 | **사용자만 가능** |

## 🔑 다른 PC 에이전트 체크리스트

1. `AGENTS.md` (루트) — 5분 요약 · 규칙 · 금지사항
2. `docs/HANDOFF.md` §0 — 최신 상태 블록 (2026-10-31)
3. `docs/STRATEGY-LEDGER.md` — 전략 계보 GEN 1~11
4. `docs/analysis/wel-v1-final-report.md` — **[최신] WEL-v1 · QQQ/QLD 비교 표**
5. `docs/devlog/000~028` — 실험 과정·실패 이력
6. 웹: `http://3.38.96.46:8900/` · `/autotrade` · `/heatmap`
7. SSH 키 `~/.ssh/n8n-key.pem` 를 새 환경으로 복사

## 시스템 존재 이유

> **"전략이 좋은지 몰라 계속 흔들렸다" → 백테스트·규칙엔진·근거로그로 흔들림 제거.**
> 질문 프레임: '전략이 좋은가?' → **'지금 신호가 무엇인가?'**
