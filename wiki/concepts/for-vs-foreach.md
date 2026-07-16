---
title: for vs forEach
type: concept
tags: [javascript]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# for vs forEach

**한 줄 요약:** 둘 다 반복 작업을 하는 문법이다. for는 세밀한 제어가 필요할 때, forEach는 배열을 간결하게 순회할 때 사용한다.

## 개념

- for: 가장 기본적인 반복문. 시작값, 조건, 증감을 직접 지정한다. 배열뿐 아니라 어떤 반복에도 사용할 수 있다.
- forEach: 배열 전용 메서드. 배열의 각 요소를 순서대로 콜백 함수에 넘긴다.

## 주로 사용하는 상황

| 상황 | 권장 |
|---|---|
| 배열을 단순히 순회하며 처리 | forEach |
| 중간에 break로 멈춰야 할 때 | for (forEach는 break 불가) |
| 인덱스 번호가 필요할 때 | for 또는 forEach의 두 번째 인자 |
| 배열이 아닌 숫자 카운트 반복 | for |

## for 문 사용법

```javascript
for (var i = 0; i < 5; i++) {
    console.log(i); // 0, 1, 2, 3, 4
}

var fruits = ["사과", "바나나", "딸기"];
for (var i = 0; i < fruits.length; i++) {
    console.log(i, fruits[i]);
}

// break로 중간에 멈추기 (forEach는 불가)
for (var i = 0; i < fruits.length; i++) {
    if (fruits[i] === "바나나") break;
    console.log(fruits[i]); // 사과만 출력됨
}
```

## forEach 사용법

```javascript
var fruits = ["사과", "바나나", "딸기"];

fruits.forEach(function(item) {
    console.log(item);
});

fruits.forEach(function(item, index) {
    console.log(index, item);
});

// 실무 패턴: Ajax 응답 배열로 테이블 행 만들기
response.forEach(function(user) {
    var row = document.createElement("tr");
    row.innerHTML = "<td>" + user.user_id + "</td><td>" + user.user_nm + "</td>";
    tbody.appendChild(row);
});

// jQuery $.each - jQuery 환경에서 forEach와 동일한 역할
$.each(response, function(index, user) {
    console.log(index, user.user_nm);
});
```

## for vs forEach 핵심 차이

| 구분 | for | forEach |
|---|---|---|
| 사용 대상 | 숫자 범위, 배열 등 모두 | 배열 전용 |
| break | 가능 | 불가 |
| continue | 가능 | 불가 (return으로 대체) |
| 코드 길이 | 상대적으로 길다 | 상대적으로 짧다 |

> 팁: eGovFramework JSP 환경에서는 Ajax 응답 배열을 테이블에 렌더링할 때 forEach 또는 $.each를 가장 많이 사용한다. 중간에 멈춰야 하거나 인덱스를 직접 조작해야 할 때는 for 문을 사용한다.
