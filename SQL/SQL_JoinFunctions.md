## SQL JOIN
- 테이블 간의 관계성 이해 필요

### Foreign key
- 관계형 데이터베이스에서 외래 키(외부 키, Foreign Key)는 한 테이블의 필드(attribute) 중 다른 테이블의 행(row)을 식별할 수 있는 키
- 한 테이블이 다른 테이블의 컬럼 값을 참조하는 것.
- 부모테이블의 primary key를 자식테이블의 foreign key로 생성하여 부모테이블 참조
- 참조되는 테이블의 Primary key를 참조하는 테이블에 Foreign key 컬럼으로 생성하여 참조.
- 관계성 mapping, 다중성 표시
	- **one to one 
		- left:1, right: 1
		- left:1, right: 0..1 or?
	- **many to one
		- left: 1..\* or +	right: 1
	- **one to many
		- left: 1	right: 1..\* or +
	- **many to many
		- left: 0..\* or \*	right: 0..\* or \*
		- leftL 1..\* or +	right: 1..\* or +
	- **FOREIGN KEY가 참조하는 테이블에서 Primary key로 사용 되면 직선을 사용. 그 외는 점선을 사용.
	
- Foreign key 제약조건
	- 기본 구문
		- **CONSTRAINT** 제약조건이름 **FOREIGN KEY**(컬럼) **REFERECES** 부모테이블(PK컬럼)
	- 자식 테이블로 부터 참조당하는 부모테이블의 row는 삭제 할 수 없다.
	- CASCADE DELETE
		- FOREIGN KEY 설정 시 ON DELETE CASCADE 설정
			- 부모 테이블의 참조 ROW 삭제 시 자식 테이블의 참조 ROW 삭제
			```SQL
			CONSTRAINT 제약조건이름 FOREIGN KEY(컬럼) REFERECES 부모테이블(PK컬럼) ON DELETE CASCADE		
			```
		– FOREIGN KEY 설정 시 ON DELETE SET NULL설정
			- 부모 테이블의 참조 ROW 삭제 시 참조 하는 자식 테이블의 컬럼 값을 NULL로 설정
			- 자식 테이블의 컬럼의 제약조건이 not null이 아닌 경우에만 가능.
			```SQL
			CONSTRAINT 제약조건이름 FOREIGN KEY(컬럼) REFERECES 부모테이블(PK컬럼) ON DELETE SET NULL
			```
	- Table drop시 외래키 제약조건 제거
		- 부모 테이블 DROP시 그 테이블과 연결된 모든 Foreign key 제약조건 제거
		```
		DROP TABLE 테이블이름 CASCADE CONSTRAINT
		```


### 조인이란
– SELECT 할때 두 개이상의 테이블을 합쳐 하나의 행으로 조회하기 위한 것.
	- 부모테이블의 PK와 자식테이블의 FK
	- 각 테이블을 어떻게 합칠지를 표현하는 것을 조인 연산이라고 한다.
    - 조인 연산에 따른 조인종류 : Equi join , non-equi join
	- From 절에서 Join 관계을 지정 할 수 있다.
- Inner Join, Outer Join, Cross Join
    - Inner Join 
        - 양쪽 테이블에서 조인 조건을 만족하는 행들만 합친다. 
	- ANSI Inner Join 구문
	```SQL
	FROM  테이블a INNER JOIN 테이블b ON 조인조건 -- INNER 생략가능.
	 
	-- EMP 테이블과 DEPT 테이블 두개의 테이블을 조인.
	EX)
	SELECT e.emp_id, e.salary, d.dept_name, d.loc 
	FROM emp e join dept d on e.dept_id = d.dept_id -- 두 테이블을 조인후 on절에 조인 조건 선언.
	WHERE e.salary between 10000 and 29999; -- Salary가 1만이상 3만미만인 직원만조회.
	
	-- EMP 테이블, DEPT 테이블, JOB 테이블 세개의 테이블을 조인. 세개의 테이블 조인시 테이블간의 관계성에 맞춰서 선언해야한다.
	-- DEPT와 JOB간의 관계성이 없고 EMP만 JOB과 DEPT테이블과 관계성이 있을때.
	EX)
	SELECT e.emp_id, e.salary, j.job_title, d.dept_name
	FROM emp e join dept d on e.dept_id = d.dept_id -- EMP와 DEPT테이블 조인과 on절에 조건 선언후
			   join job j on e.job_id = j.job_id    -- 세번째 테이블의 조인과 on절에 조건 선언.
	WHERE e.emp_id = 200;
	```
		- Self Join
			- 물리적으로 하나의 테이블을 두개의 테이블처럼 조인하는 것.
		```SQL
		Table의 alias를 다르게 지정
		--직원의 ID(emp.emp_id), 이름(emp.emp_name), 상사이름(emp.emp_name)을 조회
		select e.emp_id, e.emp_name,
			   b.emp_name -- 상사이름
		from emp e, emp b -- 테이블의 별칭을 e, b로 
		where e.mgr_id = b.emp_id -- table e의 mgr_id와 table b의 emp_id를 비교
		order by e.mgr_id;
		```
    - Outer Join
        - 한쪽 테이블의 행들을 모두 사용하고 다른 쪽 테이블은 조인 조건을 만족하는 행만 합친다. 조인조건을 만족하는 행이 없는 경우 NULL을 합친다.
        - 종류 : Left Outer Join,  Right Outer Join, Full Outer Join
    - Cross Join
        - 두 테이블의 곱집합을 반환한다.







