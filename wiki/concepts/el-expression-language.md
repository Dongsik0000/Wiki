---
title: "EL (Expression Language)"
type: concept
tags: [jsp]
created: 2026-07-16
updated: 2026-07-16
related: ["[[jstl]]"]
---

# EL (Expression Language)

**한 줄 요약:** JSP에서 ${} 안에 변수나 표현식을 넣으면 값을 자동으로 출력해주는 문법이다. JSTL과 함께 사용하며, Java 코드 없이 데이터를 화면에 뿌릴 수 있다.

## 개념

Controller에서 model.addAttribute("key", value)로 담은 데이터를 JSP에서 ${key}로 꺼내 쓴다. Java의 getter를 자동으로 호출해주므로 ${board.title}은 내부적으로 board.getTitle()을 실행한다.

## 기본 사용법

```jsp
<%-- Controller에서 model.addAttribute("userName", "홍길동") 했을 때 --%>
<p>${userName}</p>           <%-- 홍길동 --%>
<p>${board.title}</p>        <%-- board.getTitle() 자동 호출 --%>
<p>${board.writer}</p>       <%-- board.getWriter() 자동 호출 --%>
<p>${boardList.size()}</p>   <%-- 리스트 크기 --%>
```

## EL에서 사용 가능한 내장 객체

```jsp
${param.keyword}                   <%-- URL 파라미터: ?keyword=홍길동 --%>
${sessionScope.LOGIN_USER_VO}      <%-- 세션에 저장된 로그인 정보 --%>
${requestScope.boardList}          <%-- request 범위 데이터 --%>
${pageContext.request.contextPath} <%-- contextPath 가져오기 --%>
```

## EL 연산자

```jsp
<%-- 산술 --%>
${3 + 2}      <%-- 5 --%>
${10 / 3}     <%-- 3.3333 --%>
${10 mod 3}   <%-- 1 (나머지) --%>

<%-- 비교 --%>
${board.boardId == 1}
${board.cnt >= 10}

<%-- 논리 --%>
${empty boardList and param.keyword == ''}
${not empty boardList or board.cnt > 0}

<%-- 삼항 연산자 --%>
${board.status == 'Y' ? '승인' : '미승인'}
```

## 자주 헷갈리는 케이스

```jsp
<%-- null / 빈값 처리 --%>
${board.title}          <%-- null이면 아무것도 출력 안 함 (오류 없음) --%>
${empty board.title}    <%-- null이거나 빈 문자열이면 true --%>

<%-- Map에서 값 꺼내기 --%>
${resultMap.totalCount}  <%-- map.get("totalCount") --%>
${resultMap["key"]}      <%-- 키에 특수문자가 있을 때 --%>

<%-- 배열/리스트 인덱스 접근 --%>
${boardList[0].title}    <%-- 첫 번째 요소 --%>
```

## EL vs 스크립틀릿 비교

```jsp
<%-- 스크립틀릿 --%>
<%= board.getTitle() %>
<%= board.getWriter() %>

<%-- EL --%>
${board.title}
${board.writer}
```

> 팁: ${}는 null-safe다. Java에서는 null인 변수를 출력하면 "null" 문자열이 나오지만, EL에서는 null이면 아무것도 출력하지 않는다. 단, empty 연산자로 null/빈값 여부를 체크할 때는 ${empty 변수}를 사용한다.
