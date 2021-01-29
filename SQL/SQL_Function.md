# SQL Function(SQL 함수)

### 단일행 함수
- 그룹,집계 함수.
- 행단위로 처리.
- data type 함수 (문자열,숫자date)
- 타입 변환 함수
	- 문자열을 Number로 변환: to_number(expr[,fmt])
	- Number에서 문자열로 변환 : to_char(expr[,fmt])
	- Date에서 문자열로 변환: to_char(expr[,fmt])
	- 문자열에서 date로 변환: to_date(expr[,fmt])
	- Number에서 Date타입으로 변환하기 위해서는 Number를 Character로 변환후 Date로 변환.
```SQL
Examples
SELECT '1,000.53', TO_NUMBER('1,000.53','9,999.99')
FROM DUAL;

SELECT TO_CHAR(123456789, '999,999,999'),
TO_CHAR(123456789, 'FM999,999,999'), --FM : 최대자리수가 남을경우 공백제거. 
TO_CHAR(123456789, '999,999'), -- 최대자리수가 모자랄경우 ##으로 표시, 자리수를 맞춰야함.
FROM DUAL;

-- 문자열을 DATE타입으로 변환후 일자 덧뺄셈 가능.
select to_date('2020/01/21', 'yyyy/mm/dd')-5, to_date('2020/01', 'yyyy/mm')-5
from dual;

-- 문자열을 date 타입으로 변환후 다시 원하는 형식의 문자열로 반환
SELECT TO_CHAR(TO_DATE('20100107232215','YYYYMMDDHH24MISS'), 'YYYY"년" MM"월" DD"일" HH24"시" MI"분" SS"초"')
FROM DUAL;
```
### Functions
- 함수 - 문자열관련 함수
```
	UPPER()/ LOWER() : 대문자/소문자 로 변환
	INITCAP(): 단어 첫글자만 대문자 나머진 소문자로 변환
	LENGTH() : 글자수 조회
	LPAD(값, 크기, 채울값) : "값"을 지정한 "크기"의 고정길이 문자열로 만들고 모자라는 것은 왼쪽부터 "채울값"으로 채운다.
	RPAD(값, 크기, 채울값) : "값"을 지정한 "크기"의 고정길이 문자열로 만들고 모자라는 것은 오른쪽부터 "채울값"으로 채운다.
	SUBSTR(값, 시작index, 글자수) - "값"에서 "시작index"번째 글자부터 지정한 "글자수" 만큼의 문자열을 추출. 글자수 생략시 끝까지. 
	REPLACE(값, 찾을문자열, 변경할문자열) - "값"에서 "찾을문자열"을 "변경할문자열"로 바꾼다.
	LTRIM(값): 왼공백 제거
	RTRIM(값): 오른공백 제거
	TRIM(값): 양쪽 공백 제거
``` 
- 함수 - 숫자관련 함수
```
	round(값, 자릿수) : 자릿수이하에서 반올림 (양수 - 실수부, 음수 - 정수부, 기본값 : 0)
	select round(1.2345,3), -- 결과: 1.235
	round(1.52345,0), -- 결과: 2
	from dual;
	
	trunc(값, 자릿수) : 자릿수이하에서 절삭(양수 - 실수부, 음수 - 정수부, 기본값: 0)
	SELECT trunc(1.52345,0), -- 결과: 1
	trunc(1.5332, 2), -- 결과: 1.53
	trunc(155,-2) -- 결과: 100
	from dual;
	
	ceil(값) : 올림
	floor(값) : 내림
	mod(나뉘는수, 나누는수) : 나눗셈의 나머지 연산
	select ceil(15.67), -- 결과: 16
	floor(15.67), -- 결과: 15
	mod(10,3) -- 결과: 1
	from dual;
``` 
- 함수 - 날짜관련 계산 및 함수
```
	sysdate: 실행시점의 일시
	Date +- 정수 : 날짜 계산.
	months_between(d1, d2) -경과한 개월수(d1이 최근, d2가 과거)
	add_months(d1, 정수) - 정수개월 지난 날짜. 마지막 날짜의 1개월 후는 달의 마지막 날이 된다. 
	next_day(d1, '요일') - d1에서 첫번째 지정한 요일의 날짜. 요일은 한글(locale)로 지정한다.
	last_day(d) - d 달의 마지막날.
	extract(year|month|day from date) - date에서 year/month/day만 추출
``` 
### 기타 함수
- NVL(expr, 기본값) : expr값이 null이면 기본값을 null이 아니면 expr값을 반환.
- NVL2(expr, nn, null) - expr이 null이 아니면 nn, 널이면 세번째
- nullif(ex1, ex2) 둘이 같으면 null, 다르면 ex1

```SQL
SELECT NVL(null,0), -- 0반환
nvl(20,1), -- 20반환
nvl2(1, 'null아님','null임') --null아님 을 반환
nvl2(null, 'null아님','null임') --null임 을 반환.
from dual;

select emp_id, emp_name, salary, 
nvl(comm_pct,0) as comm_pct, --comm_pct가 null이면 0을 반환.

-- comm_pct가 null이 아니면 두번째 조건문 반환, null이면 세번째 반환.
nvl2(comm_pct, '커미션있음','커미션없음') as comm_pct2 
from emp;
```

### Decode함수와 Case문
- If else문을 함수화시킨것이라 생각할 수 있음.
	- 만약 if구문이 아래와 같다면.
	```python
	if dept_name == "Shipping":
		return "배송"
	elif dept_name == "Sales":
		return "영업"
	......
	elif dept_name == null:
		return "부서없음"
	else:
		return dept_name
	```	
- decode(컬럼, [비교값, 출력값, ...] , else출력) 
```SQL
select decode(dept_name, 'Shipping', '배송', 
                         'Sales', '영업', 
                         ...., 
                         null,'부서없음', 
                         dept_name) as dept, dept_name
from emp;
```	
- case문 동등비교
- case 컬럼 [when 비교값 then 출력값]
-          [else 출력값] end         
- case문 조건문
- case [when 조건 then 출력값]
-      [else 출력값] end
```SQL
select case dept_name when 'Shipping' then '배송' 
                      when 'Sales' then '영업'
					  .....
					  else dept_name end as dept, dept_name					  

select case when dept_name is null then '미배정' --Null일때는 '미배정'그 외는 원래이름.
            else dept_name end as dept, dept_name
```

- decode와 case를 이용해 원하는 조건을 먼저 정렬할때
- 원하는 조건 = 'ST_CLERK', 'IT_PROG', 'PU_CLERK', 'SA_MAN' 이후 나머지업무 정렬. 
```SQL
-- 먼저 나오길 원하는 column명을 지정후 출력.
decode문을 이용시:
order by decode(job, 'ST_CLERK', '1', 
                   'IT_PROG','2', 
                   'PU_CLERK', '3',
                   'SA_MAN','4', job);
-- case문을 이용:
order by case job when 'ST_CLERK' then '1' 
                  when 'IT_PROG' then '2' 
                  when 'PU_CLERK' then '3'
                  when 'SA_MAN' then'4'
                  else job end;				   
```
### 집계함수, 그룹함수, 다중행함수
- 집계함수
	- sum(): 전체합계
	- avg(): 평균
	- min(): 최소값
	- max(): 최대값
	- stddev(): 표준편차
	- variance(): 분산
	- count(): 개수
	- sum, avg, stddev, variance: number 타입에만 사용가능.
	- min, max, count :  모든 타입에 다 사용가능.
	- count(*)함수를 제외한 모든 집계함수는 null은 빼고 계산한다.

- Group by
	- 특정 컬럼(들)의 값별로 나눠 집계할 때 나누는 기준컬럼을 지정하는 구문.
	- 구문: group by 컬럼명 [, 컬럼명]
	- select의 where 절 다음에 기술한다.
	- select 절에는 **group by 에서 선언한 컬럼들만** 집계함수와 같이 올 수 있다
```SQL
select dept_name, job, count(job) as 직원수
from emp
where dept_name in ('Sales', 'Purchasing')
group by dept_name,job;
```
	- case함수를 사용한후 case문으로 출력 할 경우 select절에 같은 함수를 넣어야한다. 
```SQL
select case when salary >= 10000 then '10000 이상'
            else '10000 미만' end as sal, count(*)
from emp
group by case when salary >= 10000 then '10000 이상' 
              else '10000 미만' end;
```
- Having
	- 집계결과에 대한 행 제약 조건.
	- group by 다음 order by 전에 온다.
	- 구문: having 제약조건 
	- 집계함수를 이용한 제약조건.  
	```SQL
	-- 10명이상인 부서와 부서인원 조회.
	SELECT DEPT_NAME, COUNT(*)
	FROM EMP
	GROUP BY dept_name
	HAVING COUNT(*)>=10;
	
	-- 평균급여5천이상, 총급여5만이상인 부서와 평균,총급여를 조회.
	SELECT DEPT_NAME, ROUND(AVG(SALARY),2), SUM(SALARY)
	FROM EMP
	GROUP BY DEPT_NAME
	HAVING AVG(SALARY)>=5000 AND SUM(SALARY)>=50000;
	```
	







