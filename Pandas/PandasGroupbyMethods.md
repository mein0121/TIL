# filter()
- `DataFrameGroupBy.filter(func, dropna=True, *args, **kwargs)`
- 특정 집계 조건을 만족하는 Group의 행들만 조회한다.
    1. DataFrameGroupBy의 group로 DataFrame을 함수에 전달한다.
    2. 함수는 받은 DataFrame을 이용해 집계한 값의 조건을 비교해서 반환한다.(반환타입: Bool) 
    3. 반환값이 True인 Group들의 모든 행들로 구성된 DataFrame을 반환한다.
- 매개변수
    - func: filtering 조건을 구현한 함수
        - 첫번째 매개변수로 Group으로 묶인 DataFrame을 받는다.
    - dropna=True
        - 필터를 통과하지 못한 group의 DataFrame의 값들을 drop시킨다. False로 설정하면 NA 처리해서 반환한다.
    - \*args, \*\*kwargs: filter 함수의 매개변수에 전달할 전달인자값.
	
# transform
함수에 의해 처리된 값(반환값)으로 원래 값들을 변경(tranform) 해서 반환    
DataFrame에 Group 단위 통계량을 추가할 때 유용하다.
- `DataFrameGroupBy.transform(func, *args)`, `SeriesGroupBy..transform(func, *args)`
    - func: 매개변수로 그룹별로 Series를 받아 Series의 값들을 변환하여 (Series로)반환하는 함수객체
        - DataFrameGroupBy은 모든 컬럼의 값들을 group 별 Series로 전달한다.
    - *args: 함수에 전달할 추가 인자값이 있으면 매개변수 순서에 맞게 값을 전달한다. (위치기반 argument)
- transform() 함수를 groupby() 와 사용하면 컬럼의 각 원소들을 자신이 속한 그룹의 통계량으로 변환된 데이터셋을 생성할 수 있다.
- 컬럼의 값과 통계값을 비교해서 보거나 결측치 처리등에 사용할 수있다.

#### 결측치 처리
- transform이용해서 결측치를 같은 그룹의 평균값으로 변환
    - 전체 평균보다 좀더 정확할 수 있다.
```
# 과일별 cnt2의 평균으로 결측치를 대체
cnt2_mean = df4.groupby('fruits')['cnt2'].transform('mean')
df4['cnt2'] = df4['cnt2'].fillna(c2_mean) 
# 표준편차가 크면 과일별로 평균을 대체하기에 전체평균보다 정확하다.
```

# pivot_table()
- 분류별 집계(Group으로 묶어 집계)를 처리하는 함수로 group으로 묶고자 하는 컬럼을 행과 열로 위치시키고 집계값을 값으로 보여준다.    
- 역할은 두개이상의 컬럼을 groupby() 한 집계와 같다.

> pivot() 함수와 역할이 다르다.   
> pivot() 은 index와 column의 형태를 바꾸는 reshape 함수.

- `DataFrame.pivot_table(values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All')`
- **매개변수**
    - values
        - 문자열 또는 리스트. 집계할 대상 컬럼들
    - index
        - 문자열 또는 리스트. index로 올 컬럼들 => groupby였으면 묶었을 컬럼
    - columns
        - 문자열 또는 리스트. column으로 올 컬럼들 => groupby였으면 묶었을 컬럼
    - aggfunc
        - 집계함수 지정. 함수, 함수이름문자열, 함수리스트(함수이름 문자열/함수객체), dict: 집계할 함수
        - 기본(생략시): 평균을 구한다. (mean이 기본값)
    - fill_value, dropna
        - fill_value: 집계시 NA가 나올경우 채울 값
        - dropna: boolean. 컬럼의 전체값이 NA인 경우 그 컬럼 제거(기본: True)
    - margins/margins_name
        - margin: boolean(기본: False). 총집계결과를 만들지 여부.
        - margin_name: margin의 이름 문자열로 지정 (생략시 All)
	```
	ex)pivot_table
	# index기준
	flights.pivot_table(values='AIR_TIME',index='AIRLINE',aggfunc='mean')
	# column기준
	flights.pivot_table(values='AIR_TIME',columns='AIRLINE',aggfunc='mean')
	#총평균 추가
	flights.pivot_table(values='AIR_TIME',index='AIRLINE',aggfunc='mean',
						margins=True) 
	#총평균 추가 및 이름지정
	flights.pivot_table(values='AIR_TIME',index='AIRLINE',aggfunc='mean',
						margins=True, margins_name='total')
	# 한개이상의 집계함수 이용시 리스트로 묶음.
	flights.pivot_table(values='AIR_TIME',index='AIRLINE',aggfunc=['mean','count','sum'],
						margins=True, margins_name='total')
	# NaN(결측치)를 -1 로 대체
	flights.pivot_table(values='AIR_TIME',index='AIRLINE',aggfunc=['mean','count','sum'],
						fill_value=-1,margins=True, margins_name='total')
	# 3개이상의 컬럼을 그룹핑할때. AIRLINE,ORG_AIR,MONTH
	flights.pivot_table(values='CANCELLED',
                    index=['AIRLINE','ORG_AIR'],
                    columns='MONTH',
                    aggfunc='sum',
                    margins=True,
                    margins_name='Total_Sum')
	```
	
# apply() - Series, DataFrame의 데이터 일괄 처리
- 데이터프레임의 행들과 열들 또는 Series의 원소들에 공통된 처리를 할 때 apply 함수를 이용하면 반복문을 사용하지 않고 일괄 처리가 가능하다.
- DataFrame.apply(함수, axis=0, args=())
    - 인수로 행이나 열을 받는 함수를 apply 메서드의 인수로 넣으면 데이터프레임의 행이나 열들을 하나씩 함수에 전달한다.
    - 매개변수
        - 함수: DataFrame의 행들 또는 열들을 전달할 함수
        - axis: **0-행을 전달, 1-열을 전달 (기본값 0)** G: 0이 행이다...
        - args: 행/열 이외에 전달할 매개변수를 위치기반(순서대로) 튜플로 전달
- Series.apply(함수, args=())
    - 인수로 Series의 원소들을 받는 함수를 apply 메소드의 인수로 넣으면  Series의 원소들을 하나씩 함수로 전달한다.
    - 매개변수
        - 함수: Series의 원소들을 전달할 함수
        - args: 원소 이외에 전달할 매개변수를 위치기반(순서대로) 튜플로 전달
		
# cut()/qcut() - 연속형(실수)을 범주형으로 변환
- cut() : 지정한 값을 기준으로 구간을 나눠 그룹으로 묶는다.
    - `pd.cut(x, bins,right=True, labels=None)`
    - 매개변수
        - x: 나눌 대상. 1차원 배열형태의 자료구조(Series)
        - bins: 나누는 기준값(구간경계)을 리스트로 전달
        - right: 구간경계의 오른쪽(True)을 포함할지 왼쪽(False)을 포함할지
        - labels: 각 구간의 label을 리스트로 전달
```
df = pd.cut(func, bins=[구간경계],right=True, labels=['이름',...])
```
- qcut() : 데이터를 오름차순으로 정렬한 뒤 데이터 개수가 같도록 지정한 개수만큼의 구간으로 나눈다.
    - `pd.qcut(x, q, labels)`
    - 매개변수
        - x: 나눌 대상. 1차원 배열형태의 자료구조
        - q: 나눌 개수

