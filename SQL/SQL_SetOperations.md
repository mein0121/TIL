### SQL Set Operations
#### 집합 연산자 (결합 쿼리)
- 둘 이상의 select 결과를 가지고 하는 연산.
- 구문
	- select문  집합연산자 select문 [집합연산자 select문 ...] [order by 정렬컬럼 정렬방식]

-연산자
	- UNION: 두 select 결과를 하나로 결합한다. 단 중복되는 행은 제거한다. (합집합)
	- UNION ALL : 두 select 결과를 하나로 결합한다. 중복되는 행을 포함한다. (합집합)
	- INTERSECT: 두 select 결과의 동일한 결과행만 결합한다. (교집합)
	- MINUS: 왼쪽 조회결과에서 오른쪽 조회결과에 없는 행만 결합한다. (차집합)
	   
- 규칙
	- 연산대상 select 문의 컬럼 수가 같아야 한다. 
	- 연산대상 select 문의 컬럼의 타입이 같아야 한다.
	- 연산 결과의 컬럼이름은 첫번째 왼쪽 select문의 것을 따른다.
	- order by 절은 구문의 마지막에 넣을 수 있다.
	- UNION ALL을 제외한 나머지 연산은 중복되는 행은 제거한다.