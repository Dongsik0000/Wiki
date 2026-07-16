---
title: "MyBatis resultMap"
type: concept
tags: [mybatis, sql]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mybatis-dynamic-sql]]", "[[camel-case]]"]
---

# MyBatis resultMap

**한 줄 요약:** DB 컬럼명과 VO 필드명이 달라서 자동 매핑이 안 될 때 수동으로 연결하는 설정이다. 복잡한 JOIN 결과나 별칭이 다른 경우에 사용한다.

## 언제 필요한가?

MyBatis는 기본적으로 board_id -> boardId (snake_case -> camelCase) 자동 변환을 지원한다. 하지만 아래 경우에는 resultMap이 필요하다.

- 자동 변환 설정이 꺼져 있는 환경
- 컬럼 별칭(AS)과 VO 필드명이 완전히 다른 경우
- JOIN 결과를 하나의 VO에 담아야 할 때

## 기본 사용법

```xml
<!-- VO의 필드명과 DB 컬럼명을 수동 매핑 -->
<resultMap id="boardResultMap" type="com.example.vo.BoardVO">
    <id     property="boardId"  column="board_id"/>   <!-- PK는 id 태그 -->
    <result property="title"    column="title"/>
    <result property="writer"   column="writer"/>
    <result property="regDate"  column="reg_date"/>   <!-- snake_case -> camelCase -->
    <result property="viewCnt"  column="view_count"/>  <!-- 완전히 다른 이름 -->
</resultMap>

<!-- resultType 대신 resultMap 사용 -->
<select id="selectBoardList" resultMap="boardResultMap">
    SELECT board_id, title, writer, reg_date, view_count
    FROM board
    ORDER BY reg_date DESC
</select>
```

## resultType vs resultMap 비교

| 구분 | resultType | resultMap |
|---|---|---|
| 사용 시점 | 컬럼명과 VO 필드명이 대응될 때 | 이름이 달라서 수동 매핑이 필요할 때 |
| 설정 | 간단 (타입만 지정) | 별도 resultMap 블록 작성 |
| 자동 변환 | camelCase 설정 있으면 자동 | 명시적으로 지정 |

## camelCase 자동 변환 설정 확인

```xml
<!-- mybatis-config.xml 또는 SqlSessionFactory 설정 -->
<!-- 이 설정이 있으면 board_id -> boardId 자동 변환 -->
<settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
<!-- eGovFramework에서는 이 설정이 대부분 되어 있어 resultMap 없이도 잘 동작한다 -->
```

> 팁: eGovFramework에서 mapUnderscoreToCamelCase=true 설정이 되어 있으면 대부분 resultType만으로 충분하다. resultMap은 JOIN이 복잡하거나 명시적 매핑이 꼭 필요한 상황에서 쓴다.
