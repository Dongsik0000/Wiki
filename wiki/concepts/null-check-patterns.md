---
title: null 체크 패턴
type: concept
tags: [java, javascript, error-handling]
created: 2026-07-16
updated: 2026-07-16
related: ["[[try-catch-finally]]", "[[truthy-falsy]]"]
---

# null 체크 패턴

**한 줄 요약:** Java에서 가장 흔한 오류인 NullPointerException을 방지하기 위해 null인지 먼저 확인하고 처리하는 방어 코드 작성법이다.

## 개념

NullPointerException은 값이 없는(null) 변수의 메서드나 속성에 접근할 때 발생한다. Java에서 가장 흔한 런타임 오류이므로 null 체크를 습관화해야 한다.

## Java null 체크 패턴

```java
// null 체크 없이 사용 -> NullPointerException 발생 가능
String title = vo.getTitle();
int length = title.length(); // title이 null이면 터짐

// null 체크 후 사용
String title = vo.getTitle();
if (title != null) {
    int length = title.length();
}

// null이면 기본값 사용
String title = (vo.getTitle() != null) ? vo.getTitle() : "제목 없음";

// null이거나 빈 문자열 모두 체크
if (title != null && !title.isEmpty()) {
    // 값이 있을 때만 처리
}

// 문자열 비교는 equals() 사용 (== 대신)
// ==는 참조(주소)를 비교, equals()는 내용을 비교
if ("admin".equals(userId)) { // userId가 null이어도 오류 없음
    // 상수를 앞에 두는 이유: userId가 null이어도 NPE 방지
}

// 리스트 null + 비어있는지 동시 체크
List<BoardVO> list = boardDAO.selectBoardList(param);
if (list != null && !list.isEmpty()) {
    // 데이터 있을 때만 처리
}
```

## MyBatis Mapper.xml에서 null 체크

```xml
<!-- Java에서 null이면 SQL에 포함 안 함 -->
<if test="keyword != null and keyword != ''">...</if>
<if test="userId != null">...</if>

<!-- resultType이 int일 때 COUNT가 null을 반환하는 경우 방지 -->
<select id="selectCount" resultType="int">
    SELECT COALESCE(COUNT(*), 0) FROM board  <!-- null이면 0 반환 -->
</select>
```

## JavaScript null/undefined 체크

```javascript
// JS에서는 null과 undefined 둘 다 주의
var data = response.data;

// 바로 사용하면 위험
console.log(data.title); // data가 null/undefined면 오류

// 존재 확인 후 사용
if (data && data.title) {
    console.log(data.title);
}

// 옵셔널 체이닝 (최신 JS)
console.log(data?.title); // data가 null이면 undefined 반환 (오류 없음)

// 기본값 지정
var title = data?.title || "제목 없음";
```

> 팁: null 체크 원칙은 외부에서 들어오는 값(파라미터, DB 결과, API 응답)은 항상 null일 수 있다고 가정하고 체크하는 것이다. 특히 문자열 비교는 equals(), 리스트는 "!= null && !isEmpty()" 패턴을 습관화하자.
