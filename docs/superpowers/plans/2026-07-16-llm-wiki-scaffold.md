# LLM Wiki 뼈대 구축 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `docs/superpowers/specs/2026-07-16-llm-wiki-design.md`에서 승인된 설계에 따라 raw/wiki 3계층 디렉토리, CLAUDE.md 스키마, index.md/log.md, 첫 프로젝트 페이지를 실제로 생성한다.

**Architecture:** 순수 마크다운/디렉토리 스캐폴딩 작업이다. 실행 코드가 없으므로 "테스트"는 파일 존재·형식(frontmatter, 헤더, grep 패턴) 검증으로 대체한다.

**Tech Stack:** Markdown, git, Obsidian(수동 GUI 확인).

## Global Constraints

- raw/ 는 절대 수정하지 않는다 — 이번 계획에서는 폴더만 만들고 내용물은 넣지 않는다 (spec: "초기 raw source ingest는 하지 않는다").
- wiki/ 문서는 한국어로 작성한다 (spec: 언어 = 한국어).
- 위키 마크다운(요약/개념/index/log 페이지)은 Claude가 직접 생성·수정한다. 실행 가능한 코드(스크립트 등)를 이 프로젝트에서 작성하게 되면 사용자 전역 CLAUDE.md의 "파일/줄 번호와 함께 복사-붙여넣기 제시" 규칙을 따른다 — 이번 계획에는 그런 코드가 없다.
- 버전 관리는 로컬 git만 사용한다 (원격 푸시 없음). 이미 `git init` 완료됨 (커밋 2개 존재).
- Obsidian vault는 이미 이 폴더(`C:\Users\ds_ki\Desktop\Wiki`)로 열려 있다. `.obsidian/` 설정 폴더는 사용자 로컬 상태이므로 git으로 추적하지 않는다.
- log.md 항목 형식: `## [YYYY-MM-DD] <종류> | <제목>` (종류: ingest / query / lint / setup).
- index.md 항목 형식: `- [[페이지명]] — 한줄 요약 (업데이트일)`.
- frontmatter 필드: `title, type, tags, created, updated, related`.

---

## 파일 구조 개요

```
Wiki/
├── .gitignore                     # .obsidian/ 제외
├── CLAUDE.md                      # 스키마 (Task 2)
├── raw/
│   ├── articles/.gitkeep
│   ├── decisions/.gitkeep
│   ├── logs/.gitkeep
│   └── assets/.gitkeep
└── wiki/
    ├── index.md                   # Task 3
    ├── log.md                     # Task 4
    ├── projects/
    │   └── llm-wiki.md            # Task 5
    ├── concepts/.gitkeep
    └── decisions/.gitkeep
```

---

### Task 1: 디렉토리 스캐폴딩 + .gitignore

**Files:**
- Create: `raw/articles/.gitkeep`, `raw/decisions/.gitkeep`, `raw/logs/.gitkeep`, `raw/assets/.gitkeep`
- Create: `wiki/concepts/.gitkeep`, `wiki/decisions/.gitkeep`
- Create: `.gitignore`

**Interfaces:**
- Produces: 이후 모든 태스크가 쓰기 대상으로 사용하는 `raw/`, `wiki/` 폴더 트리.

- [ ] **Step 1: 폴더와 빈 자리표시 파일 생성**

```bash
mkdir -p raw/articles raw/decisions raw/logs raw/assets
mkdir -p wiki/concepts wiki/decisions
touch raw/articles/.gitkeep raw/decisions/.gitkeep raw/logs/.gitkeep raw/assets/.gitkeep
touch wiki/concepts/.gitkeep wiki/decisions/.gitkeep
```

- [ ] **Step 2: .gitignore 작성**

```
.obsidian/
```

- [ ] **Step 3: 생성된 구조 확인**

Run: `find raw wiki -type f`
Expected output (순서 무관):
```
raw/articles/.gitkeep
raw/decisions/.gitkeep
raw/logs/.gitkeep
raw/assets/.gitkeep
wiki/concepts/.gitkeep
wiki/decisions/.gitkeep
```

- [ ] **Step 4: git 상태 확인 — .obsidian이 무시되는지 검증**

Run: `git status --short`
Expected: `.obsidian/` 관련 항목이 나타나지 않고, `raw/`, `wiki/`, `.gitignore`만 untracked로 표시됨.

- [ ] **Step 5: 커밋**

```bash
git add .gitignore raw wiki
git commit -m "Scaffold raw/ and wiki/ directory tree"
```

---

### Task 2: CLAUDE.md 스키마 작성

**Files:**
- Create: `CLAUDE.md`

**Interfaces:**
- Consumes: Task 1의 디렉토리 구조 (경로 이름이 CLAUDE.md 본문에 그대로 언급됨).
- Produces: 이후 세션에서 이 위키를 다룰 때 참조할 스키마 문서. Task 3~5의 index/log/페이지 형식은 이 문서의 "규칙"과 정확히 일치해야 한다.

- [ ] **Step 1: CLAUDE.md 작성**

```markdown
# LLM Wiki — 스키마 및 유지보수 규칙

이 저장소는 개인 개발 지식 위키다. 설계 근거는
`docs/superpowers/specs/2026-07-16-llm-wiki-design.md` 참고.

## 3계층 구조

- `raw/` — 원본 소스. **절대 수정하지 않는다.** 읽기 전용으로만 취급한다.
- `wiki/` — 이 스키마를 따라 LLM이 생성·관리하는 마크다운 지식 페이지.
- `CLAUDE.md` (이 파일) — 스키마.

## 디렉토리

- `raw/articles/` — 학습 자료 원문
- `raw/decisions/` — 개발 결정을 논의한 원본 메모
- `raw/logs/` — 프로젝트 진행 관련 원본 기록
- `raw/assets/` — 이미지 등 첨부파일 (Obsidian attachment 경로)
- `wiki/index.md` — 전체 카탈로그
- `wiki/log.md` — 시간순 작업 이력
- `wiki/projects/` — 프로젝트별 개요 페이지
- `wiki/concepts/` — 기술 개념 페이지 (프로젝트 간 재사용)
- `wiki/decisions/` — 설계/아키텍처 결정 기록 페이지

## 언어

`wiki/` 아래 모든 페이지는 한국어로 작성한다. 원문이 영어여도 요약/정리는 한국어로 한다.

## 페이지 frontmatter

```yaml
---
title: 페이지 제목
type: project | concept | decision | source-summary
tags: [태그1, 태그2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
related: ["[[다른-페이지]]"]
---
```

## index.md 규칙

카테고리(프로젝트/개념/결정)별로:
```
- [[페이지명]] — 한줄 요약 (업데이트일)
```
새 페이지를 만들거나 기존 페이지를 갱신할 때마다 즉시 반영한다.

## log.md 규칙

Append-only, 최신 항목을 파일 맨 아래에 추가:
```
## [YYYY-MM-DD] ingest | 소스 제목
## [YYYY-MM-DD] query | 질문 요약
## [YYYY-MM-DD] lint | 점검 결과 요약
## [YYYY-MM-DD] setup | 작업 내용
```
`grep "^## \[" wiki/log.md`로 이력 조회 가능해야 하므로 접두사 형식을 반드시 지킨다.

## 워크플로우

### Ingest
1. 사용자가 `raw/`에 소스를 추가하고 처리를 요청한다.
2. 소스를 읽고 핵심 내용을 사용자와 논의한다.
3. 관련 `wiki/projects/`, `wiki/concepts/`, `wiki/decisions/` 페이지를 생성하거나 갱신한다.
4. `wiki/index.md`와 `wiki/log.md`를 갱신한다.

### Query
1. 사용자 질문을 받으면 먼저 `wiki/index.md`에서 관련 페이지를 찾는다.
2. 관련 페이지를 읽고 출처를 인용하며 답변한다.
3. 답변이 재사용 가치가 있으면 새 위키 페이지로 파일링할지 사용자에게 제안한다.

### Lint
주기적으로 다음을 점검한다: 페이지 간 모순, 새 소스로 낡아진 주장, 고아 페이지(들어오는 링크 없음), 언급만 되고 페이지가 없는 개념, 누락된 상호참조.

## 코드 작성 규칙 (예외 명시)

이 문서(스키마)와 `wiki/` 아래 마크다운 페이지는 Claude가 직접 생성·수정한다.
단, 이 프로젝트 안에서 실행 가능한 코드(검색 스크립트 등)를 작성해야 할 경우에는
사용자 전역 CLAUDE.md의 규칙 — "파일/줄 번호와 함께 복사·붙여넣기 가능한 형태로
제시하고, 에이전트가 직접 수정하지 않는다" — 를 따른다.

## Obsidian

- 이 폴더 자체가 vault다 (별도 vault 폴더 없음).
- 첨부파일 저장 경로는 `raw/assets/` (Settings → Files and links에서 수동 설정).
- `.obsidian/`은 git으로 추적하지 않는다.
```

- [ ] **Step 2: 필수 섹션 존재 확인**

Run: `grep -E "^## " CLAUDE.md`
Expected: `3계층 구조`, `디렉토리`, `언어`, `페이지 frontmatter`, `index.md 규칙`, `log.md 규칙`, `워크플로우`, `코드 작성 규칙`, `Obsidian` 섹션 헤더가 모두 출력됨.

- [ ] **Step 3: 커밋**

```bash
git add CLAUDE.md
git commit -m "Add wiki schema (CLAUDE.md)"
```

---

### Task 3: wiki/index.md 생성

**Files:**
- Create: `wiki/index.md`

**Interfaces:**
- Consumes: CLAUDE.md의 index.md 규칙 (카테고리, 항목 형식).
- Produces: Task 5가 항목을 추가할 대상 파일. 카테고리 헤더 이름(`## 프로젝트`, `## 개념`, `## 결정`)은 이후 모든 ingest 작업이 그대로 사용해야 하는 고정 이름이다.

- [ ] **Step 1: index.md 작성**

```markdown
# 위키 인덱스

이 문서는 `wiki/` 전체의 카탈로그다. 새 페이지가 생기거나 갱신될 때마다 함께 갱신한다.

## 프로젝트

(아직 없음)

## 개념

(아직 없음)

## 결정

(아직 없음)
```

- [ ] **Step 2: 헤더 확인**

Run: `grep -E "^## " wiki/index.md`
Expected:
```
## 프로젝트
## 개념
## 결정
```

- [ ] **Step 3: 커밋**

```bash
git add wiki/index.md
git commit -m "Add wiki/index.md skeleton"
```

---

### Task 4: wiki/log.md 생성 + 첫 항목 기록

**Files:**
- Create: `wiki/log.md`

**Interfaces:**
- Consumes: CLAUDE.md의 log.md 형식 규칙.
- Produces: 이후 모든 ingest/query/lint 작업이 append하는 대상 파일.

- [ ] **Step 1: log.md 작성 (설명 + 첫 setup 항목)**

```markdown
# 작업 로그

Append-only 기록. 형식: `## [YYYY-MM-DD] <종류> | <제목>` (종류: ingest / query / lint / setup)

## [2026-07-16] setup | LLM Wiki 뼈대 생성

raw/, wiki/ 3계층 구조와 CLAUDE.md 스키마, index.md, 이 로그를 생성.
Obsidian vault로 이 폴더를 열었고, 로컬 git 저장소로 버전 관리 시작.
```

- [ ] **Step 2: grep 파싱 가능 여부 확인**

Run: `grep "^## \[" wiki/log.md`
Expected: `## [2026-07-16] setup | LLM Wiki 뼈대 생성` 한 줄 출력.

- [ ] **Step 3: 커밋**

```bash
git add wiki/log.md
git commit -m "Add wiki/log.md with initial setup entry"
```

---

### Task 5: 첫 프로젝트 페이지 — 이 위키 자체를 프로젝트로 기록

**Files:**
- Create: `wiki/projects/llm-wiki.md`
- Modify: `wiki/index.md` (프로젝트 섹션에 링크 추가)

**Interfaces:**
- Consumes: Task 2의 frontmatter 스키마, Task 3의 `## 프로젝트` 헤더.
- Produces: 향후 이 위키 프로젝트 자체에 대한 진행 상황을 계속 갱신할 페이지.

- [ ] **Step 1: 프로젝트 페이지 작성**

```markdown
---
title: LLM Wiki
type: project
tags: [meta, wiki]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# LLM Wiki

## 개요

Claude와 Obsidian을 이용한 개인 개발 지식 위키 프로젝트. 여러 프로젝트와
기술 학습, 개발 결정을 하나의 위키에 누적한다.

## 구조

- `raw/` — 원본 소스 (불변)
- `wiki/` — 이 프로젝트를 포함해 LLM이 관리하는 지식 페이지
- 설계 근거: [[../../docs/superpowers/specs/2026-07-16-llm-wiki-design|설계 문서]]

## 현재 상태

- 2026-07-16: raw/wiki 디렉토리, CLAUDE.md 스키마, index.md, log.md 뼈대 생성 완료.
  Obsidian vault로 연결, 로컬 git으로 버전 관리 시작.
- 다음 단계: 실제 raw source ingest 시작 (아직 없음).
```

- [ ] **Step 2: index.md 프로젝트 섹션에 링크 추가**

`wiki/index.md`의 `## 프로젝트` 섹션을 아래처럼 수정:

```markdown
## 프로젝트

- [[projects/llm-wiki|LLM Wiki]] — 이 위키 프로젝트 자체의 진행 기록 (2026-07-16)
```

- [ ] **Step 3: frontmatter 및 링크 확인**

Run: `grep -E "^(title|type|created):" wiki/projects/llm-wiki.md`
Expected: `title: LLM Wiki`, `type: project`, `created: 2026-07-16` 출력.

Run: `grep "llm-wiki" wiki/index.md`
Expected: 방금 추가한 링크 줄이 출력됨.

- [ ] **Step 4: 커밋**

```bash
git add wiki/projects/llm-wiki.md wiki/index.md
git commit -m "Add first project page (llm-wiki) and link it from index"
```

---

### Task 6: 전체 구조 최종 확인

**Files:** 없음 (검증 전용)

**Interfaces:**
- Consumes: Task 1~5의 모든 산출물.

- [ ] **Step 1: 전체 트리 확인**

Run: `find . -not -path './.git*' -not -path './.obsidian*' -type f | sort`

Expected:
```
./.gitignore
./CLAUDE.md
./docs/superpowers/plans/2026-07-16-llm-wiki-scaffold.md
./docs/superpowers/specs/2026-07-16-llm-wiki-design.md
./raw/articles/.gitkeep
./raw/assets/.gitkeep
./raw/decisions/.gitkeep
./raw/logs/.gitkeep
./wiki/concepts/.gitkeep
./wiki/decisions/.gitkeep
./wiki/index.md
./wiki/log.md
./wiki/projects/llm-wiki.md
```

- [ ] **Step 2: git 커밋 이력 확인**

Run: `git log --oneline`
Expected: 최소 5개 이상의 커밋 (spec 2개 + Task 1~5).

- [ ] **Step 3: 사용자에게 Obsidian 확인 요청**

자동화 불가 — 사용자에게 Obsidian 창에서 왼쪽 파일 탐색기에 `raw/`, `wiki/`, `CLAUDE.md`가
보이는지, `wiki/projects/llm-wiki.md`를 열었을 때 그래프 뷰에 index.md와의 링크가
표시되는지 확인해달라고 요청한다.
