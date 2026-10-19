---
title: "🚦 프로젝트 진행현황 — 트레이딩 시스템"
tags: [진행현황, trading-api, 대시보드, 다른PC]
date: 2026-10-20
---

# 🚦 프로젝트 진행현황 — AI 주식투자 시스템

> **다른 PC 에이전트용 실시간 진행현황 페이지.** 이 페이지는 새 작업 세션이 시작될 때마다 갱신된다.
> 전체 인수인계는 [trading-api `docs/HANDOFF.md`](https://github.com/tonyhan18/trading-api/blob/main/docs/HANDOFF.md) (365라인 단일 문서)를 읽을 것.

**최종 갱신**: 2026-10-20 (KST)
**작성 에이전트**: Hermes Agent (봉순) — Mac
**최신 커밋**: `7c0f467` devlog 024 · **DevLog**: 025개 (000~024) · **backtests**: 6건

---

## 시스템 한눈에

```
EC2 3.38.96.46 (t3.small — SSH: ssh -i ~/.ssh/n8n-key.pem ubuntu@3.38.96.46)

trading-api  :8900  NestJS (order·dashboard·heatmap serving)
collector-api: 8899  RSS/YouTube 수집 (Docker, network host)
n8n          :5678  수집 스케줄러 (4h → /run 202 async)
Qdrant       :6333  RAG 벡터 (rss_articles 562 points 100%)
ollama       :11434 glm-5.3-flash

GitHub: tonyhan18/trading-api (private) — Mac ~/trading-api는 clone본
        commit+push → webhook → EC2 자동재배포 (~45초)
```

## ✅ 완료 — 데이터 인프라

| 소스 | 규모 | 상태 |
|------|------|------|
| RSS·유튜브 4채널 (삼프로TV·한경2·소수몽키·수페TV) | 562건 | ✅ 4h 자동 |
| Telethon 텔레그램 | 85건 | ✅ |
| 학회 SMIC 리포트 | 480건 (413 벡터화) | ✅ |
| 주가 (34종목 + 10년 QQQ·SOXX·SPY·GLD·TLT) | 25,000+ rows | ✅ 평일 KST 16:30 |
| 섹터 스냅샷 (17섹터) | — | ✅ 평일 KST 16:40 |
| RAG (Qdrant) | 562 points | ✅ 의미 검색 작동 |

## ⏱️ 자동 파이프라인 (크론탭 KST)

| 시각 | 작업 |
|------|------|
| 16:30 | 주가 수집 (pykrx+yfinance 34종목) |
| 16:40 | daily_insight — 섹터 집계 → LLM 전망 → 히트맵 → 추천 |
| 16:45 | strategy_engine — MTF·모멘텀·웅덩이 신호 → signal_log |
| 16:50 | pitbuy — 웅덩이 매매법 (90일 고점 -15% & RSI 7<30) |
| 17:10 | ai_trader — LLM 매매 의견 |
| 매 4h | n8n → collector /run |

## 📈 전략 (백테스트 6건 — 자세한 계보는 STRATEGY-LEDGER.md)

| 전략 | 샤프(단기) | 샤프(10y) | MDD | 판정 |
|------|-----------|----------|-----|------|
| 섹터모멘텀 TOP2 (5종) | **1.56** | 0.82 | -20.2% | PASS |
| MTF 감쇠 분할매도+재진입 | 1.32 | 0.90 | -9.3% | 방어 OK |
| **MTF+모멘텀 50/50** | 1.41 | **0.91** | -15.5% | 균형 (권장 시작점) |
| 웅덩이 매매 (R3·R4) | — | — | — | 규칙엔진 통합 |

**통과 기준**: 샤프 ≥ 1.5 & MDD ≥ -35% · **계산 법칙**: 현금=가치보존(SHY 3%), 재진입 수량=cash_value/현재가

## 🆕 최근 작업 (10-18 → 10-19)

| 일자 | 작업 |
|------|------|
| 10-19 | devlog 024 — "매매 판단은 금요일, 실행은 월·수" 요일효과 분석 (bootstrap 1,000회, QQQ p=0.069 우연) |
| 10-19 | FEEDBACK-REQUEST-mtf — 커뮤니티 공유 문서 |
| 10-18 | devlog 022/023 — strategy_engine 크론 등록, 수페TV 채널 추가 |
| 10-18 | devlog 020 — weight sweep + 10y 검증 |
| 10-18 | STRATEGY-LEDGER 원장 신설 · devlog 017 cron TZ 수정 · collector Docker화 (015) |

## 🚧 진행중 / 대기 (10-19 기준)

| # | 작업 | 상태 |
|---|------|------|
| 1 | Next.js 빌드·배포 (web/ 스캔폴드 완성) | 코드 done — **t3.small swap 필요** → 배포 대기 |
| 2 | 쇼츠 자동화 (섹터전환 → shorts_script → TTS) | 미착수 |
| 3 | SHY→TQQQ 스위칭 + 웅덩이 히트맵 연동 | 계획 (devlog 021) |
| 4 | TG 섹터알림 실발송 (TG_BOT_TOKEN·TG_CHAT_ID) | **사용자만 가능** — 대기 |
| 5 | 실계좌 키 등록 (KIS·토스·바이낸스·바이비트) | dry-run만 동작 — 대기 |
| 6 | recommendation_history + /trend (Chart.js) | 로컬 구현 완료, **push 대기 중** |

## 🔑 다른 PC 에이전트 체크리스트

1. `git clone https://github.com/tonyhan18/trading-api.git` (private — 권한 필요)
2. `docs/HANDOFF.md` 전체 읽기 (365 라인 — 시스템 전체 인수인계)
3. `docs/STRATEGY-LEDGER.md` — 전략 현재 유효 결론 스냅샷
4. `docs/devlog/` — 025개 개발일지 (실험 과정·실패 이력)
5. 웹사이트: `http://3.38.96.46:8900/` · `/autotrade` · `/heatmap`
6. SSH 키 `~/.ssh/n8n-key.pem` 새 환경으로 복사 필요

## 시스템 존재 이유

> **"전략이 좋은지 몰라 계속 흔들렸다" → 백테스트·규칙엔진·근거로그로 흔들림 제거.**
> 질문 프레임: '전략이 좋은가?' → **'지금 신호가 무엇인가?'**

지금은 코스피 90일 고점 대비 -39%의 역사적 웅덩이 구간 — 시스템이 실전에서 처음 증명될 시점.