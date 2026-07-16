---
title: HTTP 상태코드
type: concept
tags: [http, debugging]
created: 2026-07-16
updated: 2026-07-16
related: ["[[jquery-ajax-vs-fetch]]", "[[csrf]]", "[[devtools-usage]]"]
---

# HTTP 상태코드

**한 줄 요약:** 서버가 요청을 처리한 결과를 숫자로 알려주는 코드다. 디버깅할 때 상태코드만 봐도 원인을 빠르게 좁힐 수 있다.

## 상태코드 분류

| 범위 | 의미 |
|---|---|
| 2xx | 성공 |
| 3xx | 리다이렉트 (다른 주소로 이동) |
| 4xx | 클라이언트 오류 (요청이 잘못됨) |
| 5xx | 서버 오류 (서버가 잘못 처리함) |

## 자주 마주치는 상태코드

| 코드 | 이름 | 의미 | eGov에서 주로 발생하는 원인 |
|---|---|---|---|
| 200 | OK | 성공 | 정상 처리 |
| 302 | Found | 리다이렉트 | redirect: 반환 시 |
| 400 | Bad Request | 잘못된 요청 | 요청 데이터 형식 불일치, JSON 파싱 오류 |
| 401 | Unauthorized | 인증 필요 | 로그인 안 한 상태로 접근 |
| 403 | Forbidden | 접근 금지 | CSRF 토큰 누락, 권한 없음 |
| 404 | Not Found | 찾을 수 없음 | URL 오타, Controller 매핑 없음 |
| 405 | Method Not Allowed | 메서드 불일치 | GET으로 요청했는데 POST만 처리하는 경우 |
| 500 | Internal Server Error | 서버 오류 | Java 예외 발생 (NullPointerException 등) |

## 상태코드별 디버깅 체크리스트

```
400 Bad Request
  -> Ajax contentType과 Controller @RequestBody 불일치 확인
  -> JSON.stringify() 빠진 것 없는지 확인
  -> @RequestParam 이름이 실제 파라미터명과 일치하는지 확인

403 Forbidden
  -> CSRF 토큰 헤더 누락 확인
  -> 권한 설정 확인 (Spring Security)

404 Not Found
  -> @RequestMapping URL 오타 확인
  -> contextPath 누락 확인
  -> GET/POST 메서드 방향 확인

500 Internal Server Error
  -> Eclipse 콘솔에서 실제 예외 메시지 확인
  -> NullPointerException: null 체크 누락
  -> SQL 오류: Mapper.xml 쿼리 확인
  -> 타입 불일치: VO 필드 타입과 DB 컬럼 타입 확인
```

## Ajax에서 상태코드 확인하는 방법

```javascript
$.ajax({
    url: URLs.selectList,
    type: "POST",
    success: function(data) {
        // 200 OK만 여기로 옴
    },
    error: function(xhr, status, err) {
        console.log("상태코드:", xhr.status);
        console.log("응답 내용:", xhr.responseText);
        alert("오류 " + xhr.status + ": " + err);
    }
});
```

> 팁: 디버깅 순서는 상태코드 확인 -> F12 Network 탭에서 요청/응답 확인 -> Eclipse 콘솔에서 서버 오류 메시지 확인. 500이면 반드시 Eclipse 콘솔을 봐야 실제 원인을 알 수 있다.
