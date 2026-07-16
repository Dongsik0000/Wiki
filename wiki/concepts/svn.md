---
title: SVN (Subversion)
type: concept
tags: [git-workflow, tools]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# SVN (Subversion)

**한 줄 요약:** 소스 코드를 중앙 서버에 저장하고 팀원들이 함께 코드를 커밋/업데이트하며 협업하는 버전 관리 시스템이다. eGovFramework은 Eclipse + SVN 조합을 많이 사용한다.

## 핵심 개념

| 용어 | 의미 |
|---|---|
| Repository | 코드가 저장되는 중앙 서버 |
| Trunk | 실제 개발 중인 메인 코드라인 |
| Branch | 특정 기능을 따로 개발할 때 만드는 분기 |
| Tag | 배포 시점의 코드를 스냅샷으로 백업 |
| Commit | 내 PC의 변경사항을 서버에 반영 |
| Update | 서버의 최신 코드를 내 PC로 가져오기 |
| Checkout | 리포지토리에서 코드를 처음 내려받기 |
| Conflict | 두 사람이 같은 파일을 수정했을 때 발생하는 충돌 |
| Revert | 코드를 이전 상태로 되돌리기 |

## 사용법 ① — Eclipse에서 Trunk 코드 처음 가져오기 (Checkout)

```
1. Eclipse 상단 메뉴 → Window → Open Perspective → SVN Repository Exploring
2. SVN Repositories 뷰 안에서 우클릭 → New → Repository Location
3. URL 입력 (ex. https://svn.company.com/repos/myproject)
4. trunk 폴더 확인 우클릭 → Checkout
5. 내 PC에 저장할 경로 선택 → Finish
6. Package Explorer에 프로젝트가 생김
```

## 사용법 ② — Update (서버의 최신 코드 받아오기)

팀원이 commit한 코드를 내 PC에 반영할 때. 코드 수정 전, 하루 시작 시 하는 것이 습관이 되어야 한다.

```
1. Package Explorer에서 프로젝트 또는 폴더/파일 선택
2. 우클릭 → Team → Update to HEAD
3. Console에 "최신 revision 123으로 업데이트됨" 표시되면 완료
4. Conflict 마크(C)가 있는 파일은 직접 충돌 해소 필요
```

## 사용법 ③ — Commit (내 코드를 서버에 올리기)

팀원들이 볼 수 있도록 변경사항을 서버에 반영하는 작업이다. commit 전에 반드시 Update 먼저.

```
1. 코드 수정 완료 후
2. 커밋할 파일들 선택
3. 우클릭 → Team → Commit
4. 코멘트 메시지 입력 (ex. "회원관리 목록 조회 기능 추가")
5. 업로드할 파일에 체크 확인 → OK
6. 로그에 "Committed revision 124" 표시되면 완료
```

> ⚠️ commit 전 Update 없이 commit하면 conflict 가능성이 높다. 모든 commit은 Update → conflict 해소 → 빌드 확인 → commit 순서로 하자.

## 사용법 ④ — Conflict 해소 (충돌 파일 수정)

같은 파일을 두 사람이 동시에 수정하면 conflict가 발생한다.

```
[conflict 발생 시 파일에 삽입되는 표시]

<<<<<<< .mine           ← 내가 수정한 코드
String title = "My Title";
=======
String title = "Other Title";   ← 다른 사람이 수정한 코드
>>>>>>> .r124
```

```
Conflict 해소 방법:

1. conflict 난 파일 우클릭 → Team → Edit Conflicts
2. 왼쪽: 내 코드 / 오른쪽: 서버 코드
3. 두 코드를 비교하면서 어떤 코드를 살릴지 판단
4. 모든 표시(<<<, ===, >>>) 제거
5. 우클릭 → Team → Mark as Merged
6. 다시 commit
```

## 사용법 ⑤ — Revert (코드 수정 취소 — commit 전)

```
1. 취소할 파일 선택
2. 우클릭 → Team → Revert
3. 내 수정사항이 사라지고 마지막 commit 상태로 복원됨
```

> ⚠️ Revert는 commit 전에만 사용 가능하다. 이미 commit된 코드를 되돌리려면 관리자에게 문의해야 한다.

## 사용법 ⑥ — 협업할 때 실제 작업 흐름

```
[하루 시작]
1. Eclipse 실행
2. 프로젝트 또는 전체 우클릭 → Team → Update to HEAD
   → 팀원들이 어제 올린 코드를 내 PC에 반영

[코딩 중]
3. 내 담당 파일 수정

[코딩 완료 후]
4. 다시 Update to HEAD (코딩하는 사이 다른 사람이 commit한 내용 반영)
5. Conflict가 있으면 해소
6. 빌드/실행해서 정상 동작 확인
7. 파일 선택 → Team → Commit → 코멘트 메시지 입력 → OK

[코멘트 메시지 작성 권장 형식]
"회원관리 - 목록 조회 기능 추가"
"게시판 - 등록 유효성 검사 로직 수정"
"버그 픽스 - 로그인 실패 시 500 에러 해결"
```

## SVN 상태 아이콘 의미

| 아이콘 | 의미 |
|---|---|
| ✨ (Yellow star) | 수정됨 (아직 commit 안 됨) |
| 🟢 (Green circle) | 정상, 서버와 동기화됨 |
| ⚠️ (Warning) | Conflict 발생 |
| ❓ (Question mark) | SVN 관리 대상이 아님 (새로 추가된 파일) |

> 💡 협업 시 가장 중요한 규칙: commit 전 Update, Update 후 conflict 확인, 코멘트 메시지는 작업 내용을 한 눈에 파악할 수 있게 구체적으로. 이 세 가지를 지켜야 팀원과의 충돌을 줄일 수 있다.
