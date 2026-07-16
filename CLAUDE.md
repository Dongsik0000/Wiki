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
