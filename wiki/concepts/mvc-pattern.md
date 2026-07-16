---
title: MVC 패턴 (Model - View - Controller)
type: concept
tags: [spring, architecture]
created: 2026-07-16
updated: 2026-07-16
related: ["[[controller-service-dao-vo]]", "[[jsp-spring-mvc]]"]
---

# MVC 패턴 (Model - View - Controller)

**한 줄 요약:** 웹 애플리케이션을 **데이터(Model)**, **화면(View)**, **요청 처리(Controller)** 세 영역으로 나눠서 관리하는 설계 패턴이다. Spring MVC는 이 패턴을 프레임워크 수준에서 지원한다.

## MVC가 나누는 이유

MVC 없이 하나의 파일에 모든 코드를 넣으면 코드가 일정 기준 이상으로 길어지면 어떤 코드가 어느 역할을 하는지 파악하기 어려워지고 유지보수가 힘들어진다. MVC는 각 역할을 분리해 수정이 한 곳에만 영향을 미치도록 한다.

| 구성요소 | 역할 | eGov에서의 해당 |
|---|---|---|
| Model | 데이터와 비즈니스 로직 | Service, ServiceImpl, DAO, Mapper, VO |
| View | 사용자에게 보여주는 화면 | JSP |
| Controller | 요청을 받아 Model과 View를 연결 | Controller.java |

## MVC 요청 흐름 (Spring MVC)

```
[브라우저] /board/list 요청
    ↓
[DispatcherServlet]  ← 모든 요청의 첫 관문 (web.xml에 설정)
    ↓
[HandlerMapping]     ← URL에 맞는 Controller 탐색
    ↓
[Controller]         ← 요청 수신, Service 호출
    ↓
[Service / DAO]      ← 비즈니스 로직 + DB 조회  ← Model 역할
    ↓
[Controller]         ← 결과를 Model에 담아 View 이름 반환
    ↓
[ViewResolver]       ← View 이름으로 JSP 경로 결정
    ↓
[JSP]                ← Model 데이터를 HTML로 렌더링  ← View 역할
    ↓
[브라우저에 화면 출력]
```

## Model — 데이터와 로직

```java
// VO.java - 데이터를 담는 객체 (Model의 데이터 부분)
public class BoardVO {
    private int boardId;
    private String title;
    private String writer;
    // getter / setter
}

// ServiceImpl.java - 비즈니스 로직 (Model의 로직 부분)
@Service
public class BoardServiceImpl implements BoardService {
    @Autowired
    private BoardDAO boardDAO;

    @Override
    public List<BoardVO> getBoardList() {
        return boardDAO.selectBoardList();
    }
}
```

## Controller — 요청을 받아 Model과 View를 연결

```java
@Controller
@RequestMapping("/board")
public class BoardController {

    @Autowired
    private BoardService boardService;

    @RequestMapping("/list")
    public String list(Model model) {
        List<BoardVO> list = boardService.getBoardList(); // ① Model에서 데이터 조회
        model.addAttribute("boardList", list);            // ② View에 데이터 넣기
        return "board/list"; // ③ 보여줄 View 지정
    }
}
```

## View — 데이터를 화면에 출력

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<table>
    <c:forEach var="board" items="${boardList}">
        <tr>
            <td>${board.boardId}</td>
            <td>${board.title}</td>
            <td>${board.writer}</td>
        </tr>
    </c:forEach>
</table>
```

## 정리

| 질문 | 답 |
|---|---|
| 요청 URL을 어디서 받나? | Controller (`@RequestMapping`) |
| DB 조회는 어디에서 하나? | DAO + Mapper.xml (Model) |
| 화면은 어디서 만드나? | JSP (View) |
| 직접 DB 연동 코드가 JSP에 있나? | ❌ MVC로 분리해야 올바른 구조 |

> 💡 eGovFramework = Spring MVC + MyBatis. Controller-Service-DAO-Mapper-VO 구조가 바로 Spring MVC 패턴의 실제 적용이다.
