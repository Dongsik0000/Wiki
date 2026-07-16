---
title: Truthy & Falsy
type: concept
tags: [javascript]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# Truthy & Falsy

**한 줄 요약:** JavaScript에서 true/false가 아닌 값도 조건문에서 참/거짓으로 취급된다. 어떤 값이 참으로, 어떤 값이 거짓으로 평가되는지 알아야 if 문 버그를 줄일 수 있다.

## 개념

JavaScript는 if(값)에 boolean이 아닌 값을 넣어도 자동으로 true/false로 변환해서 판단한다. Falsy는 false로 평가되는 값(if 블록이 실행되지 않음), Truthy는 falsy가 아닌 모든 값(if 블록이 실행됨)이다.

## Falsy 값 (딱 6가지만 외우면 된다)

| 값 | 설명 |
|---|---|
| false | boolean false |
| 0 | 숫자 0 (음수 -0 포함) |
| "" | 빈 문자열 |
| null | 값 없음 |
| undefined | 선언만 되고 값이 없음 |
| NaN | 숫자가 아님 (Not a Number) |

## Truthy 값 (falsy가 아닌 모든 것)

| 값 | 설명 |
|---|---|
| "hello" | 비어있지 않은 문자열 |
| 1, -1, 3.14 | 0이 아닌 숫자 |
| [] | 빈 배열도 truthy |
| {} | 빈 객체도 truthy |
| "0" | 문자열 "0"은 truthy (숫자 0과 다름) |
| function(){} | 함수 |

## 코드 예시

```javascript
// Truthy 예시
if ("hello") { console.log("실행됨"); }
if (1)       { console.log("실행됨"); }
if ([])      { console.log("실행됨"); }    // 빈 배열도 truthy
if ({})      { console.log("실행됨"); }    // 빈 객체도 truthy

// 실제 사용 예시 - input 값 검증
var title = $("#title").val();

if (!title) {
    alert("제목을 입력해주세요.");
    return;
}

// 배열이 비어있는지 확인할 때 주의
var list = [];
if (list) {           // 빈 배열도 truthy이므로 이 블록은 실행됨
    console.log("truthy");
}
if (list.length) {    // 배열 비어있는지 확인은 length로 해야 한다
    console.log("데이터 있음");
}
```

## 자주 쓰는 패턴

```javascript
// 값이 있는지 없는지 간단히 확인
if (userId) { }
if (!userId) { alert("필수"); }

// 기본값 지정
var name = inputName || "익명";

// 짧은 조건 실행
var isAdmin = true;
isAdmin && doAdminTask();
```

## 헷갈리는 케이스 정리

| 값 | Truthy / Falsy | 주의 이유 |
|---|---|---|
| "0" | Truthy | 문자열이라 0이어도 truthy |
| 0 | Falsy | 숫자 0은 falsy |
| [] | Truthy | 빈 배열도 truthy |
| {} | Truthy | 빈 객체도 truthy |

> 팁: if(값)으로 존재 여부를 체크할 때는 문자열/숫자/null/undefined에 잘 동작하지만, 배열이나 객체가 비어있는지 여부는 반드시 .length 또는 Object.keys()로 따로 확인해야 한다.
