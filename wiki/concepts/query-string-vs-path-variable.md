---
title: Query String / Path Variable
type: concept
tags: [http, spring]
created: 2026-07-16
updated: 2026-07-16
related: ["[[rest-api]]"]
---

# Query String / Path Variable

## Query String

**정의:** URL에 데이터를 전달하기 위해 사용되는 방식. ?로 시작하며, key=value 형태로 데이터를 표현하며 여러 값을 전달할 때는 &로 구분한다.

**특징:**
- 데이터가 URL에 노출되기 때문에 보안에 민감한 정보는 적합하지 않다.
- 일반적으로 필터링, 검색, 페이징 같은 비필수 데이터를 전달할 때 사용한다.
- 브라우저에서 쉽게 읽고, 디버깅하기 쉽다.

```
형식: http://example.com/resource?key1=value1&key2=value2
// ?: Query String의 시작
// key=value: 데이터를 전달하는 쌍(파라미터)
// &: 여러 쌍의 데이터를 구분
```

## Path Variable

**정의:** URL 경로의 일부로 데이터를 전달하는 방식. 동적으로 변하는 값을 경로 안에 포함한다.

**특징:**
- 데이터를 경로의 일부로 전달하므로 리소스를 식별하는 데 적합.
- RESTful API에서 자주 사용되며, 고유 식별자(ID)나 특정 자원을 다룰 때 사용.
- Query String과 달리 URL 경로에 데이터를 포함하여 더 직관적.

```
형식: http://example.com/resource/{value}
// {value}: 전달할 데이터의 자리
```

## 쉽게 말하자면

- Query String: 주소 뒤에 추가 정보를 붙여 보내는 방식. URL 뒤에 ?를 붙이고, 데이터를 key=value 형태로 서버에 전달하며 여러 데이터를 보낼 때는 &로 구분한다. 검색, 필터링, 정렬처럼 서버에 부가적인 정보를 요청할 때 자주 사용한다.
- Path Variable: 주소 경로 자체에 데이터를 포함해 전달하는 방식. URL 경로 중 하나를 변수로 사용해 서버에 데이터를 전달하며, 특정 리소스나 고유 데이터를 조회할 때, URL로 직접 식별이 필요할 때 사용한다.
