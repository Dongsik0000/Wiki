---
title: "contextPath vs <c:url>"
type: concept
tags: [jsp]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# contextPath vs `<c:url>`

**한 줄 요약:** 둘 다 URL 경로를 올바르게 만들어주는 도구. contextPath는 직접 루트 경로를 붙이는 방식이고, `<c:url>`은 JSTL이 자동으로 처리해주는 방식이다.

## 개념

웹 애플리케이션이 `/myapp` 이라는 컨텍스트 경로로 배포되어 있을 때, CSS나 JS 경로를 `/css/style.css`로만 쓰면 서버 루트 기준이 되어 경로가 틀어진다. 항상 앞에 컨텍스트 경로를 붙여야 정확한 경로가 된다.

## 주로 사용하는 상황

- JSP에서 CSS, JS, 이미지 파일 경로를 지정할 때
- `<a href>`, `<form action>` 등 링크 경로를 만들 때
- Ajax의 URL을 동적으로 만들 때

## 사용법 ① : contextPath 직접 사용 (JS 변수 포함 시 필수)

```jsp
<%-- CSS/JS 경로 --%>
<link rel="stylesheet" href="${pageContext.request.contextPath}/css/style.css">
<script src="${pageContext.request.contextPath}/js/common.js"></script>

<%-- 링크 경로 --%>
<a href="${pageContext.request.contextPath}/board/list">목록</a>

<%-- JavaScript 변수에 담아 Ajax에서 활용 --%>
<script>
    var ctx = "${pageContext.request.contextPath}";

    $.ajax({
        url: ctx + "/board/delete",
        ...
    });
</script>
```

## 사용법 ② : `<c:url>` 태그 사용 (JSP 태그 속성에 간결하게)

```jsp
<%-- 페이지 상단에 JSTL 선언 필요 --%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<%-- CSS 경로 --%>
<link rel="stylesheet" href="<c:url value='/css/style.css'/>">

<%-- 링크 경로 --%>
<a href="<c:url value='/board/list'/>">목록</a>

<%-- 파라미터 포함 URL 자동 생성 --%>
<c:url value="/board/view" var="viewUrl">
    <c:param name="id" value="${board.id}"/>
</c:url>
<a href="${viewUrl}">상세보기</a>  <%-- /myapp/board/view?id=1 로 자동 생성 --%>
```

## 방식 비교

| 방식 | 사용 위치 | 특징 |
|---|---|---|
| `${pageContext.request.contextPath}` | JSP EL, JS 변수 | JS에서 활용 가능, 직관적 |
| `<c:url value='...'/>` | JSP 태그 속성 | 파라미터 자동 처리, 간결 |

> ⚠️ JavaScript 코드 안에서는 `<c:url>` 태그를 쓸 수 없다. JS 변수에는 반드시 `${pageContext.request.contextPath}`를 사용할 것.
