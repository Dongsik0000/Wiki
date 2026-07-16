---
title: "MyBatis XML 특수문자 처리 (이스케이프와 CDATA)"
type: concept
tags: [mybatis, sql]
created: 2026-07-16
updated: 2026-07-16
related: ["[[mybatis-dynamic-sql]]"]
---

# MyBatis XML 특수문자 처리 (이스케이프와 CDATA)

**한 줄 요약:** Mapper.xml은 XML 파일이므로 SQL 안에 부등호(<, >) 같은 특수문자를 그대로 쓰면 파싱 오류가 난다. 이스케이프를 쓰거나 CDATA 섹션으로 감싸면 해결된다.

## 개념 - 왜 이스케이프가 필요한가?

MyBatis Mapper는 .xml 파일이다. XML은 부등호 문자를 태그의 시작/끝으로 인식한다. 그래서 SQL 안에 >=, <=, <> 등을 쓰면 XML 파서가 태그로 오해해 파싱 오류가 발생한다.

## 방법 1 - 이스케이프 사용

| 실제 문자 | XML 이스케이프 | 설명 |
|---|---|---|
| > | &gt; | Greater Than |
| < | &lt; | Less Than |
| >= | &gt;= | |
| <= | &lt;= | |
| <> | &lt;&gt; | 다름 (!=) |
| & | &amp; | AND 등 |

```xml
<!-- 잘못된 예 - XML 파서가 < 를 태그로 오해 -->
<if test="age >= 18 and age <= 65">
    AND age >= #{minAge} AND age <= #{maxAge}
</if>

<!-- 이스케이프 적용 -->
<if test="age &gt;= 18 and age &lt;= 65">
    AND age &gt;= #{minAge} AND age &lt;= #{maxAge}
</if>
```

## 방법 2 - CDATA 섹션 (더 직관적)

CDATA는 Character Data의 약자로, `<![CDATA[ ... ]]>` 안의 내용은 XML 파서가 읽지 않고 문자 그대로 처리한다.

```xml
<!-- CDATA로 감싸면 부등호 등 특수문자를 그대로 쓸 수 있다 -->
<select id="selectUserList" parameterType="map" resultType="UserVO">
    SELECT user_id, user_nm, age
    FROM user
    <where>
        <if test="minAge != null">
            <![CDATA[
            AND age >= #{minAge} AND age <= #{maxAge}
            ]]>
        </if>
    </where>
</select>
<!-- 주의: CDATA 안에는 MyBatis 태그(if, where 등)를 쓸 수 없다 -->
<!-- 그래서 if 태그 바깥에 위치하고 SQL 부분만 CDATA로 감싸는 것이 올바르다 -->
```

## 이스케이프 vs CDATA 비교

| 구분 | 이스케이프 (&gt;) | CDATA |
|---|---|---|
| 가독성 | SQL이 복잡해 보일 수 있음 | 원시 SQL 그대로 작성 가능 |
| 사용범위 | 특수문자 하나씩 | 구문 전체를 감쌈 |
| MyBatis 태그 사용 | 가능 | 불가 (CDATA 안에서는 태그 인식 안 됨) |
| 주로 쓰는 상황 | 조건 한두 개일 때 | 부등호 비교연산자가 많을 때 |

## 실무에서 헷갈리는 케이스

```xml
<!-- test 속성에는 CDATA를 쓸 수 없다 -> 이스케이프만 사용 -->

<!-- 올바른 예 -->
<if test="cnt &gt;= 0">           <!-- test 속성은 이스케이프 사용 -->
    <![CDATA[
    AND cnt >= #{cnt}              -- SQL 본문은 CDATA 사용
    ]]>
</if>
```

> 팁: 원칙은 if test="..." 속성에는 이스케이프(&gt;), SQL 본문에는 CDATA. 부등호 비교연산자가 많다면 CDATA가 훨씬 읽기 쉽고, 간단한 조건이라면 &gt;=, &lt;= 이스케이프를 쓴다.
