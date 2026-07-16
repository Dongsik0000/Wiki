---
title: PK & FK (Primary Key, Foreign Key)
type: concept
tags: [db, sql]
created: 2026-07-16
updated: 2026-07-16
related: ["[[rdbms-vs-nosql]]"]
---

# PK & FK (Primary Key, Foreign Key)

## Primary Key (PK)

Primary Key는 데이터베이스 테이블에서 각 레코드를 고유하게 식별하기 위해 설정되는 속성입니다. 데이터베이스에서 레코드를 찾고 수정하는 데 중요한 역할을 합니다. 테이블의 모든 레코드는 Primary Key를 통해 유일하게 식별되며, 이는 데이터의 무결성을 확보하는 데 중요한 요소입니다.

**특징:**
- 유일성: 각 레코드의 고유한 식별자로, 테이블 내에서 동일한 값을 가진 레코드가 존재할 수 없습니다. 데이터 중복을 방지합니다.
- NULL 불가: Primary Key 필드는 반드시 값을 가져야 하며, NULL 값은 허용되지 않습니다.
- 자동 인덱스 생성: 대부분의 DBMS는 Primary Key 관련 필드에 대해 자동으로 인덱스를 생성하여 검색 성능을 향상시킵니다.
- Composite Key: 두 개 이상의 필드를 조합하여 Primary Key를 만들 수도 있습니다. 이 경우 조합된 모든 필드의 값이 유일해야 합니다.

```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY,
    UserName VARCHAR(50),
    Email VARCHAR(100)
);
```

## Foreign Key (FK)

Foreign Key는 한 테이블에서 다른 테이블의 Primary Key를 참조하는 속성입니다. 이 관계를 통해 두 테이블 간의 관계를 형성하고, 데이터베이스의 무결성을 유지할 수 있습니다. 부모-자식 관계를 나타내며, 레코드 간의 데이터 일관성을 보장합니다.

**특징:**
- 관계 설정: 한 테이블의 특정 필드가 다른 테이블의 Primary Key를 참조할 수 있습니다. Foreign Key가 설정된 테이블을 자식 테이블, 참조되는 테이블을 부모 테이블이라고 합니다.
- NULL 가능: Foreign Key는 NULL 값을 가질 수 있습니다. 특정 레코드가 다른 테이블의 레코드와 연결되지 않을 수 있음을 나타냅니다.
- 데이터 무결성: 외래 키로 참조하는 레코드가 삭제되면, 연결된 레코드의 존재 여부에 따라 여러 제약 조건을 설정할 수 있습니다 (ON DELETE, ON UPDATE).
  - ON DELETE CASCADE: 부모 테이블에서 레코드가 삭제되면, 자식 테이블의 해당 Foreign Key를 가진 레코드도 함께 삭제됩니다.
  - ON DELETE SET NULL: 부모 테이블에서 레코드가 삭제될 때, 자식 테이블의 해당 Foreign Key 값을 NULL로 설정합니다.

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    UserID INT,
    OrderDate DATETIME,
    FOREIGN KEY (UserID) REFERENCES Users(UserID)
);
```
