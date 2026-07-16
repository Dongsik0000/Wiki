---
title: CSRF (Cross-Site Request Forgery)
type: concept
tags: [security, spring]
created: 2026-07-16
updated: 2026-07-16
related: ["[[jquery-ajax-vs-fetch]]"]
---

# CSRF (Cross-Site Request Forgery)

**한 줄 요약:** 사용자 몰래 다른 사이트에서 요청을 보내는 공격. Spring Security가 토큰으로 자동 차단한다.

## 개념

로그인된 상태에서 악성 사이트를 방문하면, 그 사이트가 내 권한을 도용해 서버에 몰래 요청(글 삭제, 비밀번호 변경 등)을 보낼 수 있다. CSRF 토큰은 서버가 발급하고, 요청 시 이 토큰을 같이 보내야만 정상 처리되어 공격을 차단한다.

## 주로 사용하는 상황

- JSP 폼에서 POST 요청을 보낼 때
- Ajax로 POST/PUT/DELETE 요청을 보낼 때
- eGovFramework 프로젝트에서 403 에러가 발생할 때 (CSRF 토큰 누락이 원인인 경우가 많다)

## 사용법 1 : JSP 폼에 추가

```jsp
<form method="post" action="/board/insert">
    <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>
    <input type="text" name="title"/>
    <button type="submit">등록</button>
</form>
```

## 사용법 2 : Ajax 요청에 헤더로 추가

```javascript
var csrfHeader = "${_csrf.headerName}";
var csrfToken = "${_csrf.token}";

$.ajax({
    url: "/board/delete",
    type: "POST",
    headers: { [csrfHeader]: csrfToken },
    data: { id: boardId },
    success: function(result) {
        alert("삭제 완료");
    },
    error: function(xhr) {
        alert("오류: " + xhr.status);
    }
});
```

## 개발 중 CSRF 비활성화 방법 (테스트용 한정)

```xml
<security:http>
    <security:csrf disabled="true"/>  <!-- 운영 환경에서는 절대 사용 금지 -->
</security:http>
```

> 경고: 운영 환경에서는 반드시 CSRF를 활성화해야 한다. 403 에러가 갑자기 생기면 CSRF 토큰 누락부터 확인하자.
