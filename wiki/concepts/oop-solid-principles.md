---
title: 객체 지향 프로그래밍 5가지 설계 원칙 (SOLID)
type: concept
tags: [oop, design-pattern]
created: 2026-07-16
updated: 2026-07-16
related: ["[[class-vs-instance]]", "[[di-ioc-orm]]"]
---

# 객체 지향 프로그래밍 5가지 설계 원칙 (SOLID)

## 1. 단일 책임 원칙 (Single Responsibility Principle, SRP)

클래스는 하나의 책임만 가져야 한다. 클래스는 단 하나의 역할이나 기능만 수행해야 하며, 하나의 클래스는 하나의 변경 이유만 가져야 한다.

```typescript
class ReportGenerator {
  generatePDF() {
    // PDF 생성
  }
}

class DatabaseSaver {
  saveToDatabase() {
    // 데이터베이스 저장
  }
}
// 각 클래스가 한 가지 책임만 담당
```

## 2. 개방-폐쇄 원칙 (Open/Closed Principle, OCP)

소프트웨어 엔티티(클래스, 모듈, 함수 등)는 확장에는 열려 있어야 하지만, 변경에는 닫혀 있어야 한다. 기존 코드를 변경하지 않고도 새로운 기능을 추가할 수 있어야 하며, 다형성과 인터페이스를 활용하여 확장성을 높인다.

```typescript
interface Discount {
  getDiscount(): number;
}

class SilverDiscount implements Discount {
  getDiscount(): number {
    return 10;
  }
}

class GoldDiscount implements Discount {
  getDiscount(): number {
    return 20;
  }
}
// 새로운 유형을 추가해도 기존 코드를 수정하지 않고 확장이 가능
```

## 3. 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)

자식 클래스는 언제나 부모 클래스를 대체할 수 있어야 한다. 상속받은 클래스는 부모 클래스의 기능을 온전히 수행할 수 있어야 하며, 부모 클래스의 동작 규칙을 어기면 안 된다.

```typescript
class Bird {
  layEggs() {
    console.log("알을 낳는다.");
  }
}

class FlyingBird extends Bird {
  fly() {
    console.log("날 수 있다.");
  }
}

class Penguin extends Bird {
  swim() {
    console.log("수영할 수 있다.");
  }
}
// Bird를 확장하면서 규칙을 어기지 않도록 설계
```

## 4. 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)

클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 한다. 큰 인터페이스를 여러 개의 작은 인터페이스로 분리한다.

```typescript
interface CanSwim {
  swim(): void;
}

interface CanRun {
  run(): void;
}

class Dog implements CanSwim, CanRun {
  swim() {
    console.log("강아지가 수영한다.");
  }
  run() {
    console.log("강아지가 뛴다.");
  }
}
// 필요한 기능만 인터페이스로 구현
```

## 5. 의존 역전 원칙 (Dependency Inversion Principle, DIP)

고수준 모듈은 저수준 모듈에 의존해서는 안 되며, 둘 다 추상화에 의존해야 한다. 구체적인 구현이 아니라 추상화된 인터페이스에 의존하도록 설계한다.

```typescript
interface DataStore {
  save(data: string): void;
}

class Database implements DataStore {
  save(data: string) {
    console.log("Saving " + data + " to database.");
  }
}

class App {
  constructor(private datastore: DataStore) {}

  saveData(data: string) {
    this.datastore.save(data);
  }
}
// App은 DataStore라는 추상화에 의존하므로, Database를 다른 구현으로 교체하기 쉽다
```
