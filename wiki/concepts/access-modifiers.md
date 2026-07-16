---
title: 접근제어자 (public, private, protected, default)
type: concept
tags: [java, oop]
created: 2026-07-16
updated: 2026-07-16
related: []
---

# 접근제어자 (public, private, protected, default)

**한 줄 요약:** 클래스, 메서드, 변수에 누가 접근할 수 있는지 범위를 제한하는 키워드다. 외부에서 함부로 내부 데이터를 바꾸지 못하게 막는 역할을 한다.

## 4가지 접근제어자 비교

| 접근제어자 | 같은 클래스 | 같은 패키지 | 자식 클래스 | 외부 클래스 |
|---|---|---|---|---|
| public | O | O | O | O |
| protected | O | O | O | X |
| default (생략) | O | O | X | X |
| private | O | X | X | X |

## 실무 사용법

```java
public class BoardVO {

    // private: 외부에서 직접 접근 금지 -> getter/setter로만 접근
    private int boardId;
    private String title;
    private String writer;

    // public: 어디서든 호출 가능한 getter/setter
    public int getBoardId() { return boardId; }
    public void setBoardId(int boardId) { this.boardId = boardId; }

    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }
}

// public: 다른 클래스에서 호출 가능한 메서드
@Service
public class BoardServiceImpl implements BoardService {

    @Autowired
    private BoardDAO boardDAO; // private: 이 클래스 안에서만 사용

    @Override
    public List<BoardVO> getBoardList() { // public: Controller에서 호출
        return boardDAO.selectBoardList();
    }

    // private: 이 클래스 내부에서만 쓰는 헬퍼 메서드
    private String encryptPassword(String pw) {
        return pw; // 암호화 로직
    }
}
```

## 왜 private + getter/setter를 쓰는가?

```java
// 필드를 public으로 열어두면 외부에서 마음대로 값을 바꿀 수 있다
public int boardId;
board.boardId = -1; // 유효하지 않은 값도 막을 수 없음

// private + setter로 검증 로직을 추가할 수 있다
private int boardId;
public void setBoardId(int boardId) {
    if (boardId <= 0) throw new IllegalArgumentException("유효하지 않은 ID");
    this.boardId = boardId;
}
```

## eGovFramework에서의 규칙 요약

| 위치 | 권장 접근제어자 | 이유 |
|---|---|---|
| VO 필드 | private | getter/setter로만 접근 |
| VO getter/setter | public | 외부에서 데이터 접근 |
| Service/DAO 필드 | private | 내부에서만 사용 |
| Controller 메서드 | public | Spring이 호출해야 함 |
| 내부 헬퍼 메서드 | private | 외부 노출 불필요 |

> 팁: 실무 원칙은 필드는 항상 private, 메서드는 외부에서 필요한 것만 public으로 하고, 나머지는 private으로 최대한 숨기는 것이 좋은 설계다.
