# 작업 로그

Append-only 기록. 형식: `## [YYYY-MM-DD] <종류> | <제목>` (종류: ingest / query / lint / setup)

## [2026-07-16] setup | LLM Wiki 뼈대 생성

raw/, wiki/ 3계층 구조와 CLAUDE.md 스키마, index.md, 이 로그를 생성.
Obsidian vault로 이 폴더를 열었고, 로컬 git 저장소로 버전 관리 시작.

## [2026-07-16] ingest | 노션 "개념 정리" 페이지 일괄 ingest (48개 개념)

노션 페이지(1.분석 17개, 2.분석 14개, 3.분석 하위 17개)를 Notion MCP로 읽어
wiki/concepts/ 에 48개 개념 페이지로 정리. wiki/index.md 개념 섹션에 백엔드/
Spring/eGovFramework(35), 프론트엔드/JavaScript(9), 객체지향/기본개념(4)
세 카테고리로 링크 추가. raw/articles/에는 전문 재복제 대신 출처
매니페스트(제목+URL 목록)를 저장 — 노션 자체가 살아있는 원본이므로 중복 저장을
피함.

## [2026-07-16] lint | 48개 개념 페이지 SQL 오류 점검 및 수정

heredoc 작성 중 bash 홑따옴표 충돌을 피하려고 SQL 예제의 문자열 리터럴
('%', 'board_seq')을 겹따옴표로 바꿔 옮긴 부분을 발견. PostgreSQL에서
겹따옴표는 식별자용이라 실제로는 문법 오류였음. map-hashmap.md,
mybatis-dynamic-sql.md, pagination.md, postgresql.md의 CONCAT(...),
nextval(...), 문자열 연결 예제 총 8곳을 홑따옴표로 수정. 나머지 46개
페이지는 기술 내용 검토 결과 특이사항 없음.
