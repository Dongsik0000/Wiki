---
title: Fiddler
type: concept
tags: [debugging, http, tools]
created: 2026-07-16
updated: 2026-07-16
related: ["[[devtools-usage]]"]
---

# Fiddler

**한 줄 요약:** 내 컴퓨터와 서버 사이를 오가는 HTTP/HTTPS 요청을 실시간으로 가로채 확인하고 수정할 수 있는 네트워크 디버깅 툴이다.

## 개념

Fiddler는 프록시 방식으로 동작한다. 브라우저가 서버에 요청을 보낼 때 중간에서 모든 요청/응답을 캡처한다. 브라우저 개발자도구(F12) Network 탭과 비슷하지만, 앱 전체 트래픽을 잡을 수 있고 요청을 직접 수정해서 재전송하는 것도 가능하다.

## 주로 사용하는 상황

- Ajax 요청이 실제로 어떤 데이터를 보내고 받는지 상세히 확인할 때
- 요청 헤더, CSRF 토큰, 쿠키 값 등을 직접 눈으로 확인할 때
- 서버 응답이 예상과 다를 때 원인 파악
- HTTPS 트래픽도 복호화해서 확인해야 할 때

## 기본 화면 구성

왼쪽 패널에는 캡처된 요청 목록(순번, 상태코드, 프로토콜, 호스트, URL)이 나오고, 오른쪽 패널에는 선택한 요청의 상세 내용(Headers, TextView, JSON)이 나온다.

## Ajax 요청 디버깅 흐름

```
1. Fiddler 실행 - 자동으로 프록시 설정됨
2. 브라우저에서 Ajax 요청 발생
3. 왼쪽 목록에서 해당 요청 클릭
4. 오른쪽 Inspectors 탭 선택
   - Request Headers : CSRF 토큰, Content-Type 확인
   - Request Body    : 실제로 보낸 JSON 데이터 확인
   - Response Body   : 서버가 돌려준 JSON 확인
5. 상태코드가 403이면 Headers에서 CSRF 토큰 누락 여부 확인
   상태코드가 500이면 Response Body에서 서버 에러 메시지 확인
```

## 요청 수정 후 재전송 (Replay)

왼쪽 목록에서 요청을 선택하고 우클릭 후 Replay 또는 Composer 탭에서 직접 수정해서 재전송할 수 있다. 파라미터 값이나 헤더를 바꿔서 테스트 가능하다.

## F12 Network 탭과의 차이

| 구분 | F12 Network | Fiddler |
|---|---|---|
| 범위 | 현재 브라우저 탭만 | PC 전체 트래픽 |
| HTTPS 복호화 | 자동 | 인증서 설치 필요 |
| 요청 수정/재전송 | 제한적 | 자유롭게 가능 |
| 주요 용도 | 빠른 확인 | 상세 분석, 테스트 |

> 팁: eGovFramework 개발 중 Ajax 요청이 403/500으로 실패할 때 Fiddler로 실제 전송된 헤더와 바디를 확인하면 원인을 빠르게 찾을 수 있다.
