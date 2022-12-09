# 1-2 데이터베이스

## 정보 시스템 구축 단계
* 분석-설계-구현-시험-유지 · 보수

## 데이터베이스 필수 용어
* 데이터 : 테이블에 저장된 하나하나의 단편적인 정보
* 테이블 : 데이터를 입력하기 위해 표 형태로 만든 것
* 데이터베이스 : 테이블이 저장되는 저장소로, 원통 모양으로 표현
* DBMS : DataBase Management System의 약자로, 데이터베이스를 관리하는 시스템 또는 소프트웨어
* 열(필드) : 각 테이블을 열로 구성
* 열 이름 : 각 열을 구분하기 위한 이름
* 데이터 형식 : 열의 데이터 형식
* 행(레코드) : 실질적인 데이터
* 기본키(주키) : 각 행을 구분하는 유일한 열로, 기본키는 중복되어서도 비어 있어서도 안 됨
* 외래키 : 두 테이블의 관계를 맺어주는 키
* SQL(구조화된 질의 언어) : 사람과 DBMS가 소통하기 위한 말(언어)

# SELECT 문 작성하기
## 회원 테이블의 모든 데이터 조회
SELECT * FROM memberTBL;

## 회원 테이블의 이름과 주소만 출력
SELECT memberName, memberAddress FROM memberTBL;

## ‘토마스’에 대한 정보만 추출
SELECT * FROM memberTBL WHERE memberName = ‘토마스’;
--------------------------------------------------------
## 간단한 테이블을 생성하는 SQL 문 실행
CREATE TABLE `my testTBL` (id INT);

## DROP TABLE 문을 사용하여 테이블 삭제
DROP TABLE `my TestTBL`;
-------------------------------------------------------
## 요약된 SELECT 문의 형식
SELECT select_expr
[FROM table_references]   
[WHERE where_condition]   
[GROUP BY {col_name |expr |position}]   
[HAVING where_condition]   
[ORDER BY {col_name |expr |position}]

## 더 요약된 SELECT 문의 형식
SELECT 열이름   
FROM 테이블이름   
WHERE 조건

## 현재 사용하는 데이터베이스를 지정하거나 변경하는 구문 형식
USE 데이터베이스이름;   EX)USE employees;

## 쿼리 창을 연 후 자신이 작업할 데이터베이스가 선택되어 있는지 먼저 확인하는 습관을 들여야 함
USE mysql;   
SELECT * FROM employees;

## 여러 개의 열을 가져오고 싶으면 쉼표(,)로 구분
SELECT first_name, last_name, gender FROM employees;

## 원하는 열만 검색
SELECT first_name FROM employees;

# WHERE 절
## SELECT … FROM 문에 WHERE 절을 추가하면 특정한 조건을 만족하는 데이터만 조회할 수있음
SELECT 열이름 FROM 테이블이름 WHERE 조건식;

## WHERE 절 없이 cookDB의 회원 테이블(userTBL) 조회
USE cookDB;   
SELECT * FROM userTBL;

## 원 테이블(userTBL)에서 강호동의 정보만 조회
SELECT * FROM userTBL WHERE userName = '강호동';

## 회원 테이블에서 1970년 이후에 출생했고 키가 182cm 이상인 사람의 아이디와 이름을 조회
SELECT userID, userName FROM userTBL WHERE birthYear >= 1970 AND height >= 182;

## 1970년 이후에 출생했거나 키가 182cm 이상인 사람의 아이디와 이름 조회
SELECT userID, userName FROM userTBL WHERE birthYear >= 1970 OR height >= 182;

# BETWEEN … AND, IN( ), LIKE 연산자
