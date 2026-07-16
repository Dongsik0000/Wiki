---
title: 페이징 처리 (Pagination)
type: concept
tags: [mybatis, sql, backend]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mybatis-dynamic-sql]]"]
---

# 페이징 처리 (Pagination)

**한 줄 요약:** 데이터가 많을 때 한 번에 전부 보여주지 않고 페이지 단위로 나눠서 보여주는 기능이다. 목록 화면에 거의 항상 존재한다.

## 개념 - 핵심 용어

| 용어 | 의미 | 예시 |
|---|---|---|
| currentPage | 현재 페이지 번호 | 1, 2, 3 |
| pageSize | 한 페이지에 보여줄 행 수 | 10, 20 |
| totalCount | 전체 데이터 수 | 153 |
| totalPages | 전체 페이지 수 | ceil(153/10) = 16 |
| offset | 건너뛸 행 수 | (currentPage-1) x pageSize |

## SQL - LIMIT / OFFSET (PostgreSQL)

```sql
-- 1페이지: LIMIT 10 OFFSET 0  -> 1~10번째 행
-- 2페이지: LIMIT 10 OFFSET 10 -> 11~20번째 행
SELECT board_id, title, writer, reg_date
FROM board
ORDER BY reg_date DESC
LIMIT #{pageSize} OFFSET #{offset};

-- 전체 건수도 별도로 조회해야 한다
SELECT COUNT(*) FROM board;
```

## Mapper.xml

```xml
<!-- 목록 조회 -->
<select id="selectBoardList" parameterType="map" resultType="BoardVO">
    SELECT board_id, title, writer, reg_date
    FROM board
    WHERE 1=1
    <if test="keyword != null and keyword != ''">
        AND title LIKE CONCAT('%', #{keyword}, '%')
    </if>
    ORDER BY reg_date DESC
    LIMIT #{pageSize} OFFSET #{offset}
</select>

<!-- 전체 건수 -->
<select id="selectBoardCount" parameterType="map" resultType="int">
    SELECT COUNT(*)
    FROM board
    WHERE 1=1
    <if test="keyword != null and keyword != ''">
        AND title LIKE CONCAT('%', #{keyword}, '%')
    </if>
</select>
```

## ServiceImpl.java - offset 계산

```java
@Override
public Map<String, Object> getBoardListWithPaging(int currentPage, String keyword) {
    int pageSize  = 10;
    int offset    = (currentPage - 1) * pageSize; // 핵심 계산

    Map<String, Object> param = new HashMap<>();
    param.put("keyword",  keyword);
    param.put("pageSize", pageSize);
    param.put("offset",   offset);

    List<BoardVO> list       = boardDAO.selectBoardList(param);
    int           totalCount = boardDAO.selectBoardCount(param);
    int           totalPages = (int) Math.ceil((double) totalCount / pageSize);

    Map<String, Object> result = new HashMap<>();
    result.put("list",        list);
    result.put("totalCount",  totalCount);
    result.put("totalPages",  totalPages);
    result.put("currentPage", currentPage);
    return result;
}
```

## JS - 페이지 버튼 렌더링

```javascript
function renderPaging(currentPage, totalPages) {
    var html = "";

    if (currentPage > 1) {
        html += "<button onclick=\"Actions.goPage(" + (currentPage - 1) + ")\">이전</button>";
    }

    for (var i = 1; i <= totalPages; i++) {
        var active = (i === currentPage) ? "style=\"font-weight:bold\"" : "";
        html += "<button " + active + " onclick=\"Actions.goPage(" + i + ")\">" + i + "</button>";
    }

    if (currentPage < totalPages) {
        html += "<button onclick=\"Actions.goPage(" + (currentPage + 1) + ")\">다음</button>";
    }

    document.querySelector(".paging-area").innerHTML = html;
}
```

> 팁: offset = (currentPage - 1) x pageSize 이 공식 하나가 핵심이다. 1페이지는 0부터, 2페이지는 10부터 시작하는 원리다.
