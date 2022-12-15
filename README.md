# 1-2 데이터베이스

>## 정보 시스템 구축 단계
* 분석-설계-구현-시험-유지 · 보수

>## 데이터베이스 필수 용어
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
>## 회원 테이블의 모든 데이터 조회
SELECT * FROM memberTBL;

>## 회원 테이블의 이름과 주소만 출력
SELECT memberName, memberAddress FROM memberTBL;

>## ‘토마스’에 대한 정보만 추출
SELECT * FROM memberTBL WHERE memberName = ‘토마스’;

--------------------------------------------------------
>## 간단한 테이블을 생성하는 SQL 문 실행
CREATE TABLE `my testTBL` (id INT);

>## DROP TABLE 문을 사용하여 테이블 삭제
DROP TABLE `my TestTBL`;

-------------------------------------------------------
>## 요약된 SELECT 문의 형식
SELECT select_expr
[FROM table_references]   
[WHERE where_condition]   
[GROUP BY {col_name |expr |position}]   
[HAVING where_condition]   
[ORDER BY {col_name |expr |position}]

>## 더 요약된 SELECT 문의 형식
SELECT 열이름   
FROM 테이블이름   
WHERE 조건

>## 현재 사용하는 데이터베이스를 지정하거나 변경하는 구문 형식
USE 데이터베이스이름;   EX)USE employees;

>## 쿼리 창을 연 후 자신이 작업할 데이터베이스가 선택되어 있는지 먼저 확인하는 습관을 들여야 함
USE mysql;   
SELECT * FROM employees;

>## 여러 개의 열을 가져오고 싶으면 쉼표(,)로 구분
SELECT first_name, last_name, gender FROM employees;

>## 원하는 열만 검색
SELECT first_name FROM employees;

# WHERE 절
>## SELECT … FROM 문에 WHERE 절을 추가하면 특정한 조건을 만족하는 데이터만 조회할 수있음
SELECT 열이름 FROM 테이블이름 WHERE 조건식;

>## WHERE 절 없이 cookDB의 회원 테이블(userTBL) 조회
USE cookDB;   
SELECT * FROM userTBL;

>## 원 테이블(userTBL)에서 강호동의 정보만 조회
SELECT * FROM userTBL WHERE userName = '강호동';

>## 회원 테이블에서 1970년 이후에 출생했고 키가 182cm 이상인 사람의 아이디와 이름을 조회
SELECT userID, userName FROM userTBL WHERE birthYear >= 1970 AND height >= 182;

>## 1970년 이후에 출생했거나 키가 182cm 이상인 사람의 아이디와 이름 조회
SELECT userID, userName FROM userTBL WHERE birthYear >= 1970 OR height >= 182;

# BETWEEN … AND, IN( ), LIKE 연산자
>## 회원 테이블에서 키가 180~182cm인 사람 조회
SELECT userName, height FROM userTBL WHERE height >= 180 AND height <= 182;

>## 위 쿼리문은 BETWEEN … AND 연산자를 사용하여 다음과 같이 작성
SELECT userName, height FROM userTBL WHERE height BETWEEN 180 AND 182;

>## 지역이 경남 또는 충남 또는 경북인 사람은 OR 연산자를 사용하여 조회
SELECT userName, addr FROM userTBL WHERE addr='경남' OR addr='충남' OR addr='경북';

>## 성이 김 씨인 회원의 이름과 키 조회
SELECT userName, height FROM userTBL WHERE userName LIKE '김%';

>## 맨 앞의 한 글자가 무엇이든 상관없고 그다음이 ‘경규’인 사람 조회
SELECT userName, height FROM userTBL WHERE userName LIKE '_경규';

# 서브쿼리와 ANY, ALL, SOME 연산자
>## 김용만보다 키가 크거나 같은 사람의 이름과 키 출력
SELECT userName, height FROM userTBL WHERE height > 177;   
SELECT userName, height FROM userTBL    
  WHERE height > (SELECT height FROM userTBL WHERE userName = '김용만');

>## 지역이 경기인 사람보다 키가 크거나 같은 사람 추출
SELECT userName, height FROM userTBL   
  WHERE height >= (SELECT height FROM userTBL WHERE addr = '경기');

# ORDER BY 절
>## 가입한 순서대로 회원 출력(기본적으로 오름차순(ascending)으로 정렬)
SELECT userName, mDate FROM userTBL ORDER BY mDate;

>## 내림차순(descending)으로 정렬(열 이름 뒤에 DESC를 넣음)
SELECT userName, mDate FROM userTBL ORDER BY mDate DESC;

>## 정렬 기준을 2개로 설정하고 정렬
SELECT userName, height FROM userTBL ORDER BY height DESC, userName ASC;

# CREATE TABLE … SELECT 문
>## CREATE TABLE … SELECT 구문 형식
CREATE TABLE 새로운테이블 (SELECT 복사할열 FROM 기존테이블)

# GROUP BY 절
>## SELECT 문의 형식 중에서 GROUP BY … HAVING 절의 위치\
SELECT select_expr   
[FROM table_references]   
[WHERE where_condition]   
[GROUP BY {col_name | expr | position}]   
[HAVING where_condition]   
[ORDER BY {col_name | expr | position}]   

>## cookDB의 구매 테이블 (buyTBL)에서 아이디(userID)마다 구매한 물건의 개수(amount)를 조회하는 쿼리문
USE cookDB;   
SELECT userID, amount FROM buyTBL ORDER BY userID;

>##  같은 아이디(userID)끼리 GROUP BY 절로 묶은 후 SUM( ) 함수로 구매 개수(amount)를 합치는 방식
SELECT userID, SUM(amount) FROM buyTBL GROUP BY userID;

>## 별칭을 사용하여 열 이름을 이해하기 좋게 변경
SELECT userID AS '사용자 아이디', SUM(amount) AS '총 구매 개수’   
  FROM buyTBL GROUP BY userID
  
# 집계 함수
|함수|설명|
|---|---|
|AVG()|평균을 구한다|
|MIN()|최솟값을 구한다|
|MAX()|최댓값을 구한다|
|COUNT()|행의 개수를 샌다|
|COUNT(DISTNCT)|행의 개수를 샌다(중복은 1개만 인정)|
|STDEV()|표준편차를 구한다|
|VAR_SAMP()|분산을 구한다|

>## 전체적으로 한 번 구매할 때마다 평균 몇 개를 구매했는지 조회
SELECT AVG(amount) AS '평균 구매 개수' FROM buyTBL;

>## 회원별로 한 번 구매할 때마다 평균적으로 몇 개를 구매했는지 조회(GROUP BY 절 사용)
SELECT userID, AVG(amount) AS '평균 구매 개수’   
  FROM buyTBL GROUP BY userID;
  
>## 가장 키가 큰 회원과 가장 키가 작은 회원의 이름과 키 출력
SELECT userName, MAX(height), MIN(height) FROM userTBL;

>## GROUP BY 절을 활용하여 수정
SELECT userName, MAX(height), MIN(height)   
  FROM userTBL GROUP BY userName;
  
>## 서브쿼리와 조합하여 다시 실행
SELECT userName, height   
 FROM userTBL   
 WHERE height = (SELECT MAX(height) FROM userTBL)   
  OR height = (SELECT MIN(height) FROM userTBL);
  
>## 휴대폰이 있는 회원의 수(의도와 다르게 전체 회원이 조회됨)
SELECT COUNT( * ) FROM userTBL;

>## 휴대폰이 있는 회원만 세려면 휴대폰 열이름(mobile1)을 지정해야 함
SELECT COUNT(mobile1) AS '휴대폰이 있는 사용자' FROM userTBL;

# HAVING 절
>## 아이디별 총구매액 구하기
USE cookDB;   
SELECT userID AS '사용자', SUM(price * amount) AS '총구매액’   
  FROM buyTBL
  GROUP BY userID;
  
>## 총 구매액이 1000 이상인 회원에게만 사은품을 증정하고 싶다면?
SELECT userID AS '사용자', SUM(price * amount) AS '총구매액’   
  FROM buyTBL   
  WHERE SUM(price * amount) > 1000   
  GROUP BY userID
  
>## HAVING 절을 사용하여 다시 작성
SELECT userID AS '사용자', SUM(price * amount) AS '총구매액’
  FROM buyTBL   
  GROUP BY userID   
  HAVING SUM(price * amount) > 1000;
  
>## 총 구매액이 적은 회원 순으로 정렬(ORDER BY 절 사용)
SELECT userID AS '사용자', SUM(price * amount) AS '총구매액’
  FROM buyTBL   
  GROUP BY userID   
  HAVING SUM(price * amount) > 1000   
  ORDER BY SUM(price * amount);
  
# INSERT 문
>## 테이블에 데이터를 삽입하는 명령어
INSERT [INTO] 테이블이름[(열1, 열2, …)] VALUES (값1, 값2, …)

>## INSERT 문에서 테이블 이름 다음에 나오는 열 생략 가능(단 열의 순서 및 개수는 동일해야 함)
USE cookDB;   
CREATE TABLE testTBL1 (id int, userName char(3), age int);   
INSERT INTO testTBL1 VALUES (1, '뽀로로', 16);

>## id와 이름만 입력하고 나이는 입력하고 싶지 않다면
INSERT INTO testTBL1 (id, userName) VALUES (2, '크롱');

>## 열의 순서를 바꾸어 입력하고 싶을 때
INSERT INTO testTBL1 (userName, age, id) VALUES ('루피', 14, 3);

>## AUTO_INCREMENT 키워드
>##  자동으로 1부터 증가하는 값을 입력하는 키워드
* 특정 열을 AUTO_INCREMENT로 지정할 때는 반드시 PRIMARY KEY(기본키) 또는 UNIQUE(유일한 값)로 설정해야 함
* 데이터 형식이 숫자인 열에만 사용 가능
* AUTO_INCREMENT로 지정된 열은 INSERT 문에서 NULL 값으로 지정하면 자동으로 값이 입력됨

USE cookDB;   
CREATE TABLE testTBL2   
( id int AUTO_INCREMENT PRIMARY KEY,   
 userName char(3),       
 age int     
);   
INSERT INTO testTBL2 VALUES (NULL, '에디', 15);   
INSERT INTO testTBL2 VALUES (NULL, '포비', 12);   
INSERT INTO testTBL2 VALUES (NULL, '통통이', 11);   
SELECT * FROM testTBL2;

>## AUTO_INCREMENT 입력 값을 100부터 시작하도록 변경하고 싶다면
ALTER TABLE testTBL2 AUTO_INCREMENT=100;   
INSERT INTO testTBL2 VALUES (NULL, '패티', 13);   
SELECT * FROM testTBL2;

>##  AUTO_INCREMENT로 증가되는 값을 지정하기 위해서는 서버 변수인 @@auto_increment_ increment 변수 변경(초깃값을 1000으로 하고 증가 값을 3으로 변경하는 구문)
USE cookDB;   
CREATE TABLE testTBL3   
( id int AUTO_INCREMENT PRIMARY KEY,   
 userName char(3),   
 age int   
);   
ALTER TABLE testTBL3 AUTO_INCREMENT=1000;   
SET @@auto_increment_increment=3;   
INSERT INTO testTBL3 VALUES (NULL, '우디', 20);   
INSERT INTO testTBL3 VALUES (NULL, '버즈', 18);   
INSERT INTO testTBL3 VALUES (NULL, '제시', 19);   
SELECT * FROM testTBL3;  

>## 데이터를 삽입할 때 코드를 줄이려면 여러 행을 한꺼번에 입력
INSERT INTO testTBL3 VALUES (NULL, '토이', 17), (NULL, '스토리', 18),   
(NULL, '무비', 19);   
SELECT * FROM testTBL3;

>## 대량 데이터 삽입 형식
INSERT INTO 테이블이름 (열1, 열2, …)   
  SELECT 문;
  
>## employees 테이블의 데이터를 가져와 testTBL4 테이블에 입력
USE cookDB;   
CREATE TABLE testTBL4 (id int, Fname varchar(50), Lname varchar(50));   
INSERT I NTO testTBL4   
 SELECT emp_no, first_name, last_name FROM employees.employees;   
 
>## 아예 테이블 정의까지 생략하고 싶다면 CREATE TABLE … SELECT 문 사용
CREATE TABLE testTBL5   
 (SELECT emp_no, first_name, last_name FROM employees.employees);   
SELECT * FROM testTBL5 LIMIT 3;

>## CREATE TABLE … SELECT 문에서 열 이름을 바꾸어 테이블을 생성하려면
CREATE TABLE testTBL6   
 (SELECT emp_no AS id, first_name AS Fname, last_name AS Lname   
  FROM employees.employees);   
SELECT * FROM testTBL6 LIMIT 3

# UPDATE 문
>## 테이블에 입력되어 있는 값을 수정하는 명령어
UPDATE 테이블이름   
SET 열1=값1, 열2=값2, …   
WHERE 조건;수정하는 명령어   

>## ‘ Kyoichi’의 Lname을 ‘없음’으로 수정
USE cookDB;   
UPDATE testTBL4   
SET Lname = '없음’   
WHERE Fname = 'Kyoichi';

>## 전체 테이블의 내용을 수정하고 싶을 때는 WHERE 절 생략
UPDATE buyTBL   
SET price = price * 1.5;

#  윈도우 함수의 개념
>## 윈도우 함수(window function)
* 테이블의 행과 행 사이 관계를 쉽게 정의하기 위해 MySQL에서 제공하는 함수
* OVER절이 들어간 함수

>##윈도우 함수와 함께 사용되는 집계 함수
AVG( ), COUNT( ), MAX( ), MIN( ), STDDEV( ), SUM( ), VARIANCE( ) 등

>## 윈도우 함수와 함께 사용되는 비집계 함수
CUME_DIST( ), DENSE_RANK( ), FIRST_VALUE( ), LAG( ), LAST_VALUE( ), LEAD( ), NTH_VALUE( ),
NTILE( ), PERCENT_RANK( ), RANK( ), ROW_NUMBER( ) 등

# 순위 함수
* 결과에 순번 또는 순위(등수)를 매기는 함수
* 비집계 함수 중에서 RANK( ), NTILE( ), DENSE_RANK( ), ROW_NUMBER( ) 등이 해당
<순위함수이름>() OVER(   
[PARTITION BY <partition_by_list>]   
ORDER BY <order_by_list>)

>## 키가 큰 순으로 정렬하기
회원 테이블(userTBL)에서 키가 큰 순으로 순위 매기기   
USE cookDB;   
SELECT ROW_NUMBER() OVER(ORDER BY height DESC)   
"키큰순위", userName, addr, height   
  FROM userTBL;
  
>## 키가 같은 경우 이름의 가나다순으로 정렬
SELECT ROW_NUMBER() OVER(ORDER BY height DESC,  
userName ASC) "키큰순위", userName, addr, height   
  FROM userTBL;

>## 각 지역별로 순위 매기기
SELECT addr, ROW_NUMBER() OVER(PARTITION BY addr   
ORDER BY height DESC, userName ASC) "지역 별키큰순위“,   
userName, height   
  FROM userTBL;
  
>## 키가 같은 경우 동일한 등수로 처리
SELECT DENSE_RANK() OVER(ORDER BY height DESC) "키큰순  
위", userName, addr, height  
  FROM userTBL;
  
>##  3등 다음에 4등을 빼고 5등이 나오게 하려면 RANK( ) 함수 사용
SELECT RANK() OVER(ORDER BY height DESC) "키큰순위",   
userName, addr, height   
  FROM userTBL;
  
>## 전체 인원을 키가 큰 순으로 정렬한 후 몇 개의 그룹으로 분할
SELECT NTILE(2) OVER(ORDER BY height DESC) "반번호",   
userName, addr, height   
  FROM userTBL;
  
>## 3개 반으로 분리
SELECT NTILE(4) OVER(ORDER BY height DESC) "반번호",   
userName, addr, height   
  FROM userTBL;
