---
title: "jQuery Ajax ($.ajax) vs fetch API"
type: concept
tags: [javascript, http]
created: 2026-07-16
updated: 2026-07-16
related: ["[[rest-api]]", "[[csrf]]"]
---

# jQuery Ajax ($.ajax) vs fetch API

**한 줄 요약:** 둘 다 페이지 새로고침 없이 서버와 통신하는 방법. `$.ajax`는 jQuery 방식으로 eGovFramework에서 주로 사용하고, `fetch`는 브라우저 내장 순수 JS 방식이다.

## 개념

기존 폼 submit은 페이지 전체를 다시 로드하지만, Ajax는 폼 submit 없이 비동기적으로 요청을 보내고 서버의 응답(JSON)만 받아 화면 일부를 업데이트할 수 있다.

## 두 방식 비교

| 구분 | $.ajax (jQuery) | fetch (순수 JS) |
|---|---|---|
| 라이브러리 | jQuery 필요 | 브라우저 내장, 불필요 |
| 문법 | 콜백 기반 (success/error) | Promise 기반 (.then/.catch) |
| HTTP 오류 처리 | 4xx/5xx 자동 error 콜백 | 4xx/5xx도 성공 처리됨, 직접 체크 필요 |
| JSON 파싱 | dataType: json 자동 | .json() 별도 호출 필요 |
| CSRF 처리 | headers 옵션에 추가 | headers 옵션에 추가 (동일) |
| eGov 환경 | 주로 사용 | 가능하지만 드물다 |

## 주로 사용하는 상황

- $.ajax: jQuery가 이미 포함된 eGovFramework JSP 환경 (대부분 이걸 사용)
- fetch: jQuery 없이 개발할 때, React/Vue 등 프레임워크, 최신 JS 환경
- 목록에서 몇 가지를 선택하고 삭제/수정할 때 (페이지 새로고침 없이)
- 콤보박스 선택 시 연관 데이터를 동적으로 로딩할 때

## $.ajax 기본 사용법

```javascript
var ctx = "${pageContext.request.contextPath}";

$.ajax({
    url: ctx + "/api/board/list",
    type: "GET",
    dataType: "json",
    data: { page: 1, size: 10 },
    success: function(data) {
        $("#boardList").empty();
        $.each(data, function(i, board) {
            $("#boardList").append("<tr><td>" + board.title + "</td></tr>");
        });
    },
    error: function(xhr, status, err) {
        console.error("status:", xhr.status, "error:", err);
        alert("조회 실패: " + xhr.status);
    }
});

var csrfHeader = "${_csrf.headerName}";
var csrfToken  = "${_csrf.token}";

$.ajax({
    url: ctx + "/api/board/insert",
    type: "POST",
    contentType: "application/json",
    headers: { [csrfHeader]: csrfToken },
    data: JSON.stringify({ title: "제목" }),
    success: function(result) {
        alert(result.message);
    },
    error: function(xhr) {
        if (xhr.status === 403) alert("CSRF 토큰 오류");
        else alert("오류 발생: " + xhr.status);
    }
});
```

## fetch 사용법

```javascript
const ctx        = "${pageContext.request.contextPath}";
const csrfHeader = "${_csrf.headerName}";
const csrfToken  = "${_csrf.token}";

fetch(ctx + "/api/board/list?page=1")
    .then(res => {
        if (!res.ok) throw new Error("오류: " + res.status);
        return res.json();
    })
    .then(data => console.log(data))
    .catch(err => alert(err.message));

fetch(ctx + "/api/board/insert", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        [csrfHeader]: csrfToken
    },
    body: JSON.stringify({ title: "제목", content: "내용" })
})
    .then(res => res.json())
    .then(result => alert(result.message))
    .catch(err => alert(err.message));
```

## fetch의 핵심 주의점

fetch는 404, 500도 성공으로 처리한다. response.ok 체크를 빠뜨리면 오류를 못 잡는다.

## 오류 디버깅 팁

| 상황 | 확인 방법 |
|---|---|
| Ajax 요청이 안 날아감 | 개발자도구(F12) Network 탭에서 URL/실패 코드 확인 |
| 403 에러 | CSRF 토큰 누락 여부 확인 |
| 400 에러 | 요청 데이터 형식 불일치 확인 |
| 500 에러 | 서버 내부 오류, Eclipse 콘솔 로그 확인 |

> 팁: eGovFramework에서는 $.ajax를 권장한다. jQuery가 이미 포함되어 있고, 에러 처리가 더 직관적이며 CSRF 처리 방식도 동일하다.
