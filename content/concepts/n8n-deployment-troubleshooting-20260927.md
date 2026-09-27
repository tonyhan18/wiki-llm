---
title: "n8n 배포 이슈 트러블슈팅 — secure cookie 에러"
tags: [n8n, AWS, Docker, 트러블슈팅, DevOps]
date: 2026-09-27
---

# n8n 배포 이슈 트러블슈팅 — secure cookie 에러

> 2026-09-27 AWS EC2 t4g.micro에 n8n을 배포하면서 발생한 문제와 해결 과정 기록

## 1. 배경

데이터 수집 자동화를 위해 Hermes cron(AI 에이전트 의존)에서 n8n(결정적 자동화)으로 이관하기로 결정. AWS EC2 t4g.micro (ARM64, 1GB RAM)에 Docker Compose로 n8n + PostgreSQL을 배포.

## 2. 문제 발생

n8n 배포 후 브라우저 접속 시 `/setup` URL로 리다이렉트되며 다음 에러 발생:

```
Your n8n server is configured to use a secure cookie, however you are either 
visiting this via an insecure URL, or using Safari.

To fix this, please consider the following options:
- Setup TLS/HTTPS (recommended)
- If you are running this locally, and not using Safari, try using localhost instead
- Set the environment variable N8N_SECURE_COOKIE to false
```

### 원인

n8n은 기본적으로 secure cookie를 사용함. HTTP(비HTTPS)로 접속하면 브라우저가 secure cookie를 거부하여 setup 페이지 진입 불가.

## 3. 해결 과정

### 시도 1: .env 파일에 환경변수 추가 (실패)

`.env` 파일에 `N8N_SECURE_COOKIE=false`를 추가하고 `docker restart n8n` 실행.

```bash
echo 'N8N_SECURE_COOKIE=false' >> ~/n8n/.env
docker restart n8n
```

**결과**: 컨테이너 내부에서 환경변수 확인 시 비어 있음.

```bash
docker exec n8n printenv | grep COOKIE
# (출력 없음)
```

**원인**: `.env` 파일에 변수를 추가했지만, `docker-compose.yml`의 `environment` 섹션에 명시적으로 선언되어 있지 않아 컨테이너로 전달되지 않음. Docker Compose는 `.env` 파일의 변수를 `${VAR}` 형태로 참조할 때만 사용함.

### 시도 2: docker-compose.yml에 환경변수 직접 추가 (성공)

`docker-compose.yml`의 n8n 서비스 `environment` 섹션에 직접 추가:

```yaml
environment:
  # ... 기존 변수들 ...
  - GENERIC_TIMEZONE=Asia/Seoul
  - TZ=Asia/Seoul
  
  # ── 보안 쿠키 비활성화 (HTTP 접속 허용) ──
  - N8N_SECURE_COOKIE=false
```

재배포:

```bash
docker compose down
docker compose up -d
```

**결과**: 컨테이너 내부에서 환경변수 확인 성공.

```bash
docker exec n8n printenv | grep COOKIE
# N8N_SECURE_COOKIE=false
```

n8n setup 페이지 정상 응답 (HTTP 200). 브라우저 접속 성공.

## 4. 핵심 교훈

### Docker Compose 환경변수 전달 방식

| 방식 | 방법 | 작동 여부 |
|------|------|:---------:|
| `.env` 파일만 작성 | 변수를 .env에 추가 | ❌ 컨테이너에 전달 안 됨 |
| `docker-compose.yml`에 명시 | `environment: - VAR=value` | ✅ 컨테이너에 전달됨 |
| `${VAR}` 참조 | yml에서 `${N8N_SECURE_COOKIE:-false}` | ✅ .env에서 읽어옴 |

> **Docker Compose는 `.env` 파일을 `docker-compose.yml` 내에서 `${VAR}` 형태로 참조할 때만 사용한다. 단순히 `.env`에 변수를 추가한다고 해서 컨테이너 환경변수로 주입되지 않는다.**

### 올바른 패턴

```yaml
# docker-compose.yml
environment:
  - N8N_SECURE_COOKIE=${N8N_SECURE_COOKIE:-false}  # .env에서 읽거나 기본값
```

```bash
# .env
N8N_SECURE_COOKIE=false
```

## 5. 추가로 발생했던 문제들

### 5.1 EC2 OOM (Out of Memory)

t4g.micro는 RAM이 1GB라 Docker 빌드 시 npm ci가 메모리 부족으로 실패.

**해결**: 2GB 스왑 추가

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

### 5.2 EC2 SSH 접속 불가 (OOM 후)

빌드 중 메모리 부족으로 SSH 데몬이 죽어서 접속 불가.

**해결**: AWS CLI로 인스턴스 stop → start (재부팅이 아닌 완전 stop/start). 이후 새 퍼블릭 IP 할당됨 (43.200.244.70 → 3.34.144.81).

```bash
aws ec2 stop-instances --instance-ids i-xxx
aws ec2 wait instance-stopped --instance-ids i-xxx
aws ec2 start-instances --instance-ids i-xxx
```

### 5.3 Docker 빌드 실패 — public 디렉토리 없음

Next.js Dockerfile에서 `COPY --from=builder /app/public ./public`가 실패. public 디렉토리가 없었음.

**해결**: `COPY` 대신 `RUN mkdir -p ./public`으로 변경.

### 5.4 보안 그룹 포트 미개방

Control Panel (포트 3000)이 보안 그룹에 등록되지 않아 외부 접속 불가.

**해결**: AWS CLI로 인바운드 규칙 추가

```bash
aws ec2 authorize-security-group-ingress \
  --group-id sg-xxx \
  --protocol tcp --port 3000 \
  --cidr 0.0.0.0/0
```

### 5.5 Docker Hub private repo 한도

Docker Hub 무료 플랜은 private repo 1개만 가능. 기존 `myweb`이 private 1개를 차지 중.

**해결**: `data-control-panel`을 public으로 생성 (코드는 GitHub에 이미 public).

## 6. 최종 배포 아키텍처

```
AWS EC2 t4g.micro (3.34.144.81)
├── n8n (포트 5678) — 워크플로우 자동화
├── n8n-postgres (포트 5432) — n8n 데이터베이스
├── control-panel (포트 3000) — 데이터 수집 관리자 페이지
└── 2GB swap — 메모리 부족 보완
```

## 7. 관련 문서

- [[n8n vs Airflow 선택 기준]]
- [[Data Control Panel 아키텍처]]
- [[데이터 수집 파이프라인 설계]]