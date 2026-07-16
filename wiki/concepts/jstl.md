---
title: JSTL (JSP Standard Tag Library)
type: concept
tags: [jsp]
created: 2026-07-16
updated: 2026-07-16
related: ["[[el-expression-language]]", "[[jsp-include]]"]
---

# JSTL (JSP Standard Tag Library)

**한 줄 요약:** JSP에서 Java 코드 대신 태그 형태로 조건문, 반복문, 변수 선언을 처리하는 표준 라이브러리다. 스크립틀릿 없이 깔끔하게 동적 HTML을 만들 수 있다.

## 개념 - 왜 쓰는가?

JSP 안에 Java 코드(스크립틀릿)를 직접 넣으면 HTML과 뒤섞여 가독성이 매우 나빠진다. JSTL은 c:if, c:forEach 같은 태그로 Java 로직을 대체해 HTML처럼 읽기 쉽게 만든다.

## taglib 선언 - 반드시 JSP 상단에 추가

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!-- prefix="c" -> c:forEach, c:if 등에 사용 -->
<!-- prefix="fmt" -> fmt:formatDate, fmt:formatNumber 등에 사용 -->
```

## c:forEach - 반복 출력

```jsp
<!-- 기본: 리스트 순회 -->
<c:forEach var="board" items="${boardList}">
    <tr>
        <td>${board.boardId}</td>
        <td>${board.title}</td>
        <td>${board.writer}</td>
    </tr>
</c:forEach>

<!-- 인덱스 포함: varStatus로 현재 순번 접근 -->
<c:forEach var="board" items="${boardList}" varStatus="status">
    <tr>
        <td>${status.index}</td>   <!-- 0부터 시작 -->
        <td>${status.count}</td>   <!-- 1부터 시작 -->
        <td>${board.title}</td>
    </tr>
</c:forEach>

<!-- 숫자 범위 반복 (begin~end) -->
<c:forEach var="i" begin="1" end="5">
    <span>${i}</span>   <!-- 1 2 3 4 5 -->
</c:forEach>
```

## c:if - 단순 조건

```jsp
<!-- test 조건이 true일 때만 렌더링 -->
<c:if test="${not empty boardList}">
    <p>게시글이 있습니다.</p>
</c:if>

<c:if test="${empty boardList}">
    <p>게시글이 없습니다.</p>
</c:if>

<!-- c:if는 else가 없다. else가 필요하면 c:choose 사용 -->
```

## c:choose - if-else if-else

```jsp
<c:choose>
    <c:when test="${board.status == 'Y'}">
        <span style="color:green">승인완료</span>
    </c:when>
    <c:when test="${board.status == 'N'}">
        <span style="color:red">반려</span>
    </c:when>
    <c:otherwise>
        <span style="color:gray">검토중</span>
    </c:otherwise>
</c:choose>
```

## c:set / c:out / c:remove - 변수 관리

```jsp
<!-- c:set: 변수 선언 -->
<c:set var="pageTitle" value="회원 목록" />
<h2>${pageTitle}</h2>

<!-- c:out: XSS 방지 출력 (< > & 등을 자동 이스케이프) -->
<c:out value="${board.title}" />

<!-- c:remove: 변수 제거 -->
<c:remove var="pageTitle" />
```

## fmt:formatDate / fmt:formatNumber - 포맷 변환

```jsp
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

<!-- 날짜 포맷 -->
<fmt:formatDate value="${board.regDate}" pattern="yyyy-MM-dd" />

<!-- 숫자 포맷 (천 단위 쉼표) -->
<fmt:formatNumber value="${board.price}" pattern="#,###" />
```

## 자주 쓰는 EL 내장 함수

```
${empty boardList}        조건: null이거나 빈 컬렉션이면 true
${not empty boardList}    조건: 값이 있으면 true
${boardList.size()}       리스트 크기
${board.title.length()}   문자열 길이
```

## 스크립틀릿 vs JSTL 비교

```jsp
<%-- 스크립틀릿 방식 (가독성 나쁨) --%>
<% for(BoardVO board : boardList) { %>
    <tr><td><%= board.getTitle() %></td></tr>
<% } %>

<%-- JSTL 방식 (HTML처럼 읽기 쉬움) --%>
<c:forEach var="board" items="${boardList}">
    <tr><td>${board.title}</td></tr>
</c:forEach>
```

> 팁: eGovFramework에서는 JSP에 Java 코드(스크립틀릿)를 직접 쓰지 않는 것이 원칙이다. 조건, 반복은 JSTL, 값 출력은 EL로 처리한다. 특히 c:forEach와 c:if는 거의 모든 목록 페이지에서 사용하므로 반드시 익혀두자.
