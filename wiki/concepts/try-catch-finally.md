---
title: try-catch-finally / 예외 처리
type: concept
tags: [java, error-handling]
created: 2026-07-16
updated: 2026-07-16
related: ["[[null-check-patterns]]"]
---

# try-catch-finally / 예외 처리

**한 줄 요약:** 코드 실행 중 오류(예외)가 발생했을 때 프로그램이 죽지 않도록 잡아서 처리하는 문법이다. DB 오류, null 참조 등 예상치 못한 상황에 대비한다.

## 개념 - 구조

```java
try {
    // 실행할 코드 (예외가 발생할 수 있는 부분)
} catch (예외타입 변수명) {
    // 예외가 발생했을 때 실행할 코드
} finally {
    // 예외 발생 여부와 관계없이 항상 실행 (생략 가능)
}
```

## Java - 기본 사용법

```java
// 예시 1: 기본 try-catch
try {
    int result = 10 / 0;  // ArithmeticException 발생
} catch (ArithmeticException e) {
    System.out.println("0으로 나눌 수 없습니다: " + e.getMessage());
}

// 예시 2: 여러 예외 처리
try {
    String str = null;
    str.length();  // NullPointerException 발생
} catch (NullPointerException e) {
    System.out.println("null 참조 오류");
} catch (Exception e) {
    // 위에서 못 잡은 모든 예외
    System.out.println("알 수 없는 오류: " + e.getMessage());
} finally {
    System.out.println("항상 실행");  // DB 커넥션 반환 등에 사용
}
```

## eGovFramework Service에서의 실무 패턴

```java
@Override
public int insertBoard(BoardVO vo) {
    try {
        return boardDAO.insertBoard(vo);
    } catch (Exception e) {
        // 로그 기록 (eGov는 log4j 사용)
        log.error("게시글 등록 실패", e);
        throw new RuntimeException("게시글 등록 중 오류가 발생했습니다.", e);
        // Controller에서 이 예외를 잡거나, 공통 에러 페이지로 이동
    }
}
```

## Ajax에서의 예외 처리 흐름

```javascript
// JS에서는 try-catch보다 $.ajax의 error 콜백으로 처리
$.ajax({
    url: URLs.insertInfo,
    type: "POST",
    success: function(result) {
        if (result.status === "success") {
            alert("등록 완료");
        } else {
            alert("처리 실패: " + result.message);
        }
    },
    error: function(xhr) {
        // 서버에서 예외가 터지면 500으로 옴
        alert("서버 오류가 발생했습니다. (" + xhr.status + ")");
        console.error(xhr.responseText);
    }
});
```

## 자주 발생하는 예외 종류

| 예외 | 주로 발생하는 상황 |
|---|---|
| NullPointerException | null인 변수의 메서드 호출 |
| ArrayIndexOutOfBoundsException | 배열 범위 초과 |
| ClassCastException | 잘못된 타입 변환 |
| NumberFormatException | 숫자가 아닌 문자열을 int로 변환 |
| SQLException | DB 쿼리 실패 |
| MyBatisSystemException | Mapper 오류 |

> 팁: catch(Exception e)는 마지막 안전망이다. 구체적인 예외를 먼저 잡고, 나머지를 Exception으로 잡는 것이 좋다. e.getMessage()와 log.error()로 원인을 남겨두면 디버깅이 훨씬 쉬워진다.
