---
title: JSP & Spring MVC
type: concept
tags: [spring, jsp]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mvc-pattern]]", "[[rest-api]]"]
---

# JSP & Spring MVC

**한 줄 요약:** 브라우저 요청 → DispatcherServlet → Controller → Service → JSP 순서로 흐른다. Controller가 데이터를 Model에 담아 JSP로 넘기면, JSP가 HTML로 화면을 만든다.

## 개념 — 전체 요청 흐름

```
[브라우저 요청: /board/list]
    ↓
web.xml의 DispatcherServlet  → 모든 요청의 첫 관문, Spring에 위임
    ↓
HandlerMapping               → URL에 맞는 Controller 메서드 탐색
    ↓
Controller                   → 비즈니스 로직 처리 (Service 호출)
    ↓
Model                        → 처리 결과 데이터를 담는 그릇
    ↓
ViewResolver                 → 리턴한 문자열로 JSP 경로 결정
                               예) "board/list" → /WEB-INF/views/board/list.jsp
    ↓
JSP                          → Model의 데이터를 EL/JSTL로 HTML에 출력
    ↓
[브라우저에 화면 출력]
```

## 주로 사용하는 상황

- 화면(JSP)을 렌더링해서 응답해야 할 때 (목록, 상세, 폼 등)
- 폼 submit 후 처리 결과 페이지로 이동할 때 (redirect, forward)
- DB에서 데이터를 조회해서 JSP로 전달할 때

## 사용법 ① — Controller에서 데이터를 Model에 담기

```java
@Controller
@RequestMapping("/board")
public class BoardController {

    @Autowired
    private BoardService boardService;

    // 목록 페이지
    @RequestMapping("/list")
    public String list(Model model) {
        List<BoardVO> list = boardService.getBoardList();
        model.addAttribute("boardList", list);  // JSP에서 ${boardList}로 사용
        return "board/list";  // → /WEB-INF/views/board/list.jsp
    }

    // 상세 페이지
    @RequestMapping("/view")
    public String view(@RequestParam int id, Model model) {
        BoardVO board = boardService.getBoardDetail(id);
        model.addAttribute("board", board);  // JSP에서 ${board.title} 등으로 사용
        return "board/view";  // → /WEB-INF/views/board/view.jsp
    }

    // 폼 처리 후 리다이렉트
    @RequestMapping("/insert")
    public String insert(BoardVO vo) {
        boardService.insertBoard(vo);
        return "redirect:/board/list";  // 등록 후 목록으로 이동 (새로고침 해도 중복 등록 안 됨)
    }
}
```

## 사용법 ② — JSP에서 데이터 출력 (EL + JSTL)

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!-- 단일 값 출력 -->
<h2>${board.title}</h2>
<p>작성자: ${board.writer}</p>

<!-- 목록 반복 출력 -->
<table>
<c:forEach var="board" items="${boardList}">
    <tr>
        <td>${board.boardId}</td>
        <td><a href="<c:url value='/board/view'/>?id=${board.boardId}">${board.title}</a></td>
        <td>${board.writer}</td>
    </tr>
</c:forEach>
</table>

<!-- 조건 분기 -->
<c:if test="${empty boardList}">
    <p>게시글이 없습니다.</p>
</c:if>
```

## redirect vs forward 차이

| 구분 | 방식 | 브라우저 URL 변경 | 주요 용도 |
|---|---|---|---|
| `return "board/list"` | forward | 변경 안 됨 | 조회 후 화면 출력 |
| `return "redirect:/board/list"` | redirect | 변경됨 | 등록/수정/삭제 후 목록 이동 |

> ⚠️ 등록/수정/삭제 처리 후에는 반드시 `redirect:`를 써야 새로고침 시 중복 처리를 막을 수 있다.
