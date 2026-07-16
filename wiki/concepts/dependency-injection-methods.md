---
title: 의존성 주입의 방법들
type: concept
tags: [spring, oop]
created: 2026-07-16
updated: 2026-07-16
related: ["[[di-ioc-orm]]"]
---

# 의존성 주입의 방법들

의존성 주입(Dependency Injection, DI)은 객체의 의존성을 외부에서 주입하는 방법으로, 코드의 결합도를 낮추고 유연성을 높이는 데 도움이 됩니다. 다양한 의존성 주입 방법이 있으며, 그 중에서도 생성자 주입이 가장 많이 사용됩니다.

## 1. 생성자 주입 (Constructor Injection)

의존성이 필요한 객체를 생성자의 매개변수로 전달하여 주입하는 방법입니다.

**장점:** 객체가 생성될 때 모든 의존성이 설정되므로, 불완전한 상태로 생성되는 것을 방지합니다. Mock 객체를 쉽게 주입하여 단위 테스트를 수행할 수 있습니다.

```typescript
class UserService {
    constructor(private userRepository: UserRepository) {}
}
```

> 비유 (식당 주문): 고객이 주문할 때 요리사가 필요한 모든 재료(의존성)를 준비하여 요리를 시작하는 것과 같습니다. 음식은 고객에게 제공되기 전에 모든 재료가 갖추어진 완전한 상태로 준비됩니다.

## 2. 세터 주입 (Setter Injection)

필요한 의존성을 세터 메서드를 통해 주입하는 방법입니다.

**장점:** 객체 생성 후, 필요할 때 의존성을 주입할 수 있습니다. 선택적인 의존성이 있을 때 유용합니다.
**단점:** 의존성이 나중에 설정되므로, 객체가 불완전한 상태로 사용될 수 있습니다.

```typescript
class UserService {
    private userRepository: UserRepository;

    setUserRepository(repository: UserRepository) {
        this.userRepository = repository;
    }
}
```

> 비유 (책장에 책 꽂기): 처음에는 비어 있는 책장(객체)에 나중에 책(의존성)을 꽂는 것과 비슷합니다. 결국 책장이 채워지지만, 처음에는 비어 있을 수 있어서 사용하기 전에 확인이 필요합니다.

## 3. 인터페이스 주입 (Interface Injection)

인터페이스를 통해 의존성을 주입하는 방법입니다. 이 방법은 덜 일반적이며, 주로 IoC 컨테이너와 함께 사용됩니다.

**장점:** 의존성 관리가 잘 됩니다.
**단점:** 구현이 복잡할 수 있으며, 코드 가독성을 저하시킬 수 있습니다.

```typescript
interface UserRepositoryInjector {
    setUserRepository(repository: UserRepository): void;
}

class UserService implements UserRepositoryInjector {
    private userRepository: UserRepository;

    setUserRepository(repository: UserRepository) {
        this.userRepository = repository;
    }
}
```

> 비유 (훈련과 자격증): 특정 직업에 종사하기 위해 특정 자격증(인터페이스)을 반드시 보유해야 하는 것과 같습니다. 자격증을 획득한 뒤에야 해당 작업을 수행할 수 있습니다.
