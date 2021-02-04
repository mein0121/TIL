### Sub Query
####서브쿼리(Sub Query)
- 쿼리안에서(insert, delete, update, select)안에 select 쿼리를 사용하는 것.
– 메인 쿼리 : 실제 조회하고자 하는 쿼리
– 서브 쿼리 : 메인 쿼리에서 사용할 데이터를 조회하기 위한 쿼리

- 서브쿼리가 사용되는 구
	- select절, from절, where절, having절
 
- 서브쿼리의 종류
	- 어느 구절에 사용되었는지에 따른 구분
		- 스칼라 서브쿼리 - select 절에 사용. 반드시 서브쿼리 결과가 1행 1열(값 하나-스칼라) 0행이 조회되면 null을 반환
		- 인라인 뷰 - from 절에 사용되어 테이블의 역할을 한다.
- 서브쿼리 조회결과 행수에 따른 구분
	- 단일행 서브쿼리 - 서브쿼리의 조회결과 행이 한행인 것.
    - 다중행 서브쿼리 - 서브쿼리의 조회결과 행이 여러행인 것.
		- 연산자 종류
			- in
			- 비교연산자 any : 조회된 값들 중 하나만 참이면 참 (where 컬럼 > any(서브쿼리) )
			- 비교연산자 all : 조회된 값들 모두와 참이면 참 (where 컬럼 > all(서브쿼리) )
			```SQL
			ex)
			-- job_id가 'SA_REP' 인 직원들중 가장 적은 급여를 받는 직원보다 많은 급여를 받는 직원을 조회
			where salary > (select min(salary) from emp where job_id in 'SA_REP');
			where salary > any(select salary from emp where job_id in 'SA_REP');
			
			-- job_id가 'SA_REP' 인 직원들중 가장 많은 급여를 받는 직원보다 많은 급여를 받는 직원을 조회
			where salary > (select max(salary) from emp where job_id in 'SA_REP');
			where salary > all(select salary from emp where job_id in 'SA_REP');
			```
- 동작 방식에 따른 구분
    - 비상관(비연관) 서브쿼리 - 서브쿼리에 메인쿼리의 컬럼이 사용되지 않는다. 메인쿼리에 사용할 값을 서브쿼리가 제공하는 역할을 한다.
		- 실행 순서: subquery실행 -> subquery 실행결과를 가지고 mainquery 실행.
    - 상관(연관) 서브쿼리 - 서브쿼리에서 메인쿼리의 컬럼을 사용한다.
		- 실행 순서: mainquery실행 -> mainquery 실행결과를 바탕으로 subquery 조건절을 비교.
		- 연산자
			- EXISTS, NOT EXISTS
		
		
		
		