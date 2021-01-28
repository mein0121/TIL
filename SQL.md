### SQL 연산 우선순위 (Operator precedence and order of evaluation)
1: ( )  Parenthetic expression
2: - Unary negative, not, Boolean test
3: *, /, **
4: +, -
5: <, <=, >,>=, =, <>
6: AND, OR
```SQL
and with or 절에서는 and 를 먼저 연산
ex) a or b and c
or를 먼저 연산하려면. ()사용
ex) (a or b) and c

SELECT emp_id, emp_name, job, dept_name
FROM emp
where job like '%MAN%' AND dept_name in ('Marketing', 'Sales');

SELECT emp_id, emp_name, job, dept_name
FROM emp
where job like '%MAN%' AND (dept_name = 'Marketing' or dept_name = 'Sales');
```

### Order by를 이용한 정렬
- select문에 가장 마지막에 오는 구절.
- order by 정렬기준컬럼 정렬방식 [, 정렬기준컬럼 정렬방식,...]
- 정렬기준컬럼
	- 컬럼이름.
	- select절에 선언된 순서.
	- 별칭이 있을 경우 별칭.
- 정렬방식
	- asc : 오름차순 (기본-생략가능)
	- desc : 내림차순
- 문자열: 숫자 < 대문자 < 소문자 < 한글
- date : 과거 < 미래	

NULL 값
ASC : 마지막.  order by 컬럼명 asc nulls first
DESC : 처음.   order by 컬럼명 desc nulls last
-- nulls first, nulls last ==> 오라클 문법.
```SQL
Examples:
select  emp_id, emp_name, job, salary
from    emp
order by job asc,salary desc; -- job오름차순 정렬후, salary로 내림차순 정렬.

select  emp_id, emp_name, job, salary
from    emp
order by 3, 4 desc; --select 절의 3번째(job), 4번째(salary) 컬럼기준 정렬. ***테입블의 컬럼순서가 아니라 select절에 선언한 컬럼순서

select  emp_id "직원ID",emp_name "이름",job "업무",salary "급여"
from    emp
order by "업무", 급여 desc; --컬럼의 별칭을 사용할 수 있다.
--order by 3, 4 desc;
--order by job, salary desc;
```



