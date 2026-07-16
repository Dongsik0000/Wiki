# LLM Wiki 사용법

Claude(에이전트)와 Obsidian(뷰어)을 이용한 개인 개발 지식 위키. 설계 배경은
`docs/superpowers/specs/2026-07-16-llm-wiki-design.md`, LLM이 따르는 스키마는
`CLAUDE.md`에 있다. 이 문서는 **사람이 실제로 어떻게 쓰는지**를 정리한다.

## 1. 구조

```
Wiki/
├── CLAUDE.md          # LLM이 세션 시작 시 자동으로 읽는 규칙 문서
├── README.md          # 이 문서 (사람용 사용법)
├── raw/               # 원본 소스 (불변) — articles/ decisions/ logs/ assets/
└── wiki/
    ├── index.md       # 전체 카탈로그 (프로젝트/개념/결정)
    ├── log.md         # 시간순 작업 이력
    ├── projects/      # 프로젝트별 진행 기록
    ├── concepts/      # 기술 개념 페이지
    └── decisions/     # 설계/아키텍처 결정 기록
```

- **raw/**: 절대 수정하지 않는다. 원본을 그대로 보관하거나(파일 복사), 출처만
  적어둔 매니페스트로 대체한다 (예: 노션처럼 살아있는 원본이 따로 있는 경우).
- **wiki/**: Claude가 쓰고 갱신하는 지식 페이지. 사람은 Obsidian으로 읽는다.

## 2. 하루하루 어떻게 쓰나

### (1) 자료 추가하기 (ingest)

형식 없이 그냥 말하면 된다.

> "오늘 Next.js App Router 관련 글 읽었는데 정리해줘: (내용 붙여넣기)"
> "이 프로젝트에서 Redux 대신 Zustand 쓰기로 했어. 이유는 보일러플레이트가 적어서."
> 노션/URL 등 외부 소스는 링크를 주면 가져와서 처리한다.

Claude가 하는 일: 핵심 내용 확인 → `wiki/concepts/` 또는 `wiki/decisions/`,
`wiki/projects/`에 페이지 생성/갱신 → `wiki/index.md`, `wiki/log.md` 갱신.

### (2) 질문하기 (query)

> "예전에 상태관리 뭐 쓰기로 했었지?"
> "지금까지 배운 React 관련 개념 요약해줘"

`wiki/index.md`에서 관련 페이지를 찾아 읽고 출처와 함께 답한다. 답이 재사용
가치가 있으면 새 페이지로 저장할지 먼저 물어본다.

### (3) 점검하기 (lint)

> "위키 한번 점검해줘"

모순되는 내용, 오래된 정보, 고아 페이지(링크 없는 페이지), 누락된
상호참조를 찾아 보고한다. 위키가 어느 정도 쌓인 뒤 가끔 실행하면 된다.

## 3. 새 세션에서 이어서 쓰기

- Claude Code를 이 폴더(`C:\Users\ds_ki\Desktop\Wiki`)에서 실행하면 `CLAUDE.md`를
  자동으로 읽기 때문에, 대화가 끊겨도 규칙을 다시 설명할 필요가 없다.
- Obsidian은 최근 vault 목록에서 "Wiki"를 클릭하면 바로 열린다.
- 지금까지 뭘 했는지 궁금하면 `wiki/log.md`를 보면 된다 (`grep "^## \[" wiki/log.md`
  로 이력만 빠르게 조회 가능).

## 4. 다른 프로젝트 작업 내용을 위키에 정리하기

Claude Code로 실제 코드 프로젝트를 작업하는 세션과 이 위키 세션은 보통
별개다. 두 방법이 있다.

**방법 1 (추천):** 프로젝트 폴더에서 평소처럼 작업 → 어느 정도 끝나면 이 Wiki
폴더에서 새 세션을 열고 "오늘 `프로젝트명`에서 이런 작업 했어" 라고 요약을
붙여넣는다 (`git log --oneline -20` 출력도 좋다). 위키 세션은 `CLAUDE.md`
규칙을 이미 알고 있어서 형식에 맞게 바로 정리해준다.

**방법 2:** 프로젝트 세션에서 Claude에게 절대경로(`C:\Users\ds_ki\Desktop\Wiki\raw\logs\...`)
를 알려주고 그 경로에 저장하게 한다. 다만 그 세션은 위키 규칙을 자동으로
읽지 않으므로, 원본만 던져두고 나중에 방법 1로 정리하는 편이 깔끔하다.

## 5. Obsidian 팁

- **그래프 뷰**: `Ctrl+P` → "graph" 검색 → "Open graph view". 페이지 간
  연결(`related` frontmatter, 위키링크)이 선으로 보인다.
- **백링크**: 페이지 하단에 "N개 백링크"로 표시되며, 어떤 페이지가 지금 보는
  페이지를 참조하는지 확인할 수 있다.
- **첨부파일 경로**: Settings → Files and links → Attachment folder path를
  `raw/assets`로 지정해두면, 이미지를 붙여넣을 때 자동으로 그 폴더에 저장된다
  (아직 설정 안 했다면 한 번 해두면 편하다).
- vault 설정(`.obsidian/`)은 git으로 추적하지 않는다 (`.gitignore` 참고).

## 6. 페이지 규칙 (참고용, 자세한 건 CLAUDE.md)

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

- `wiki/index.md`: `- [[페이지명]] — 한줄 요약 (업데이트일)` 형식으로 카테고리별 목록
- `wiki/log.md`: `## [YYYY-MM-DD] <종류> | <제목>` 형식으로 append-only 기록
  (종류: ingest / query / lint / setup)
