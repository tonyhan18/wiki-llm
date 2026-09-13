---
title: "1990년대 인증기관의 RSA 키를 인수분해했다"
source: GeekNews
url: https://news.hada.io/topic?id=33359
collected: 2026-09-08
---

# 1990년대 인증기관의 RSA 키를 인수분해했다

## 요약
초기 웹 브라우저가 신뢰했던 E-Certify의 512비트 RSA 루트 인증서 2개를 데스크톱 컴퓨터에서 인수분해하여 개인 키를 복원함.

## 핵심 내용
- CADO-NFS(Number Field Sieve)를 사용하여 개인 키 복원에 필요한 두 소수를 구함
- Ryzen 9 5950X에서 SSL용 키는 32시간, S/MIME용 키는 29시간 소요
- 옛 브라우저에서 이 인증서를 신뢰하도록 설정되어 있었음
- 512비트 RSA 키가 현대 하드웨어로 인수분해 가능 수준이라는 것을 실증