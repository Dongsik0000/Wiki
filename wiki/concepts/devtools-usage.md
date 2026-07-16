---
title: "개발자 도구 사용법 (F12, Chrome DevTools)"
type: concept
tags: [debugging, browser, tools]
created: 2026-07-16
updated: 2026-07-16
related: ["[[console-log-debugging]]", "[[fiddler]]", "[[http-status-codes]]"]
---

# 개발자 도구 사용법 (F12, Chrome DevTools)

**한 줄 요약:** 브라우저에 내장된 디버깅 도구다. HTML 구조 확인, Ajax 요청/응답 추적, 브레이크포인트 디버깅, 저장소 확인까지 프론트엔드 개발의 모든 문제를 여기서 해결한다.

## 열기 / 닫기

| 방법 | 설명 |
|---|---|
| F12 | 개발자 도구 열기/닫기 |
| Ctrl + Shift + I | 개발자 도구 열기/닫기 |
| Ctrl + Shift + J | Console 탭 바로 열기 |
| Ctrl + Shift + C | Elements 탭 + 요소 선택 모드 |

## 탭 구성 - 자주 쓰는 탭

| 탭 | 주요 용도 |
|---|---|
| Elements | HTML/CSS 구조 확인, 실시간 스타일 수정 |
| Console | JS 로그 확인, JS 직접 실행 |
| Network | Ajax 요청/응답 확인, 상태코드 확인 |
| Sources | 브레이크포인트 설정, 단계별 디버깅 |
| Application | localStorage, sessionStorage, 쿠키 확인 |

## Elements 탭 - HTML/CSS 확인

요소 선택(Ctrl+Shift+C 또는 좌상단 커서 아이콘)으로 화면에서 원하는 요소를 클릭하면 Elements 탭에서 해당 태그로 이동한다. 태그를 더블클릭하면 내용을 임시로 수정할 수 있고, 우측 Styles 패널에서 CSS 속성을 확인/수정(새로고침하면 원복)할 수 있다. 내가 추가한 id가 실제로 HTML에 붙어있는지, querySelector가 왜 작동 안 하는지 확인할 때 여기서 구조를 먼저 본다.

## Console 탭 - 로그 확인 / JS 직접 실행

console.log로 찍은 데이터가 여기 나온다. 직접 JS를 입력해서 실행할 수도 있다.

```javascript
document.querySelector('#userMgmt-table-body')  // 요소 존재 확인
localStorage.getItem('board_keyword')            // 저장된 값 확인
$('#userMgmt-modal').is(':visible')              // 모달 표시 여부 확인
```

빨간색 에러 로그가 뜨면 파일명:줄번호가 표시되고, 클릭하면 해당 코드로 이동한다.

## Network 탭 - Ajax 요청/응답 추적 (가장 자주 씀)

Ajax 호출했는데 데이터가 안 오거나 오류가 날 때, 서버에 어떤 값이 전달됐는지 확인할 때 연다.

```
1. F12 -> Network 탭 클릭
2. 페이지에서 Ajax 요청 발생 (버튼 클릭, 검색 등)
3. 목록에서 해당 요청 클릭

하위 탭:
- Headers  : 요청 URL, 메서드(GET/POST), 상태코드 확인
- Payload  : 서버에 보낸 파라미터 값 확인
- Preview  : 응답 데이터를 보기 좋게 정렬해서 보여줌
- Response : 응답 원문 (JSON 문자열)

필터: Fetch/XHR 클릭하면 Ajax 요청만 필터링 (HTML/JS/CSS 제외)

디버깅 체크 순서:
1. Status: 상태코드 확인 (200/400/403/404/500)
2. Payload: 파라미터가 제대로 전달됐는지 확인
3. Preview: 응답 데이터 구조 확인
```

## Sources 탭 - 브레이크포인트 디버깅

브레이크포인트는 코드 실행을 특정 줄에서 멈추게 해서 그 시점의 변수 값, 실행 흐름을 확인하는 기능이다.

```
1. F12 -> Sources 탭
2. 좌측 파일 트리에서 디버깅할 JS 파일 선택 (Ctrl+P로 파일명 검색)
3. 멈추고 싶은 줄 번호 클릭 -> 파란 점 표시됨
4. 페이지에서 해당 코드 실행 -> 브레이크포인트에서 멈춤
```

| 단축키 | 동작 | 설명 |
|---|---|---|
| F8 | Resume | 다음 브레이크포인트까지 계속 실행 |
| F10 | Step Over | 현재 줄 실행 후 다음 줄로 이동 (함수 안으로 안 들어감) |
| F11 | Step Into | 현재 줄의 함수 안으로 들어가서 실행 |
| Shift+F11 | Step Out | 현재 함수에서 빠져나옴 |

멈춘 후에는 우측 Scope 패널에서 현재 시점의 변수 값을 확인하거나, Console 탭에서 변수명을 입력해 값을 직접 확인할 수 있다.

## Application 탭 - 저장소 확인

Storage > Local Storage / Session Storage > 도메인 선택하면 저장된 key/value 목록을 확인할 수 있고, 값을 클릭해서 수정하거나 우클릭으로 삭제할 수 있다. Cookies에서는 JSESSIONID 등 쿠키 목록을 확인한다.

## 실무 디버깅 흐름 - Ajax 오류 발생 시

```
[상황] 버튼 클릭했는데 데이터가 안 나올 때

Step 1. Console 탭 확인
        -> 빨간 에러 로그가 있으면 원인 파악

Step 2. Network 탭 확인 (Fetch/XHR 필터)
        -> 요청이 발생했는지 확인
        -> Status 코드 확인 (400? 403? 404? 500?)
        -> Payload / Preview 확인

Step 3. 상태코드에 따라 조치
        -> 400: 파라미터 타입/이름 불일치 확인
        -> 403: CSRF 토큰 누락 확인
        -> 404: URL, Controller 매핑 확인
        -> 500: Eclipse 콘솔에서 Java 오류 메시지 확인

Step 4. Sources 탭 브레이크포인트
        -> 값이 Ajax에 제대로 담겨서 가는지 직접 확인
```

> 팁: 개발자 도구를 열어두고 개발하는 습관을 들이자. Ajax 호출 시 Network 탭에서 요청/응답을 바로 확인하고, 오류 발생 시 Console 탭의 빨간 에러 로그를 먼저 읽는 것이 가장 빠른 디버깅 방법이다. Ctrl+Shift+R (강력 새로고침)은 JS/CSS가 캐시에 묶여서 수정이 반영 안 될 때 가장 먼저 해봐야 할 것이다.
