---
title: 비즈니스 로직 (Business Logic)
type: concept
tags: [spring, architecture]
created: 2026-07-16
updated: 2026-07-16
related: ["[[controller-service-dao-vo]]"]
---

# 비즈니스 로직 (Business Logic)

**한 줄 요약:** 이 서비스만의 핵심 규칙과 판단을 처리하는 코드. DB 저장/조회 외에 "어떻게 처리할지"를 결정하는 부분이다.

## 개념

단순히 데이터를 저장/조회하는 것이 아니라, "어떤 조건과 규칙으로 처리할지"를 결정하는 코드다. 예를 들어 "재고보다 많이 주문할 수 없다", "이미 가입된 이메일은 중복 등록 불가" 같은 규칙이 비즈니스 로직이다.

## 주로 사용하는 상황

- 등록/수정 전에 유효성 검사가 필요할 때 (중복 체크, 필수값 확인 등)
- 여러 DAO를 조합해서 처리해야 할 때 (주문 → 재고 차감 → 이력 저장 등)
- 조건에 따라 다른 처리를 해야 할 때 (권한에 따라 다른 결과 반환 등)

## 사용법 : ServiceImpl에 작성

```java
// ❌ 잘못된 예 : 로직이 Controller에 들어간 경우
@RequestMapping("/user/join")
public String join(UserVO user) {
    // Controller에 판단 로직이 있으면 안 된다
    if (userDAO.selectByEmail(user.getEmail()) != null) {
        return "중복 이메일";
    }
    userDAO.insert(user);
    return "가입 완료";
}

// ✅ 올바른 예 : 로직을 ServiceImpl로 분리
// BoardController.java - 요청/응답 처리만
@RequestMapping("/user/join")
public String join(UserVO user, Model model) {
    String result = userService.join(user); // 판단은 Service에 위임
    model.addAttribute("msg", result);
    return "user/joinResult";
}

// UserServiceImpl.java - 실제 비즈니스 로직
@Override
public String join(UserVO user) {
    // ① 중복 이메일 체크
    if (userDAO.selectByEmail(user.getEmail()) != null) {
        return "이미 사용 중인 이메일입니다.";
    }
    // ② 비밀번호 암호화 (비즈니스 규칙)
    user.setPassword(encrypt(user.getPassword()));
    // ③ DB 저장
    userDAO.insert(user);
    return "가입이 완료되었습니다.";
}
```

## 어디에 넣어야 할까?

| 위치 | 역할 | 비즈니스 로직 여부 |
|---|---|---|
| Controller | 요청/응답 처리, 화면 이동 | ❌ 넣지 않는다 |
| ServiceImpl | 핵심 판단, 규칙 처리 | ✅ 여기에 작성 |
| DAO/Mapper | DB 접근만 | ❌ 넣지 않는다 |

> 💡 "이 코드가 DB 접근인가, 판단/규칙인가?"를 스스로 물어보면 어디에 넣을지 바로 결정된다.
