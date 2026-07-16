---
title: "DTO (Data Transfer Object)"
type: concept
tags: [architecture, spring]
created: 2026-07-16
updated: 2026-07-16
related: ["[[controller-service-dao-vo]]"]
---

# DTO (Data Transfer Object)

데이터 전송 객체로, 계층 간 데이터 전송을 위해 사용하는 객체. 주로 데이터베이스와 애플리케이션 간, 또는 클라이언트와 서버 간의 데이터 교환을 간소화하고 구조화하는 용도로 사용한다.

## DTO의 주요 개념

1. **목적**: 데이터를 전송하기 위한 간결한 구조체 역할. 비즈니스 로직이나 데이터베이스와 관련된 세부 사항을 숨기고, 필요한 데이터만 담아 전송합니다.
2. **계층 간 데이터 전송**: 주로 서비스 계층과 프레젠테이션 계층(UI, API) 간의 데이터 전송에 사용됩니다. 데이터의 형식을 일관되게 유지할 수 있습니다.
3. **비즈니스 로직과 분리**: DTO는 단순히 데이터를 담는 그릇 역할만 하며, 비즈니스 로직을 포함하지 않습니다. 코드의 책임을 분리하고 유지보수를 용이하게 만듭니다.
4. **데이터 캡슐화**: 데이터의 구조를 정의하고, 불필요한 세부 정보를 감추어 클라이언트가 요청하는 데이터만 포함하도록 설계할 수 있습니다.

## DTO 사용 예시

```typescript
class UserDTO {
    id: number;
    name: string;
    email: string;

    constructor(id: number, name: string, email: string) {
        this.id = id;
        this.name = name;
        this.email = email;
    }
}

const userDto = new UserDTO(1, "John Doe", "john@example.com");
const json = JSON.stringify(userDto);
```

## DTO의 장점

1. **유연성**: 다양한 클라이언트가 각기 다른 데이터 요구 사항을 가질 때, DTO를 사용하면 특정 클라이언트에 맞게 데이터를 조정할 수 있습니다.
2. **보안**: 직접 데이터베이스 모델을 노출하는 대신 DTO를 사용하면, 필요한 데이터만 외부에 노출하여 보안을 높일 수 있습니다.
3. **유지보수성**: 데이터 모델의 변경에 따라 DTO를 쉽게 수정할 수 있어, 시스템 통합이나 변경 시 더 유연하게 대처할 수 있습니다.

> 비유: DTO는 필요한 데이터만 담는 "상자"처럼, 다양한 시스템 간에 데이터를 안전하고 간편하게 전달하는 역할을 합니다. 불필요한 정보는 제외하고, 수취인이 필요한 정보만 받을 수 있도록 하는 것이라고 볼 수 있습니다.
