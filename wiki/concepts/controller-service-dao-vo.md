---
title: Controller / Service / ServiceImpl / DAO / Mapper / VO
type: concept
tags: [spring, architecture]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mvc-pattern]]"]
---

# Controller / Service / ServiceImpl / DAO / Mapper / VO

**한 줄 요약:** 역할을 나눠서 코드를 관리하는 구조. 식당의 홀직원 → 주방장 → 요리사 → 창고지기처럼 각자 맡은 일이 있다.

## 각 레이어의 역할

| 이름 | 역할 | 비유 |
|---|---|---|
| Controller | 요청을 받아 Service에 넘기고 View로 반환 | 홀 직원 |
| Service (interface) | 비즈니스 로직의 명세 (무엇을 할지 선언) | 메뉴판 |
| ServiceImpl | Service의 실제 구현체, 핵심 로직 작성 | 주방장 |
| DAO | DB 접근 메서드를 선언하는 인터페이스 | 창고 관리자 |
| Mapper (.xml) | 실제 SQL이 작성되는 파일 (MyBatis) | 입출고 명세서 |
| VO | 데이터를 담아 레이어 간 전달하는 객체 | 배달 박스 |

## 주로 사용하는 상황

- 새로운 기능(조회/등록/수정/삭제) 하나를 만들 때마다 이 구조를 따른다
- eGovFramework은 이 구조가 기본 뼈대로 제공된다

## 전체 코드 흐름 예시 (게시글 목록 조회)

```java
// 1. BoardController.java
@Controller
@RequestMapping("/board")
public class BoardController {
    @Autowired
    private BoardService boardService;

    @RequestMapping("/list")
    public String list(Model model) {
        List<BoardVO> list = boardService.getBoardList();
        model.addAttribute("boardList", list); // JSP로 전달
        return "board/list";  // /WEB-INF/views/board/list.jsp
    }
}
```

```java
// 2. BoardService.java (인터페이스 - 선언만)
public interface BoardService {
    List<BoardVO> getBoardList();
}
```

```java
// 3. BoardServiceImpl.java (실제 로직 구현)
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

```java
// 4. BoardDAO.java (인터페이스 - DB 접근 선언)
public interface BoardDAO {
    List<BoardVO> selectBoardList();
}
```

```xml
<!-- 5. BoardMapper.xml (실제 SQL) -->
<mapper namespace="com.example.dao.BoardDAO">
    <select id="selectBoardList" resultType="com.example.vo.BoardVO">
        SELECT board_id, title, writer, reg_date
        FROM board
        ORDER BY reg_date DESC
    </select>
</mapper>
```

```java
// 6. BoardVO.java (데이터 운반 객체)
public class BoardVO {
    private int boardId;
    private String title;
    private String writer;
    private String regDate;
    // getter / setter
}
```

> 💡 왜 Service와 ServiceImpl을 분리하나? Controller는 인터페이스 타입만 알면 되기 때문에, 나중에 구현체를 교체하거나 테스트용 Mock 객체를 주입하기 쉬워진다.
