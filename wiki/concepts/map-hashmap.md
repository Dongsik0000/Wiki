---
title: Map & HashMap
type: concept
tags: [java, mybatis]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mybatis-resultmap]]"]
---

# Map & HashMap

**한 줄 요약:** 키-값(key-value) 형태로 데이터를 저장하는 자료 구조. eGovFramework에서 MyBatis parameterType, resultType으로 자주 사용한다.

## 파라미터(Parameter)란?

메서드나 SQL을 실행할 때 외부에서 넘겨주는 입력값이다. "이 기능을 실행하려면 뭘 알려줘야 하나?"에 해당하는 값이다.

```java
public BoardVO getBoard(int id)  // id가 파라미터
```

파라미터 개수에 따라 담는 방식이 달라진다.

| 상황 | 방법 | 예시 |
|---|---|---|
| 파라미터 1개 (기본 타입) | int, String 그대로 | parameterType=int |
| 파라미터 1개 (객체) | VO 그대로 | parameterType=BoardVO |
| 파라미터 여러 개 | Map에 담아서 전달 | parameterType=map |

Map은 인터페이스이고, HashMap은 그 구현체다. 키는 중복 불가하고, 하나의 키에 하나의 값만 매핑된다. VO를 따로 만들기 부담스러울 때 또는 여러 종류의 데이터를 한번에 담아야 할 때 사용한다.

## 사용법 1 - 기본 사용법 (Java)

```java
Map<String, Object> param = new HashMap<>();
param.put("page", 1);
param.put("size", 10);
param.put("keyword", "게시글");

int page = (int) param.get("page");
String kw = (String) param.get("keyword");

for (String key : param.keySet()) {
    System.out.println(key + " = " + param.get(key));
}
```

## 사용법 2 - parameterType=map (Mapper.xml 여러 파라미터 전달)

```xml
<select id="selectBoardList" parameterType="map" resultType="com.example.vo.BoardVO">
    SELECT board_id, title, writer, reg_date
    FROM board
    WHERE 1=1
    <if test="keyword != null and keyword != ''">
        AND title LIKE CONCAT('%', #{keyword}, '%')
    </if>
    ORDER BY reg_date DESC
    LIMIT #{size} OFFSET #{offset}
</select>
```

```java
Map<String, Object> param = new HashMap<>();
param.put("keyword", keyword);
param.put("size", 10);
param.put("offset", (page - 1) * 10);

List<BoardVO> list = boardDAO.selectBoardList(param);
```

## 사용법 3 - resultType=map (Mapper.xml 조회 결과를 Map으로 받기)

```xml
<select id="selectBoardSummary" resultType="map">
    SELECT board_id  AS boardId,
           title,
           writer,
           reg_date  AS regDate
    FROM board
    WHERE board_id = #{boardId}
</select>
```

```java
Map<String, Object> result = boardDAO.selectBoardSummary(1);
String title = (String) result.get("title");
```

## 사용법 4 - RestController에서 Ajax 응답 JSON 구성

```java
@PostMapping("/board/insert")
public Map<String, Object> insert(@RequestBody BoardVO vo) {
    boardService.insertBoard(vo);

    Map<String, Object> result = new HashMap<>();
    result.put("status", "success");
    result.put("message", "등록이 완료되었습니다.");
    result.put("boardId", vo.getBoardId());
    return result;
}
```

> 팁: 언제 Map을 쓰고 언제 VO를 쓸까? 파라미터가 3개 이하이거나 재사용이 없는 가벼운 쿼리는 Map이 편하고, 특정 엔티티의 CRUD처럼 구조가 명확할 때는 VO가 더 좋다.
