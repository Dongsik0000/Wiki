---
title: Camel Case (카멜 케이스)
type: concept
tags: [naming-convention]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# Camel Case (카멜 케이스)

**한 줄 요약:** 여러 단어를 붙여 쓸 때 첫 단어는 소문자, 이후 단어의 첫 글자는 대문자로 쓰는 명명 규칙이다.

## 개념

프로그래밍에서는 변수명, 메서드명, 클래스명 등에 띄어쓰기를 쓸 수 없다. 그래서 단어를 구분하기 위해 대소문자를 이용한 명명 규칙이 생겼고, 가장 많이 쓰이는 방식이 Camel Case다.

## 종류

| 종류 | 형태 | 예시 | 주로 쓰는 곳 |
|---|---|---|---|
| lowerCamelCase | 첫 글자 소문자 | getUserList, boardId | Java 변수/메서드, JavaScript 변수/함수 |
| UpperCamelCase (PascalCase) | 첫 글자 대문자 | BoardController, UserVO | Java 클래스/인터페이스 이름 |

## 다른 명명 규칙과 비교

| 규칙 | 형태 | 예시 | 주로 쓰는 곳 |
|---|---|---|---|
| camelCase | 단어 경계마다 대문자 | getUserList | Java/JS 변수, 메서드 |
| snake_case | 단어 사이 언더스코어 | get_user_list | Python, DB 컬럼명, MyBatis 파라미터 |
| kebab-case | 단어 사이 하이픈 | get-user-list | HTML id/class, CSS, URL |
| UPPER_SNAKE_CASE | 대문자 + 언더스코어 | MAX_SIZE, API_KEY | Java 상수 (static final) |

## eGovFramework에서의 실제 사용 예시

```java
// Java - lowerCamelCase: 변수, 메서드
private BoardService boardService;
public List<BoardVO> getBoardList() { }
int boardId = 1;

// Java - UpperCamelCase(PascalCase): 클래스, 인터페이스
public class BoardController { }
public interface BoardService { }
public class BoardVO { }
```

```javascript
// JavaScript - lowerCamelCase: 변수, 함수
var contextPath = "...";
var csrfToken = "...";

function refreshList() { }
function renderTable() { }
```

```sql
-- DB 컬럼명 - snake_case
SELECT board_id, user_nm, reg_date FROM board;
```

MyBatis Mapper는 resultType의 VO 필드를 camelCase로 자동 매핑한다. DB의 board_id는 VO의 boardId로 자동 변환된다 (MyBatis 기본 설정).

## 자주 하는 실수

```java
// 잘못된 예 - 규칙 혼용
String UserName = "홍길동";   // 변수인데 대문자로 시작
String user_name = "홍길동";  // Java 변수인데 snake_case 사용

// 올바른 예
String userName = "홍길동";   // lowerCamelCase

// 잘못된 예 - 클래스명
class boardController { }      // 클래스인데 소문자 시작

// 올바른 예
class BoardController { }      // UpperCamelCase
```

> 팁: Java 변수/메서드는 lowerCamelCase, Java 클래스는 UpperCamelCase, DB 컬럼은 snake_case, HTML id/class는 kebab-case. MyBatis는 board_id(snake_case)를 boardId(camelCase)로 자동 변환해주므로 VO 필드명은 camelCase로 작성한다.
