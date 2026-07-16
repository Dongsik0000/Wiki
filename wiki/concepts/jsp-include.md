---
title: "JSP include - 공통 레이아웃 재사용"
type: concept
tags: [jsp]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# JSP include - 공통 레이아웃 재사용

**한 줄 요약:** 헤더, 푸터, 사이드바 같은 공통 영역을 매 JSP마다 반복 작성하지 않고 한 곳에 만들어두고 include로 가져다 쓰는 방법이다.

## 개념 - 왜 필요한가?

모든 페이지에 헤더를 복붙하면, 헤더가 바뀔 때 모든 JSP를 수정해야 한다. include를 쓰면 공통 파일 하나만 수정하면 전체에 반영된다.

## 방법 1 - JSP 지시자 include (정적 포함)

```jsp
<!-- 컴파일 시점에 파일 내용을 그대로 붙여 넣음 -->
<!-- 변수 공유 가능, 헤더/푸터처럼 고정된 내용에 사용 -->
<%@ include file="/WEB-INF/views/common/header.jsp" %>

<div class="content">
    <!-- 각 페이지 고유 내용 -->
</div>

<%@ include file="/WEB-INF/views/common/footer.jsp" %>
```

## 방법 2 - jsp:include 액션 태그 (동적 포함)

```jsp
<!-- 런타임에 별도 요청으로 포함. param으로 값 전달 가능 -->
<jsp:include page="/WEB-INF/views/common/header.jsp">
    <jsp:param name="pageTitle" value="게시판 목록"/>
</jsp:include>

<div class="content">
    <!-- 각 페이지 고유 내용 -->
</div>

<jsp:include page="/WEB-INF/views/common/footer.jsp"/>
```

## 공통 파일 구조 예시

```
/WEB-INF/views/
├── common/
│   ├── header.jsp    <- 상단 GNB, 로그인 정보
│   ├── footer.jsp    <- 하단 정보, 저작권
│   ├── sidebar.jsp   <- 좌측 메뉴
│   └── head.jsp      <- head 태그, CSS/JS 링크
├── board/
│   ├── list.jsp
│   └── view.jsp
└── user/
    └── login.jsp
```

## 실제 list.jsp 전체 구조

```jsp
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <%@ include file="/WEB-INF/views/common/head.jsp" %>
    <title>게시판 목록</title>
</head>
<body>
    <%@ include file="/WEB-INF/views/common/header.jsp" %>
    <%@ include file="/WEB-INF/views/common/sidebar.jsp" %>

    <div class="main-content">
        <!-- 이 페이지만의 고유 내용 -->
    </div>

    <%@ include file="/WEB-INF/views/common/footer.jsp" %>
</body>
</html>
```

> 팁: 퍼블리셔에게 받은 HTML의 정적 헤더/푸터는 JSP로 변환할 때 공통 파일로 분리하고 include로 교체한다. 공통 파일 하나만 수정하면 모든 페이지에 반영되므로 유지보수가 훨씬 쉬워진다.
