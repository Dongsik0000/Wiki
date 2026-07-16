---
title: console.log 디버깅 패턴
type: concept
tags: [javascript, debugging]
created: 2026-07-16
updated: 2026-07-16
related: ["[[devtools-usage]]"]
---

# console.log 디버깅 패턴

**한 줄 요약:** 브라우저 개발자도구 콘솔에서 데이터를 확인하는 방법이다. console.log 하나만 쓰는 것보다 다양한 메서드를 알면 디버깅 속도가 크게 빨라진다.

## 기본 사용법

```javascript
var user = { id: 1, name: "홍길동", dept: "개발팀" };
var list = [user, { id: 2, name: "김철수", dept: "기획팀" }];

console.log("일반 출력:", user);
console.log("이름:", user.name);

console.error("오류 메시지");   // 빨간색 오류 로그
console.warn("경고 메시지");    // 노란색 경고 로그
console.info("정보 메시지");    // 파란색 정보 로그
```

## 실무에서 유용한 메서드

```javascript
// console.table - 배열/객체를 표 형태로 보기 (목록 확인에 최고)
console.table(list);

// console.group - 관련 로그를 묶어서 보기
console.group("Ajax 요청 결과");
    console.log("요청 URL:", URLs.selectList);
    console.log("응답 데이터:", response);
    console.log("데이터 건수:", response.length);
console.groupEnd();

// console.time - 코드 실행 시간 측정
console.time("렌더링 시간");
Actions.renderTable(list);
console.timeEnd("렌더링 시간"); // 렌더링 시간: 2.3ms

// console.dir - DOM 요소의 상세 속성 확인
console.dir(document.querySelector("#userMgmt-table-body"));
```

## Ajax 디버깅 패턴 - 실무에서 자주 쓰는 위치

```javascript
const Actions = {
    refreshList: function() {
        console.log("refreshList 호출");
        console.log("검색어:", Elements.searchInput.value);

        $.ajax({
            url: URLs.selectList,
            type: "POST",
            data: { keyword: Elements.searchInput.value.trim() },
            success: function(response) {
                console.log("Ajax 성공");
                console.table(response);
                Actions.renderTable(response);
            },
            error: function(xhr) {
                console.error("Ajax 실패:", xhr.status, xhr.responseText);
            }
        });
    }
};
```

## 운영 배포 전 주의사항

운영 환경에 console.log를 그대로 두면 보안/성능 문제가 생길 수 있다. 배포 전에 디버그용 console.log는 제거하거나 주석 처리한다. 또는 환경 변수로 분기 처리할 수 있다.

```javascript
var isDev = true; // 운영 시 false
if (isDev) console.log("디버그:", data);
```

> 팁: Ajax 성공 후 가장 먼저 console.table(response)를 찍어보자. 데이터 구조를 눈으로 보면 response.boardId인지 response[0].board_id인지 바로 알 수 있어서 코드 작성 실수를 크게 줄일 수 있다.
