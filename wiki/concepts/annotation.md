---
title: Annotation (어노테이션)
type: concept
tags: [spring, java]
created: 2026-07-16
updated: 2026-07-16
related: ["[[controller-service-dao-vo]]"]
---

# Annotation (어노테이션)

**한 줄 요약:** Java 코드에 @로 시작하는 메타데이터를 붙여서 Spring이 자동으로 처리하도록 지시하는 문법이다. XML 설정을 대신하거나 클래스/메서드의 역할을 명확히 선언할 때 사용한다.

## 개념

Annotation은 코드에 "이 클래스는 Controller야", "이 메서드는 /board/list URL을 처리해" 같은 정보를 붙여두는 것이다. Spring이 프로젝트를 시작할 때 이 어노테이션들을 읽어서 자동으로 설정을 완료한다.

## Controller 관련 어노테이션

```java
@Controller
// 이 클래스가 Spring MVC의 Controller임을 선언, 반환값을 JSP 이름(View)으로 사용
public class BoardController { }

@RestController
// @Controller + @ResponseBody 합친 버전, 반환값을 JSON으로 자동 변환 (Ajax API 용)
public class BoardApiController { }

@RequestMapping("/board")
// URL 경로를 이 클래스 또는 메서드에 매핑
public class BoardController { }

@GetMapping("/list")     // GET 요청만 처리
@PostMapping("/insert")  // POST 요청만 처리
@PutMapping("/update")   // PUT 요청만 처리
@DeleteMapping("/delete") // DELETE 요청만 처리
```

## 파라미터 수신 관련 어노테이션

```java
// @RequestParam - URL 쿼리 파라미터 또는 폼 데이터 수신
@RequestMapping("/view")
public String view(@RequestParam int boardId, Model model) {
    // /board/view?boardId=1 -> boardId = 1
}

// @PathVariable - URL 경로 변수 수신
@GetMapping("/{id}")
public BoardVO view(@PathVariable int id) {
    // /board/5 -> id = 5
}

// @RequestBody - JSON 요청 바디를 VO로 자동 변환
@PostMapping("/insert")
public String insert(@RequestBody BoardVO vo) {
    // Ajax에서 JSON.stringify()로 보낸 데이터를 VO로 받음
}
```

## Spring Bean 등록 관련 어노테이션

```java
@Service
// ServiceImpl 클래스에 붙임, Spring이 이 클래스를 Service Bean으로 등록하고 관리
public class BoardServiceImpl implements BoardService { }

@Repository
// DAO 클래스에 붙임 (MyBatis 환경에서는 보통 생략 가능), DB 접근 계층임을 선언
public class BoardDAOImpl implements BoardDAO { }

@Component
// 위 세 가지에 해당하지 않는 일반 Spring Bean에 붙임
public class FileUtils { }

@Autowired
// Spring이 관리하는 Bean을 자동으로 주입받음, 직접 new 하지 않아도 Spring이 알아서 넣어줌
@Service
public class BoardServiceImpl implements BoardService {
    @Autowired
    private BoardDAO boardDAO;
}
```

## 실무에서 자주 쓰는 어노테이션 정리표

| 어노테이션 | 위치 | 역할 |
|---|---|---|
| @Controller | 클래스 | MVC Controller 선언, JSP 반환 |
| @RestController | 클래스 | REST API Controller, JSON 반환 |
| @RequestMapping | 클래스/메서드 | URL 매핑 |
| @GetMapping | 메서드 | GET 요청 URL 매핑 |
| @PostMapping | 메서드 | POST 요청 URL 매핑 |
| @RequestParam | 파라미터 | 쿼리스트링/폼 데이터 수신 |
| @PathVariable | 파라미터 | URL 경로 변수 수신 |
| @RequestBody | 파라미터 | JSON 바디 -> VO 자동 변환 |
| @ResponseBody | 메서드 | 반환값을 JSON으로 변환 |
| @Service | 클래스 | Service Bean 등록 |
| @Repository | 클래스 | DAO Bean 등록 |
| @Component | 클래스 | 일반 Bean 등록 |
| @Autowired | 필드/생성자 | Bean 자동 주입 |

## 헷갈리기 쉬운 케이스

```java
// @RequestParam -> /board/view?id=1
public String view(@RequestParam int id) { }

// @PathVariable -> /board/view/1
@GetMapping("/view/{id}")
public String view(@PathVariable int id) { }

// @Autowired - new 없이 객체를 사용할 수 있는 이유
// 잘못된 방식 (직접 new): Spring 관리 밖
BoardService boardService = new BoardServiceImpl();

// 올바른 방식 (Spring이 주입)
@Autowired
private BoardService boardService;
```

> 팁: 어노테이션은 XML 설정을 코드로 옮긴 것이다.
