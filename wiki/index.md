# 위키 인덱스

이 문서는 `wiki/` 전체의 카탈로그다. 새 페이지가 생기거나 갱신될 때마다 함께 갱신한다.

## 프로젝트

- [[projects/llm-wiki|LLM Wiki]] — 이 위키 프로젝트 자체의 진행 기록 (2026-07-16)

## 개념

### 백엔드 / Spring / eGovFramework

- [[concepts/publisher-html-handoff|퍼블리셔에게 HTML 파일을 받았을 때]] — 퍼블리셔 HTML을 id 부여 후 JSP로 변환하는 절차 (2026-07-16)
- [[concepts/svn|SVN (Subversion)]] — 버전관리 핵심 개념과 Eclipse에서의 사용법 (2026-07-16)
- [[concepts/mvc-pattern|MVC 패턴]] — Model/View/Controller 분리와 Spring MVC 요청 흐름 (2026-07-16)
- [[concepts/controller-service-dao-vo|Controller/Service/DAO/Mapper/VO]] — 레이어별 역할 분리 구조 (2026-07-16)
- [[concepts/contextpath-vs-curl|contextPath vs c:url]] — 컨텍스트 경로를 안전하게 만드는 두 방식 (2026-07-16)
- [[concepts/business-logic|비즈니스 로직]] — 판단/규칙 코드는 ServiceImpl에 두는 이유 (2026-07-16)
- [[concepts/rest-api|REST API]] — @RestController와 HTTP 메서드로 자원을 다루는 설계 (2026-07-16)
- [[concepts/jsp-spring-mvc|JSP & Spring MVC]] — DispatcherServlet부터 JSP 렌더링까지의 요청 흐름 (2026-07-16)
- [[concepts/postgresql|PostgreSQL]] — MySQL과의 차이 및 eGov 연동 방법 (2026-07-16)
- [[concepts/annotation|Annotation]] — Spring 주요 어노테이션 정리 (2026-07-16)
- [[concepts/access-modifiers|접근제어자]] — public/private/protected/default 비교 (2026-07-16)
- [[concepts/jsp-include|JSP include]] — 공통 헤더/푸터 재사용 방법 (2026-07-16)
- [[concepts/mybatis-dynamic-sql|MyBatis 동적 SQL]] — if/foreach/choose/where/set 태그 (2026-07-16)
- [[concepts/mybatis-resultmap|MyBatis resultMap]] — 컬럼명과 VO 필드명 수동 매핑 (2026-07-16)
- [[concepts/mybatis-xml-escape|MyBatis XML 특수문자 처리]] — 이스케이프와 CDATA (2026-07-16)
- [[concepts/pagination|페이징 처리]] — offset 계산과 Mapper/JS 렌더링 (2026-07-16)
- [[concepts/map-hashmap|Map & HashMap]] — MyBatis parameterType/resultType=map 활용 (2026-07-16)
- [[concepts/jstl|JSTL]] — c:forEach/c:if/c:choose 등 JSP 태그 라이브러리 (2026-07-16)
- [[concepts/el-expression-language|EL (Expression Language)]] — ${} 문법과 연산자 (2026-07-16)
- [[concepts/dto|DTO]] — 계층 간 데이터 전송 객체 (2026-07-16)
- [[concepts/pk-fk|PK & FK]] — Primary Key/Foreign Key와 제약조건 (2026-07-16)
- [[concepts/rdbms-vs-nosql|RDBMS와 NoSQL]] — 관계형/비관계형 DB 비교 (2026-07-16)
- [[concepts/di-ioc-orm|IoC, DI, ORM]] — 제어의 역전과 객체-관계 매핑 (2026-07-16)
- [[concepts/dependency-injection-methods|의존성 주입의 방법들]] — 생성자/세터/인터페이스 주입 비교 (2026-07-16)
- [[concepts/persistence-context|영속성 컨텍스트]] — ORM의 객체 상태 관리 (2026-07-16)
- [[concepts/dirty-checking|Dirty Checking]] — 변경 감지로 필요한 데이터만 업데이트 (2026-07-16)
- [[concepts/singleton-pattern|싱글턴 패턴]] — 인스턴스를 하나만 생성하는 디자인 패턴 (2026-07-16)
- [[concepts/cookie-session|쿠키와 세션]] — 클라이언트/서버 상태 유지 방식 비교 (2026-07-16)
- [[concepts/authentication-vs-authorization|인증과 인가]] — 신원 확인 vs 접근 권한 부여 (2026-07-16)
- [[concepts/csrf|CSRF]] — 사이트 간 요청 위조 공격과 토큰 방어 (2026-07-16)
- [[concepts/validation-regex|유효성 체크 & 정규 표현식]] — 입력 검증과 대표 정규식 패턴 (2026-07-16)
- [[concepts/query-string-vs-path-variable|Query String / Path Variable]] — URL로 데이터를 전달하는 두 방식 (2026-07-16)
- [[concepts/fiddler|Fiddler]] — HTTP 트래픽을 가로채 확인하는 디버깅 툴 (2026-07-16)
- [[concepts/http-status-codes|HTTP 상태코드]] — 2xx~5xx 분류와 디버깅 체크리스트 (2026-07-16)
- [[concepts/browser-server-flow|클라이언트-서버 흐름]] — 브라우저 요청부터 렌더링까지의 전체 과정 (2026-07-16)

### 프론트엔드 / JavaScript

- [[concepts/jquery-ajax-vs-fetch|jQuery Ajax vs fetch]] — $.ajax와 fetch API 비교 (2026-07-16)
- [[concepts/truthy-falsy|Truthy & Falsy]] — JS의 참/거짓으로 취급되는 값들 (2026-07-16)
- [[concepts/for-vs-foreach|for vs forEach]] — 반복문 선택 기준과 break 가능 여부 (2026-07-16)
- [[concepts/console-log-debugging|console.log 디버깅 패턴]] — table/group/time 등 실전 디버깅 메서드 (2026-07-16)
- [[concepts/web-storage|localStorage / sessionStorage]] — 브라우저 저장소 두 종류의 차이 (2026-07-16)
- [[concepts/null-check-patterns|null 체크 패턴]] — Java/MyBatis/JS에서의 방어 코드 (2026-07-16)
- [[concepts/devtools-usage|개발자 도구 사용법 (F12)]] — Elements/Console/Network/Sources/Application 탭 (2026-07-16)
- [[concepts/async-await|async & await]] — 비동기 작업을 동기 코드처럼 다루는 문법 (2026-07-16)
- [[concepts/promise|Promise 객체]] — 비동기 작업의 완료/실패를 나타내는 객체 (2026-07-16)

### 객체지향 / 기본 개념

- [[concepts/class-vs-instance|클래스와 인스턴스의 차이]] — 설계도(클래스)와 실제 제품(인스턴스) (2026-07-16)
- [[concepts/oop-solid-principles|OOP SOLID 5원칙]] — SRP/OCP/LSP/ISP/DIP (2026-07-16)
- [[concepts/try-catch-finally|try-catch-finally / 예외 처리]] — 예외 처리 기본 구조와 eGov 패턴 (2026-07-16)
- [[concepts/camel-case|Camel Case]] — camelCase/PascalCase/snake_case/kebab-case 비교 (2026-07-16)

## 결정

(아직 없음)
