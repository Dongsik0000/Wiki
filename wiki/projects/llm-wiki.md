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
