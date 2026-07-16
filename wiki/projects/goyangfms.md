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

고양시 자율주행 관제 시스템. eGovFramework 기반 웹 애플리케이션.

- 로컬 경로: `C:\dev\workspace\goyangFms`
- Maven artifactId: `goyangFms`

## 기술 스택

- eGovFramework (Spring MVC + MyBatis)
- PostgreSQL
- Spring Security
- JSP + JSTL
- 버전관리: SVN (git 아님)

## 모듈 구조

`com.fms` 패키지 기준, Controller/Service/DAO 계층 구조 ([[../concepts/controller-service-dao-vo|관련 개념]]):

- `cmmn` — 공통 기능
- `cmmnCodeMgmt` — 공통코드관리
- `dashboard` — 대시보드
- `userMgmt` — 회원관리
- `vehicleMgmt` — 차량관리 (탑승/노선/일정 관리 포함)
- `vehicleHistory` — 차량이력 (운행/탑승/이벤트/사용자 이력)

## 현재 상태

- 2026-07-16: 코드 구조를 훑어보고 첫 프로젝트 페이지 생성. 파일 수정 시각 기준으로
  최근 작업 흐름은 dashboard(`dashBoardMain.jsp`) → vehicleMgmt(탑승/노선/일정
  관리) → vehicleHistory(운행/탑승/이벤트/사용자 이력) 순으로 보인다.
- SVN 커밋 이력은 로컬에 svn 클라이언트가 없어 아직 확인하지 못함.
- 다음 ingest 때 실제 작업 내용(결정 사항, 이슈, 진행 상황)을 더 채워나갈 것.
