## DDL, DML
### DML - 데이터(값)을 다루는 SQL문
- INSERT, SELECT(DQL), UPDATE, DELETE : CRUD
	- ROLLBACK: INSERT/UPDATE/DELETE 하기 전 상태로 복구하는 명령어.
	- COMMIT: 변경내용을 저장, COMMIT전이면 ROLLBACK으로 데이터 복구(복원)가능.
- INSERT 문 - 행 추가
	- 구문
		- INSERT INTO 테이블명 (컬럼 [, 컬럼]) VALUES (값 [, 값[])
		- 조회결과를 INSERT 하기 (subquery 이용)
			- INSERT INTO 테이블명 (컬럼 [, 컬럼])  SELECT 구문
		- INSERT할 컬럼과 조회한(subquery) 컬럼의 개수와 타입이 맞아야 한다.
		```SQL
		-- 모든 값을 넣을 경우 컬럼지정 생략가능, NULLABLE 컬럼은 NULL로 입력 가능.
		INSERT INTO EMP VALUES(1000, '홍길동', 'FI_ACCOUNT', NULL, '2020/01/02', 7000, NULL, 10);
		-- 컬럼지정해서 입력시 NULLABLE 컬럼 생략가능.
		INSERT INTO EMP(EMP_ID, EMP_NAME, HIRE_DATE, SALARY) VALUES (1100, '이순신', '2000/01/05', 6000);
		-- DATE 타입 값을 변환
		INSERT INTO EMP VALUES(1200, '강길동', 'FI_ACCOUNT', NULL,TO_DATE('2020/01', 'YYYY/MM'), 7000, NULL, 10);
		```
- UPDATE 문 - 데이터를 수정
	- 구문
	```SQL
	UPDATE 테이블명 -- UPDATE할 테이블명
	SET    변경할 컬럼 = 변경할 값  [, 변경할 컬럼 = 변경할 값] -- 변경할 컬럼과 값 지정
	[WHERE 제약조건] -- 변경할 행(변경할 조건) 선택, 생략 가능
	```
- DELETE : 테이블의 행을 삭제
	- 구문 
		- DELETE FROM 테이블명 [WHERE 제약조건]
		- WHERE: 삭제할 행을 선택

- **특정행의 컬럼값을 없애거나 변경할때는 UPDATE. DELETE는 행 자체를 삭제.**

### DDL
- DDL = DATABASE에 객체(테이블, 유저, 시퀀스 등)를 관리한다.
	- 생성(CREATE), 수정(ALTER), 삭제(DROP)
	- CREATE
	```SQL
	CREATE TABLE PARENT_TB(
    NO NUMBER CONSTRAINT PK_PARENT_TB PRIMARY KEY, -- 컬럼 이름을 직점 지정 
    -- NO NUMBER PRIMARY KEY -- 컬럼 이름을 디폴트값으로 지정
    NAME NVARCHAR2(30) NOT  NULL,
    BIRTHDAY DATE DEFAULT SYSDATE, -- 기본값 설정: INSERT시 값을 넣지 않으면 기본값(SYSDATE)으로.
    EMAIL VARCHAR2(100) CONSTRAINT UK_PARENT_TB_EMAIL UNIQUE, -- 유니크키(중복된 값이 들어갈수없다, null제외) 설정시 컬럼이름 지정.
    -- EMAIL VARCHAR2(100) UNIQUE, -- 컬럼이름을 디폴트값으로 지정.
    GENDER CHAR(1) NOT NULL CONSTRAINT CK_PARENT_TB_GENDER CHECK(GENDER IN ('M','F'))
    -- CHECK 제약조건: 특정 값만 가질수있도록 설정.
	)
	```