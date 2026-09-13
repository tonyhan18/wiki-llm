---
title: "Build vs Buy: AI 시대의 개발 비용과 유지보수 책임"
source_urls:
  - https://news.hada.io/topic?id=33346
  - https://news.hada.io/topic?id=33344
collected: 2026-09-08
---

## Build vs Buy - AI로 개발은 싸졌지만, 3년 뒤 유지보수는 누가 맡을까?

AI는 소프트웨어의 초기 개발 비용을 크게 낮췄지만, 수년간 유지보수와 보안, 장애 대응을 책임지는 운영 비용까지 없애지는 못함. 직접 만들지는 가격뿐 아니라 장애나 침해의 피해 범위, 장기 운영 책임자, 사업 차별화, 교체할 경로를 기준으로 판단해야 함.

### 핵심 내용
- 자체 구축한 인증 시스템은 라이선스 절감액을 유지보수와 보안 심사 대응에 소모
- 아낀 라이선스 비용은 개발 시간, 출시하지 못한 기능, 경영진의 고객 대응 시간으로 되돌아옴
- 상용 인증 서비스로 전환 후 약 한 달 뒤 기존 오픈소스에서 심각한 취약점 발생
- AI가 바꾼 것은 만드는 비용이지, 계속 운영하는 비용이 아님
- 핵심 질문은 "만드는 편이 더 싼가"가 아니라 "무엇을 앞으로도 계속 책임질 것인가"

### 직접 만들기 전 확인할 네 가지 질문
1. **피해 범위**: 장애나 침해가 발생하면 무엇이 영향을 받는가
2. **장기 운영 비용과 책임**: 향후 수년간 감시, 패치, 보안 대응을 누가 맡는가
3. **사업 차별화**: 고객에게 중요한 개선인가, 단지 취향에 잘 맞는가
4. **교체할 경로**: 1년 뒤 좋은 외부 제품이 나오면 큰 부담 없이 버릴 수 있는가

### MCP 서버 사례
- 공식 MCP 서버가 출시되자 즉시 전환해 자체 버전 유지보수 책임을 피함
- 공급업체에는 자사 API 변화에 맞춰 서버를 유지할 동기가 훨씬 큼

## Claude Code에서 GPT-6 Astra를 메인 모델로 쓰기

Claude Code 하나에서 OpenAI 모델과 Claude 모델을 섞어 쓰는 방법. 메인 모델은 GPT-6 Astra로, 서브 에이전트는 Claude로 운영.

### 기술 구현
- Anthropic API 앞단에서 자체 CA로 TLS를 종료하며 api.anthropic.com 인 척 응답
- ANTHROPIC_BASE_URL을 바꾸거나 SDK를 패치하지 않음
- 추가된 모델 이름이면 OpenAI Responses API로 번역해서 전송, 나머지는 Anthropic에 전달
- ChatGPT 구독을 OAuth로 인증, Codex CLI의 auth.json 재사용 가능
- reasoning effort는 Claude Code의 /effort를 그대로 사용

### 추가 모델
- gpt-6-astra, gpt-5.6-sol / terra / luna, gpt-5.5, gpt-5.3-codex-spark
- remote control, 서브 에이전트, hooks, MCP 전부 지원