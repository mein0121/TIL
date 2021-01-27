# SQL Basic(Structured Query Language)
### Database
- 지속적으로 유지, 관리되어야하는 데이터들의 집합.
- CRUD
	- Create, Read(Retrive), Update, Delete
	- 기본적인 데이터 처리 기능(저장,조회,수정,삭제)
### Relational Database (관계형 데이터 베이스)
- 행(Row)과 열(column)로 이루어진 2차원 표 형식으로 data를 관리하는 데이터 베이스.
### 기본 용어
- 테이블 (Table)
	- 데이터 베이스에서 데이터를 저장하는 단위.
	- Entity : 시스템이 독립적으로 관리하길 원하는 데이터.
	- Table : Entity를 물리적 데이터 베이스로 표현하는 방식.
- 열(Column)
	- 테이블에서 데이터를 구성하는 속성(attribute), 또는 세로로 묶은 데이터셋.
- 행(Row)
	- 테이블이 관리하는 하나의 데이터셋.

### SQL
- DML(Data Manipulation Language) – INSERT, UPDATE, DELETE, SELECT
	- Manipulate data in a table
- DDL(Data Definition Language) – CREATE, ALTER, DROP, TRUNCATE
- DCL(Data Control Language) – GRANT, REVOKE # 권한 부여 및 취소.
- TCL(Transaction Control Language) – COMMIT, ROLLBACK, SAVEPOINT

### DDL(Data Definition Language)
- Create 
```
사용자 계정 생성
CREATE USER username IDENTIFIED BY Password;
테이블 생성
CREATE TABLE [TableName](
	ColumnName dataType [Constraints],
	ColumnName dataType [Constraints],
	....);
ex)
CREATE TABLE member(
    id varchar2(10) PRIMARY KEY,
    password varchar2(10) NOT NULL,
    name NVARCHAR2(50) NOT NULL,
    point NUMBER(7),
    join_date DATE NOT NULL    
	);
```
- DROP
```
사용자 계정 삭제
DROP USER username CASCADE;
테이블 삭제
DROP TABLE TableName CASCADE CONSTRAINTS;
```
- ALTER
	- 컬럼 및 제약조건 추가 
```
Add Column
ALTER TABLE TableName ADD (ColumnName dataType Constraints)

Add CONSTRAINTS
ALTER TABLE TableName ADD CONSTRAINTS 제약조건구문

Change column type
ALTER TABLE TableName MODIFY (ColumnName dataType Constraints)
```

### DML(Data Manipulation Language)
- INSERT (데이터 삽입)
	```
	INSERT INTO TableName (ColumnName1, ColumnName2,...) VALUES(VALUE1, VALUE2,...);
	ex) INSERT INTO member(id, password, name, point, join_date) VALUES ('id-1','abcde','Hong',10000,'2020/01/26');
	```
	- Nullable한 속성값은 생략가능. 모든 컬럼에 값을 다 넣을 경우 컬럼명은 생략 가능.	
- UPDATE
	```
	UPDATE 테이블이름
	SET 컬럼=변경할값 [, 컬럼=변경할값]
	[WHERE 제약조건]
	```
- DELETE
	```
	DELETE FROM 테이블이름 [WHERE 제약조건]
	```
- SELECT
	```
	SELECT 조회컬럼 [as 별칭][, 조회컬럼,...]
	FROM 테이블이름 [as 별칭]
	[WHERE 제약조건]
	[GROUP BY 그룹화할 기준컬럼]
	[HAVING 조건]
	[ORDER BY 정렬기준컬럼 [ASC | DESC]]
	```
	- FROM
		- dual: 더미(dummy) 테이블 - select의 from절을 만들기 위해서 사용하는 가짜 테이블
	- WHERE
	```
	사용하는 검색 조건의 주요 연산자: 
	AND, OR
	
	=, <> (!=), >, <, >=, <=
	
	BETWEEN a AND b:  a와b사이의 데이터를 조회(a, b값 포함)
	NOT BETWEEN a AND b: a와b사이에 있지 않은 데이터를 조회
	
	LIKE: 문자 형태로 일치하는 데이터를 조회 (%, _사용)
	NOT LIKE 문자 형태와 일치하지 않는 데이터를 조회
	
	IN (list): list의 값 중 어느 하나와 일치하는 데이터를 조회
	NOT IN (list): list의 값과 일치하지 않는 데이터를 조회
	
	IS NULL: NULL값을 가진 데이터를 조회
	IS NOT NULL: NULL값을 갖지 않는 데이터를 조회
	연산의 우선순위를 바꿀 경우 ( ) 를 사용한다.
	```
- 산술연산 : + - * /
	- 피연산자 중 null이 있으면 결과는 무조건 null. null + 10 = null
	- null: 값이 없다. 또는 모르는 값이다.
    - data타입값 +/-정수: day(일)을 +/-. 오늘날짜 +/-5: 5일후/전 날짜
- 연결연산자 : 문자열을 붙이는 연산자. || => 모든타입 가능.










