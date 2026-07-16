---
title: 퍼블리셔에게 HTML 파일을 받았을 때
type: concept
tags: [frontend, jsp, workflow]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# 퍼블리셔에게 HTML 파일을 받았을 때

**한 줄 요약:** 퍼블리셔로부터 받은 HTML은 **Demo 파일**이다. 풀스택 개발자로서 실제 개발에 사용하려면 JS/JSP 제어를 위한 **id를 추가**하고 구조를 파악한 후 개발에 적용한다.

## 퍼블리셔의 역할 vs 개발자의 역할

| 구분 | 퍼블리셔 | 개발자 |
|---|---|---|
| 주요 작업 | HTML/CSS 디자인, 레이아웃 | 서버 데이터 연동, 기능 구현 |
| 식별자 | 보통 class만 사용 | id를 추가해서 JS/JSP에서 제어 |
| 데이터 | 정적 텍스트 (Mock 데이터) | DB에서 가져온 실제 데이터로 교체 |
| 파일 | 독립 HTML 파일 | JSP로 변환하여 소스 통합 |

## 파일 수령 후 작업 순서

```
1. HTML 파일 열기
   → 전체 구조 파악 (헤더, 사이드바, 콘텐츠, 푸터 등)
   → 페이지가 몇 단위로 나뉘는지 확인 (목록, 등록, 수정 등)

2. class명 파악
   → JS에서 querySelector로 제어할 요소들 확인
   → 코드 작성 방식에 맞게 시맨틱(semantic) class명 확인

3. id 추가 작업 — JS/JSP에서 제어할 모든 요소에 id 부여
   → 테이블 tbody : id="XXX-table-body"
   → 입력 필드   : id="XXX-search-input"
   → 버튼류      : id="XXX-search-button"
   → 모달 요소   : id="XXX-modal"

4. JSP로 변환
   → .html → .jsp 로 확장자 변경
   → 상단에 JSP 지시자 추가 (<%@ page ... %>, taglib 등)
   → 정적 Mock 데이터를 ${} EL 식으로 교체

5. JS 코드 작성
   → 식별자를 기반으로 Elements 객체 구성
   → Ajax 연동 및 이벤트 바인딩 개발
```

## id 추가 작업 예시

```html
<!-- 퍼블리셔에게 받은 HTML (class만 있는 상태) -->
<input type="text" class="search-input" placeholder="검색어 입력">
<button class="search-button">검색</button>
<tbody class="user-table-body"></tbody>
<div class="user-modal">...</div>

<!-- 개발자가 id를 추가한 상태 -->
<input type="text" id="userMgmt-search-input" class="search-input" placeholder="검색어 입력">
<button id="userMgmt-search-button" class="search-button">검색</button>
<tbody id="userMgmt-table-body" class="user-table-body"></tbody>
<div id="userMgmt-modal" class="user-modal">...</div>
```

## id 명명 권장 규칙

```
[페이지명]-[요소역할]

ex) userMgmt-table-body     → 회원관리 테이블 tbody
    userMgmt-search-input   → 회원관리 검색어 input
    userMgmt-search-button  → 회원관리 검색 버튼
    userMgmt-modal          → 회원관리 모달
    userMgmt-save-button    → 회원관리 저장 버튼

이유: 페이지가 여러 개일 때 id가 겹치지 않도록 페이지명을 접두사로 쓴다.
```

## Mock 데이터 → EL/JSTL로 교체 예시

```html
<!-- 퍼블리셔 HTML: 정적 Mock 데이터 -->
<tr>
    <td>1</td>
    <td>홍길동</td>
    <td>010-1234-5678</td>
    <td>승인완료</td>
</tr>

<!-- 개발자 JSP: DB 데이터로 교체 -->
<tbody id="userMgmt-table-body">
    <%-- Ajax로 동적 렌더링 예정 → 빈 tbody만 남김 --%>
</tbody>
```

## HTML → JSP 변환 체크리스트

```
☐ .html → .jsp 파일명 변경
☐ JSP 지시자 추가 (<%@ page contentType="text/html; charset=UTF-8" %>)
☐ JSTL taglib 선언 추가 (<%@ taglib prefix="c" uri="..." %>)
☐ contextPath 변수 선언 (var ctx = "${pageContext.request.contextPath}")
☐ CSRF 토큰 변수 선언
☐ 제어할 모든 HTML 요소에 id 부여 완료
☐ Mock 데이터 제거 (tbody 빈 상태로 만들기)
☐ 정적 헤더/푸터 등 공통 레이아웃은 include 태그로 교체
☐ JS 코드 작성 (URLs, Elements, Actions, 이벤트 바인딩)
```

> 💡 퍼블리셔 HTML은 디자인 스케치다. 실제 개발에서는 class는 CSS 스타일링 목적으로만 사용하고, JS에서 제어할 모든 요소는 id로 식별하여 관리한다. 이 구조를 지켜야 코드의 일관성이 유지된다.
