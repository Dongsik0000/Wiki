---
title: async & await
type: concept
tags: [javascript]
created: 2026-07-16
updated: 2026-07-16
related: ["[[promise]]"]
---

# async & await

## async

함수 앞에 붙여서 해당 함수가 비동기를 반환함을 선언합니다. 이 함수는 항상 Promise 객체를 반환하며, 반환된 값은 자동으로 Promise.resolve()에 의해 감싸집니다. 용도는 비동기 작업을 간편하게 다룰 수 있도록 하는 것입니다.

```javascript
async function fetchData() {
    return "데이터 수신 완료";
}

// Promise 객체가 반환됩니다.
fetchData().then(data => console.log(data)); // "데이터 수신 완료"
```

## await

async 함수 안에서만 사용할 수 있으며, Promise가 처리될 때까지 함수의 실행을 일시 중지합니다. Promise가 해결되면 그 결과를 반환합니다. 용도는 비동기 작업의 결과를 더 간단하게 처리할 수 있게 해주는 것입니다.

```javascript
async function fetchData() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("데이터 수신 완료");
        }, 2000);
    });
}

async function main() {
    const data = await fetchData(); // Promise가 해결될 때까지 대기
    console.log(data); // "데이터 수신 완료"
}

main();
```

## 비유를 통한 설명

**1. async: 주문하기**

async는 피자 가게에 전화를 걸어 주문을 하는 것과 같습니다. 전화를 걸면 피자 가게는 "주문을 받았습니다. 피자는 곧 준비될 것입니다."라고 답합니다. 실제로 피자가 준비되기까지는 시간이 걸리겠지만, 당신은 이 시간 동안 다른 일을 할 수 있습니다. 즉, async 함수는 비동기 작업을 시작하겠다는 약속을 하는 것입니다.

**2. await: 피자 기다리기**

피자가 준비될 때까지 기다리는 상황이 await에 해당합니다. 피자 가게에 전화를 걸고 "피자 배달이 언제 오나요?"라고 묻는 것이 await입니다. 가게는 "2시간 후에 배달됩니다."라고 답변합니다. 그때 당신은 2시간 동안 피자가 오기를 기다립니다. 이때 특별한 점은, 2시간 동안 아무것도 못 하는 게 아니라 다른 일을 할 수 있다는 점입니다.
