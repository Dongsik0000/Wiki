---
title: localStorage / sessionStorage
type: concept
tags: [javascript, browser]
created: 2026-07-16
updated: 2026-07-16
related: ["[[cookie-session]]"]
---

# localStorage / sessionStorage

**한 줄 요약:** 브라우저에 데이터를 저장하는 방법이다. 페이지를 새로고침하거나 이동해도 데이터가 유지되어, 검색 조건 유지나 임시 상태 저장에 활용한다.

## 개념 - 두 가지 차이

| 구분 | localStorage | sessionStorage |
|---|---|---|
| 유지 기간 | 브라우저를 닫아도 유지 | 탭/브라우저 닫으면 삭제 |
| 공유 범위 | 같은 도메인의 모든 탭 | 현재 탭에서만 |
| 용량 | 약 5MB | 약 5MB |
| 서버 전송 | X (클라이언트 전용) | X (클라이언트 전용) |
| 주요 용도 | 검색 조건 유지, 사용자 설정 | 임시 폼 데이터, 단계별 wizard |

## 기본 사용법 (API는 동일)

```javascript
// 저장
localStorage.setItem("keyword", "홍길동");
localStorage.setItem("pageSize", "20");
// 객체/배열은 JSON으로 변환해서 저장
localStorage.setItem("searchParam", JSON.stringify({ keyword: "홍길동", page: 2 }));

// 조회
var keyword  = localStorage.getItem("keyword");       // "홍길동"
var pageSize = localStorage.getItem("pageSize");      // "20" (항상 문자열)
var param    = JSON.parse(localStorage.getItem("searchParam")); // 객체로 복원

// 삭제
localStorage.removeItem("keyword");  // 특정 항목 삭제
localStorage.clear();                // 전체 삭제

// sessionStorage도 동일한 API
sessionStorage.setItem("tempData", "임시값");
sessionStorage.getItem("tempData");
```

## 실무 패턴 - 검색 조건 유지

```javascript
const Actions = {
    refreshList: function() {
        var keyword = Elements.searchInput.value.trim();

        // 검색 조건 저장 -> 페이지 이동 후 돌아와도 유지
        localStorage.setItem("board_keyword", keyword);

        $.ajax({
            url: URLs.selectList,
            data: { keyword: keyword },
            success: function(res) { Actions.renderTable(res); }
        });
    }
};

// 페이지 로드 시 이전 검색 조건 복원
var savedKeyword = localStorage.getItem("board_keyword");
if (savedKeyword) {
    Elements.searchInput.value = savedKeyword;
}

Actions.refreshList();
```

## 주의사항

```javascript
// 값은 항상 문자열로 저장된다
localStorage.setItem("count", 5);
var count = localStorage.getItem("count");
console.log(typeof count); // "string" (숫자 아님)
count = parseInt(count);   // 숫자로 쓰려면 변환 필요

// 민감한 정보는 저장 금지
// 비밀번호, 개인정보 등은 localStorage에 절대 저장하지 않는다
// 클라이언트 브라우저에서 누구나 볼 수 있음

// 존재하지 않는 키는 null 반환
var val = localStorage.getItem("없는키"); // null
if (val !== null) { /* 있을 때만 사용 */ }
```

> 팁: localStorage는 서버가 모른다. 서버 세션과 다르게 순수하게 브라우저에서만 관리되므로, 보안이 필요한 데이터(권한, 토큰 등)는 서버 세션을 사용해야 한다. 검색 조건이나 UI 상태처럼 잃어도 괜찮은 데이터에 적합하다.
