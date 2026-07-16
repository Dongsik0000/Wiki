---
title: "MyBatis 동적 SQL (if, foreach, choose)"
type: concept
tags: [mybatis, sql]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mybatis-resultmap]]", "[[mybatis-xml-escape]]", "[[pagination]]"]
---

# MyBatis 동적 SQL (if, foreach, choose)

**한 줄 요약:** 조건에 따라 SQL이 달라져야 할 때 Mapper.xml에서 태그로 분기 처리한다. 검색 조건이 있을 때/없을 때를 하나의 SQL로 처리할 수 있다.

## 개념

일반 SQL은 고정된 쿼리만 실행된다. 하지만 실무에서는 "검색어가 있으면 WHERE 조건 추가, 없으면 전체 조회" 같은 상황이 매일 나온다. MyBatis 동적 SQL 태그를 쓰면 하나의 Mapper로 이 모든 경우를 처리할 수 있다.

## if - 조건이 참일 때만 SQL 추가

```xml
<select id="selectBoardList" parameterType="map" resultType="BoardVO">
    SELECT board_id, title, writer, reg_date
    FROM board
    WHERE 1=1
    <if test="keyword != null and keyword != ''">
        AND title LIKE CONCAT('%', #{keyword}, '%')
    </if>
    <if test="writer != null and writer != ''">
        AND writer = #{writer}
    </if>
    ORDER BY reg_date DESC
</select>
<!-- WHERE 1=1 을 쓰는 이유: 조건이 하나도 없어도 SQL이 깨지지 않도록 -->
```

## foreach - 배열/리스트를 반복해서 IN 절 생성

```xml
<!-- Java에서 List<Integer> idList = [1, 2, 3] 을 넘겼을 때 -->
<select id="selectBoardByIds" parameterType="map" resultType="BoardVO">
    SELECT board_id, title
    FROM board
    WHERE board_id IN
    <foreach collection="idList" item="id" open="(" separator="," close=")">
        #{id}
    </foreach>
</select>
<!-- collection: map에서 꺼낼 키명(List 변수명), item: 반복 시 각 요소 변수명 -->
<!-- open: 시작 문자, separator: 구분자, close: 종료 문자 -->
```

## choose / when / otherwise - Java의 if-else if-else

```xml
<select id="selectBoardList" parameterType="map" resultType="BoardVO">
    SELECT board_id, title, writer, reg_date
    FROM board
    ORDER BY
    <choose>
        <when test="sortType == 'title'">title ASC</when>
        <when test="sortType == 'writer'">writer ASC</when>
        <otherwise>reg_date DESC</otherwise>
    </choose>
</select>
```

## where - WHERE 절 자동 처리 (WHERE 1=1 대체)

```xml
<select id="selectBoardList" parameterType="map" resultType="BoardVO">
    SELECT board_id, title, writer
    FROM board
    <where>
        <if test="keyword != null and keyword != ''">AND title LIKE CONCAT('%', #{keyword}, '%')</if>
        <if test="writer != null and writer != ''">AND writer = #{writer}</if>
    </where>
</select>
<!-- where 태그가 자동으로 첫 번째 AND를 제거하고 WHERE를 붙여줌 -->
```

## set - UPDATE 시 수정할 컬럼만 동적으로

```xml
<update id="updateBoard" parameterType="BoardVO">
    UPDATE board
    <set>
        <if test="title != null">title = #{title},</if>
        <if test="content != null">content = #{content},</if>
        <if test="writer != null">writer = #{writer},</if>
    </set>
    WHERE board_id = #{boardId}
</update>
<!-- set 태그가 마지막 쉼표(,)를 자동으로 제거해줌 -->
```

> 팁: WHERE 1=1 vs where 태그 - 둘 다 동적 조건을 처리하지만, where 태그가 더 명시적이다. eGovFramework에서는 WHERE 1=1 방식도 많이 쓰이므로 둘 다 알아두자.
