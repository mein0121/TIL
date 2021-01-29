# SQL Function(SQL 함수)

### 단일행 함수
- 그룹,집계 함수.
- 행단위로 처리.
- data type 함수 (문자열,숫자date)
- 타입 변환 함수
	- 문자열을 Number로 변환: to_number()
	- Number에서 문자열로 변환 : to_char()
	- Date에서 문자열로 변환: to_char()
	- 문자열에서 date로 변환: to_date()
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
```SQL
함수 - 문자열관련 함수
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
 
함수 - 숫자관련 함수
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
 
함수 - 날짜관련 계산 및 함수
	sysdate: 실행시점의 일시
	Date +- 정수 : 날짜 계산.
	months_between(d1, d2) -경과한 개월수(d1이 최근, d2가 과거)
	add_months(d1, 정수) - 정수개월 지난 날짜. 마지막 날짜의 1개월 후는 달의 마지막 날이 된다. 
	next_day(d1, '요일') - d1에서 첫번째 지정한 요일의 날짜. 요일은 한글(locale)로 지정한다.
	last_day(d) - d 달의 마지막날.
	extract(year|month|day from date) - date에서 year/month/day만 추출
``` 