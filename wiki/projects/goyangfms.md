---
title: goyangFms (고양시 자율주행 관제 시스템)
type: project
tags: [egovframework, spring, mybatis, autonomous-driving]
created: 2026-07-16
updated: 2026-07-16
related: ["[[../concepts/mvc-pattern]]", "[[../concepts/controller-service-dao-vo]]", "[[../concepts/mybatis-dynamic-sql]]", "[[../concepts/postgresql]]", "[[../concepts/svn]]", "[[../concepts/csrf]]", "[[../concepts/jstl]]", "[[../concepts/jsp-spring-mvc]]", "[[../concepts/rest-api]]"]
---

# goyangFms (고양시 자율주행 관제 시스템)

## 개요

고양시 자율주행 관제 시스템. eGovFramework 기반 웹 애플리케이션. **완성됨** (개발 완료 상태).

- 로컬 경로: `C:\dev\workspace\goyangFms`
- Maven artifactId: `goyangFms`

## 기술 스택

- eGovFramework (Spring MVC + MyBatis)
- PostgreSQL
- Spring Security
- JSP + JSTL
- 버전관리: SVN (git 아님)

## 아키텍처 공통 패턴

컨트롤러 코드를 실제로 읽어보고 확인한, 이 프로젝트 전반에 반복되는 패턴들:

- **로그인 체크**: 화면 진입 시마다 `session`에서 `loginId`를 확인하고, 없으면
  `redirect:/login`으로 보낸다. 모든 화면 컨트롤러 메서드 첫 줄에 반복됨.
- **CSRF 토큰 발급**: GET 요청일 때 세션에 `SESSION_CSRF_TOKEN`이 없으면
  `UUID.randomUUID()`로 새로 발급 ([[../concepts/csrf|CSRF 개념]] 참고).
- **메뉴 접속 카운트 로깅**: 화면에 들어갈 때마다 `CmmnService.selectMenuAcsCnt`로
  오늘 날짜 기준 접속 기록이 있는지 확인하고, 있으면 update, 없으면 insert —
  메뉴별 접속 통계를 매일 단위로 쌓는 구조로 보임 (대시보드 통계용으로 추정).
- **공통 API 응답 포맷**: `com.fms.cmmn.util.ApiResponse` 클래스로 API 응답을
  통일 (아직 내용은 미확인).
- **엑셀 내보내기**: `ExcelUtil`로 차트 이미지(base64로 받은 걸 디코딩)까지
  포함한 엑셀 파일을 생성해서 내려주는 기능이 이력 조회 화면들에 있음.

## 모듈 구조

`com.fms` 패키지 기준, Controller/Service/DAO 계층 구조 ([[../concepts/controller-service-dao-vo|관련 개념]]):

### cmmn — 공통 기능
- `CmmnController` / `CmmnApiController` / `OpenApiController` (외부 연동용으로 추정)
- `SessionUtil`, `CommonUtil`, `ExcelUtil`, `ApiResponse`
- `WeatherApiUtil` — 날씨 API 연동 (대시보드 표시용으로 추정, 미확인)
- `ContentSecurityPolicyFilter` — CSP 보안 헤더 필터

### cmmnCodeMgmt — 공통코드관리
- Controller/DAO/Service만 확인, 상세 화면 미확인

### dashboard — 대시보드
- `DashBoardController` (화면) / `DashBoardApiController` (데이터 API)
- 화면: `dashBoardMain.jsp`

### userMgmt — 회원관리
- `UserMgmtController/ApiController` — 회원관리
- `UserIpMgmtController(Api)` — IP 관리 (접속 허용 IP 화이트리스트로 추정, 미확인)

### vehicleMgmt — 차량관리 (실제 화면 4개, `VehicleMgmtController` 확인됨)
- `/fms/vehicleMgmt/vehicleMgmtMain` — 차량 관리
- `/fms/vehicleMgmt/routeMgmtMain` — 노선 관리
- `/fms/vehicleMgmt/scheduleMgmtMain` — 일정(스케줄) 관리
- `/fms/vehicleMgmt/boardingMgmtMain` — 승하차(탑승) 관리
- 별도 `BoardingExternalApiController` 존재 — 외부 시스템에서 탑승 데이터를
  받는 API로 추정 (미확인)
- Mapper: `vehicleMgmt.xml`, `routeMgmt.xml`, `scheduleMgmt.xml`, `boardingMgmt.xml`

### vehicleHistory — 차량 이력 조회 (실제 화면 6개, `VehicleHistoryController` 확인됨)
- `/fms/vehicleHistory/runHistoryMain` — 운행 이력 조회
- `/fms/vehicleHistory/driveHistoryMain` — 주행 이력 조회
- `/fms/vehicleHistory/sensorHistoryMain` — 센서 이력 조회
- `/fms/vehicleHistory/eventHistoryMain` — 이벤트 이력 조회
- `/fms/vehicleHistory/boardingHistoryMain` — 탑승 이력 조회
- `/fms/vehicleHistory/userHistoryMain` — 로그인/접근 이력 조회
- `/fms/vehicleHistory/historyExcel` (POST) — 위 조회 화면들의 차트+표를
  엑셀로 내보내는 공통 API
- Mapper: `vehicleHistory.xml`

### login
- Mapper `login.xml`은 존재하지만, 대응하는 Java 컨트롤러 위치는 아직
  못 찾음 (cmmn 쪽에 있을 가능성 — 확인 필요)

## 아직 확인 못한 것 (다음에 볼 것)

- `login` 처리 컨트롤러 실제 위치
- `WeatherApiUtil`, `OpenApiController`, `BoardingExternalApiController`의
  실제 동작 (외부 연동 대상이 뭔지)
- DB 스키마 (테이블 목록, 관계) — Mapper.xml 안 SQL을 보면 알 수 있음
- Spring Security 설정 (권한 구조, 로그인 흐름)
- PostgreSQL 접속 정보가 있는 설정 파일 위치 (`egovProps`) — 자격증명은 위키에 절대 기록하지 않음

## 현재 상태

- **완성됨.** 개발이 끝난 상태.
- 2026-07-16: 코드 구조(Controller/Service/DAO/Mapper)를 실제로 읽고 이 페이지를
  상세화. 화면·API 목록은 코드에서 직접 확인한 것과, 이름만 보고 추정한 것을
  구분해서 적어둠 (위 "아직 확인 못한 것" 참고).
