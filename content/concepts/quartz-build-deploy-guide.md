---
title: "Quartz 빌드 및 GitHub Pages 배포 가이드"
tags: [Quartz, GitHub-Pages, 가이드, 배포, wiki-llm]
date: 2026-09-27
---

# Quartz 빌드 및 GitHub Pages 배포 가이드

> wiki-llm을 수동으로 빌드하고 GitHub Pages에 배포하는 방법

## 0. 사전 준비

### 필요한 도구

```bash
# Node.js 20+ 확인
node --version

# Git 확인
git --version

# GitHub CLI (인증 완료)
gh auth status
```

### 디렉토리 구조

```
wiki-llm/                    # GitHub repo (main 브랜치)
├── content/                 # 마크다운 원본 (665+ 파일)
│   ├── concepts/            # 개념/작업 이력
│   ├── comparisons/         # 비교 분석
│   ├── entities/            # 인물/기업
│   ├── queries/             # 질문/답변
│   ├── raw/                 # 원문 수집
│   ├── 재테크/              # 투자 관련
│   └── 프론트엔드/          # 개발 관련
├── quartz/                  # Quartz 엔진 (수정 금지)
├── quartz.config.yaml       # Quartz 설정
├── quartz.ts                # Quartz 진입점
├── package.json             # 의존성
└── public/                  # 빌드 결과 (gh-pages 브랜치)
```

## 1. 새 글 작성

### 마크다운 파일 만들기

```bash
# content/ 아래 적절한 폴더에 .md 파일 생성
# 예: 작업 이력 → content/concepts/
# 예: 투자 분석 → content/재테크/
```

### 마크다운 형식

```markdown
---
title: "글 제목"
tags: [태그1, 태그2]
date: 2026-09-27
---

# 제목

내용 작성...

## 섹션

- 항목 1
- 항목 2

> 인용구

[[다른-페이지]]   ← 위키 링크 (Obsidian 스타일)
```

### frontmatter 필드

| 필드 | 필수 | 설명 |
|------|:---:|------|
| `title` | ✅ | 페이지 제목 |
| `tags` | 선택 | 태그 배열 (검색/분류용) |
| `date` | 선택 | 작성일 (정렬용) |

## 2. Quartz 빌드

### 저장소 클론 (최초 1회)

```bash
cd /tmp
git clone -b main https://github.com/tonyhan18/wiki-llm.git wiki-llm-full
cd wiki-llm-full
npm install
```

### 빌드 실행

```bash
# content/ 의 마크다운을 public/ 으로 빌드
npx quartz build -d content -o public
```

**빌드 결과 예시:**
```
Quartz v5.0.0
Found 665 input files from `content`
Parsed 665 Markdown files in 4s
Emitted 1539 files to `public` in 1m
Done processing 665 files in 1m
```

### 빌드 확인

```bash
# 파일 카운트
find public -name "*.html" | wc -l

# 특정 페이지 확인
ls public/concepts/내-페이지.html
```

## 3. GitHub Pages 배포

### gh-pages 브랜치에 push

```bash
cd public

# git 초기화 (최초 1회만)
git init
git checkout -b gh-pages

# 변경사항 커밋
git add -A
git commit -m "docs: 새 글 추가 — 제목"

# 원격 저장소 연결 (최초 1회만)
git remote add origin https://github.com/tonyhan18/wiki-llm.git

# 푸시
git push origin gh-pages --force
```

### main 브랜치에도 원본 마크다인 push

```bash
cd /tmp/wiki-llm-full   # 저장소 루트로 이동
git add content/
git commit -m "docs: 새 글 추가 — 제목"
git push origin main
```

### 배포 확인

```bash
# GitHub Pages 빌드 대기 (30초~1분)
sleep 30

# HTTP 상태 확인
curl -s -o /dev/null -w "%{http_code}" \
  "https://tonyhan18.github.io/wiki-llm/concepts/페이지-slug"
```

| 상태 코드 | 의미 |
|-----------|------|
| 200 | ✅ 배포 성공 |
| 404 | ❌ 아직 빌드 중이거나 경로 오류 |

## 4. 전체 과정 한 번에 실행

```bash
#!/bin/bash
# deploy-wiki.sh — wiki-llm 빌드 + 배포 원클릭 스크립트

set -e

REPO_DIR="/tmp/wiki-llm-full"
COMMIT_MSG="${1:-docs: wiki-llm 업데이트}"

cd "$REPO_DIR"

echo "=== 1. Quartz 빌드 ==="
npx quartz build -d content -o public

echo "=== 2. main 브랜치 push (원본 마크다운) ==="
git add content/
git commit -m "$COMMIT_MSG" || echo "no changes"
git push origin main

echo "=== 3. gh-pages 브랜치 push (빌드 결과) ==="
cd public
git add -A
git commit -m "$COMMIT_MSG" || echo "no changes"
git push origin gh-pages --force

echo "=== 4. 배포 확인 ==="
sleep 15
STATUS=$(curl -s -o /dev/null -w "%{http_code}" "https://tonyhan18.github.io/wiki-llm/")
echo "wiki-llm 상태: $STATUS"

if [ "$STATUS" = "200" ]; then
  echo "✅ 배포 완료: https://tonyhan18.github.io/wiki-llm/"
else
  echo "⏳ GitHub Pages 빌드 대기 중..."
fi
```

### 사용법

```bash
# 기본 커밋 메시지
bash deploy-wiki.sh

# 커스텀 커밋 메시지
bash deploy-wiki.sh "docs: n8n 트러블슈팅 기록 추가"
```

## 5. 자주 발생하는 문제

### 빌드 에러

| 에러 | 원인 | 해결 |
|------|------|------|
| `npm error could not determine executable` | node_modules 없음 | `npm install` 실행 |
| `Cannot read properties of undefined` | quartz.config.yaml 손상 | 기본 config로 복원 |
| LaTeX warning | 수식에 한글 포함 | 무시해도 됨 (warn only) |

### 배포 에러

| 에러 | 원인 | 해결 |
|------|------|------|
| 404 | gh-pages 빌드 대기 중 | 30초 후 재확인 |
| 권한 에러 | gh auth 만료 | `gh auth login` 재실행 |
| conflict | gh-pages 히스토리 꼬임 | `--force`로 푸시 |

### Quartz 설정 (quartz.config.yaml)

```yaml
configuration:
  pageTitle: "TonyHan's Wiki LLM"
  enableSPA: true
  enablePopovers: true
  locale: ko-KR
  baseUrl: tonyhan18.github.io/wiki-llm
  ignorePatterns:
    - private
    - templates
    - .obsidian
    - .git
```

> `baseUrl`은 GitHub Pages 경로와 정확히 일치해야 함

## 6. 팁

### 새 글 빠르게 추가하기

```bash
# 템플릿으로 새 글 생성
cat > /tmp/wiki-llm-full/content/concepts/새글.md << 'EOF'
---
title: "새 글 제목"
tags: [태그]
date: 2026-09-27
---

# 새 글 제목

내용...
EOF
```

### 로컬 미리보기

```bash
npx quartz build -d content -o public --serve
# http://localhost:8080 에서 미리보기
```

### 전체 페이지 수 확인

```bash
find content -name "*.md" | wc -l
```