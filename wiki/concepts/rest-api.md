---
title: REST API
type: concept
tags: [spring, http, backend]
created: 2026-07-16
updated: 2026-07-16
related: ["[[jsp-spring-mvc]]", "[[jquery-ajax-vs-fetch]]"]
---

# REST API

**한 줄 요약:** URL과 HTTP 메서드(GET/POST 등)를 조합해서 서버 자원을 다루는 설계 방식이다. JSP 없이 JSON 데이터만 주고받을 때 사용한다.

## 개념

기존 방식은 URL 하나에 기능이 뒤섞인다 (`/board/getBoardList.do`, `/board/insertBoard.do`). REST는 URL은 자원(명사)만 표현하고, 동작은 HTTP 메서드로 구분한다.

## 주로 사용하는 상황

- JSP 화면이 아닌 Ajax로 데이터만 받아야 할 때
- 목록 동적 로딩, 폼 저장 후 새로고침 없이 결과 받기
- 모바일 앱이나 외부 시스템과 데이터를 주고받아야 할 때

## HTTP 메서드 역할

| 메서드 | 의미 | 주로 쓰는 상황 |
|---|---|---|
| GET | 조회 | 목록 조회, 상세 조회 |
| POST | 등록 | 데이터 저장, 로그인 |
| PUT | 전체 수정 | 전체 데이터 업데이트 |
| DELETE | 삭제 | 데이터 삭제 |

## 사용법 ① — Controller에 @RestController 선언

```java
// @Controller 대신 @RestController 사용 → 반환값이 JSON으로 자동 변환됨
@RestController
@RequestMapping("/api/board")
public class BoardApiController {

    @Autowired
    private BoardService boardService;

    // GET : 목록 조회
    @GetMapping("/list")
    public List<BoardVO> list() {
        return boardService.getBoardList(); // List가 JSON 배열로 변환됨
    }

    // GET : 단건 조회
    @GetMapping("/{id}")
    public BoardVO view(@PathVariable int id) {
        return boardService.getBoardDetail(id);
    }

    // POST : 등록
    @PostMapping("/insert")
    public Map<String, Object> insert(@RequestBody BoardVO vo) {
        boardService.insertBoard(vo);
        Map<String, Object> result = new HashMap<>();
        result.put("status", "success");
        result.put("message", "등록 완료");
        return result; // { "status": "success", "message": "등록 완료" }
    }

    // DELETE : 삭제
    @DeleteMapping("/{id}")
    public Map<String, Object> delete(@PathVariable int id) {
        boardService.deleteBoard(id);
        Map<String, Object> result = new HashMap<>();
        result.put("status", "success");
        return result;
    }
}
```

## 사용법 ② — JSP에서 Ajax로 호출

```javascript
var ctx = "${pageContext.request.contextPath}";

// GET 요청 (목록 조회)
$.ajax({
    url: ctx + "/api/board/list",
    type: "GET",
    dataType: "json",
    success: function(data) {
        // data = JSON 배열
        $.each(data, function(i, board) {
            $("#boardList").append("<tr><td>" + board.title + "</td></tr>");
        });
    }
});

// POST 요청 (등록)
var csrfHeader = "${_csrf.headerName}";
var csrfToken  = "${_csrf.token}";

$.ajax({
    url: ctx + "/api/board/insert",
    type: "POST",
    contentType: "application/json",
    headers: { [csrfHeader]: csrfToken },
    data: JSON.stringify({ title: "제목", content: "내용" }),
    success: function(result) {
        alert(result.message); // "등록 완료"
    }
});
```

## @Controller vs @RestController 비교

| 구분 | @Controller | @RestController |
|---|---|---|
| 반환값 | JSP 이름 (View) | JSON / 텍스트 데이터 |
| 사용 목적 | 화면 이동이 필요할 때 | Ajax 통신, 데이터 API |
| 파라미터 수신 | @RequestParam, VO | @RequestBody (JSON) |

> 💡 하나의 Controller에서 화면 이동과 API를 함께 쓰려면 `@Controller` + 메서드에 `@ResponseBody`를 붙이는 방법도 있다.
