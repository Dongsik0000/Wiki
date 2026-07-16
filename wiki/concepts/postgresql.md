---
title: PostgreSQL
type: concept
tags: [db, sql]
created: 2026-07-16
updated: 2026-07-16
related: ["[[rdbms-vs-nosql]]"]
---

# PostgreSQL

**한 줄 요약:** MySQL과 비슷하지만 더 엄격하고 기능이 많은 오픈소스 RDBMS. eGovFramework 프로젝트에서 자주 사용된다.

## 개념

SQL 문법은 MySQL과 대부분 비슷하지만, 컬럼명 대소문자, 시퀀스 방식, 문자열 비교 등에서 차이가 있다.

## MySQL과 주요 차이점

| 항목 | MySQL | PostgreSQL |
|---|---|---|
| 자동 증가 ID | AUTO_INCREMENT | SERIAL 또는 SEQUENCE |
| 컬럼명 대소문자 | 구분 없음 | 기본 소문자 적용 |
| 대문자 컬럼명 쓸 때 | 안 써도 됨 | 쌍따옴표 필수 |
| 문자열 연결 | CONCAT() 또는 이중파이프 | 이중파이프 연산자 권장 |
| 페이징 | LIMIT, OFFSET | LIMIT, OFFSET (동일) |

## 사용법 1 - eGovFramework datasource.xml 연결 설정

```xml
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="org.postgresql.Driver"/>
    <property name="url" value="jdbc:postgresql://localhost:5432/데이터베이스명"/>
    <property name="username" value="postgres"/>
    <property name="password" value="비밀번호"/>
    <property name="defaultAutoCommit" value="false"/>
</bean>
```

## 사용법 2 - Mapper.xml SQL 작성 예시

```xml
<select id="selectBoardList" parameterType="map" resultType="BoardVO">
    SELECT board_id, title, writer, reg_date
    FROM board
    ORDER BY reg_date DESC
    LIMIT #{pageSize} OFFSET #{offset}
</select>

<insert id="insertBoard" parameterType="BoardVO">
    INSERT INTO board (board_id, title, content, writer, reg_date)
    VALUES (nextval(board_seq), #{title}, #{content}, #{writer}, NOW())
</insert>
```

테이블 생성 예시:

```sql
CREATE TABLE board (
    board_id  SERIAL       PRIMARY KEY,
    title     VARCHAR(200) NOT NULL,
    content   TEXT,
    writer    VARCHAR(50),
    reg_date  TIMESTAMP    DEFAULT NOW()
);
```

## 사용법 3 - 자주 쓰는 PostgreSQL 함수

```sql
-- 문자열 연결
SELECT concat_ws(" ", "Hello", "World");

-- 현재 시간
NOW()
CURRENT_DATE

-- 대소문자 구분 없이 검색 (LIKE의 PostgreSQL 버전)
WHERE title ILIKE CONCAT("%", #{keyword}, "%")

-- 통계 함수
COUNT(*), SUM(amount), AVG(score), MAX(price), MIN(price)
```

> 팁: pgAdmin이나 DBeaver에서 SQL 먼저 실행해보고, 에러가 없으면 Mapper.xml로 옮겨 넣는 방식을 권장한다.
