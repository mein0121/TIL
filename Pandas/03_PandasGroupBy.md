## 정렬
### index명/컬럼명을 순 정렬
- #### sort_index(axis, ascending=True)
    - axis
        - index명 기준 정렬(행) : 'index' 또는 0 (기본값)
        - columnm 명 기준 정렬(열) : 'columns' 또는 1
    - ascending
        - True(기본): 오름차순, False: 내림차순
```
# index가 sorting된 경우 부분값을 가지고 slicing가능. stop은 포함이 안됨.
movie_df2['A':'B'] # A로 시작하는 영화들
movie_df2['A B':'B'] # A B로 시작하는 영화들
```
### 컬럼 값 기준 정렬
- #### sort_values(by, ascending, inplace)
    - by
        - 정렬기준 컬럼명
        - 여러 컬럼에 대해 정렬할 경우 리스트에 담아 전달
    - ascending
        - True(기본): 오름차순, False: 내림차순
        - 여러컬럼에 대해 정렬할 경우 정렬방식도 리스트에 담아 전달
    - inplace
        - False(기본): 변경한 복사본 반환 , True : 원본을 변경
	- 결측치는 방식(오름/내림)에 상관없이 마지막에 나온다.
```
# duration으로 정렬하고 duration값이 같은것 끼리는 imdb_score 값으로 정렬(둘다 오름차순) ascending=False면 내림차순.
movie_df.sort_values(['duration','imdb_score'],ascending=True)[['duration','imdb_score']].head(20)

# 두 조건중 정렬차순을 다르게 할때, duration: False, imdb_score: True
movie_df.sort_values(['duration','imdb_score'],ascending=[False, True])[['duration','imdb_score']].head(20)
# duration이 250이상인 영화를 num_critic_for_reviews로 정렬.
movie_df[movie_df['duration']>250].sort_values('num_critic_for_reviews')
```
## 기술통계함수를 이용한 데이터 집계
### 주요 기술통계 함수

|함수|설명|
|-|-|
|sum()|합계|
|mean()|평균|
|median()|중위수|
|quantile()|분위수|
|std()|표준편차|
|var()|분산|
|count()|결측치를 제외한 원소 개수|
|min()|최소값|
|max()|최대값|
|idxmax()|최대값 index|
|idxmin()|최소값 index|
|unique()|고유값|
|nunique()|고유값의 개수|

- DataFrame에 적용할 경우 컬럼별로 계산
- sum(), max(), min(), unique(), nunique(), count()는 문자열에 적용가능
	- 문자열 컬럼의 min/max index를 조회하려면 np.argmax(), np.argmin()을 사용
- 기본적으로 결측치(NA)는 제외하고 처리한다. 
    - 결측치 제외하지 않으려면 skipna=False로 설정
```
# 문자형빼고 sum()을 하고 싶을때. select_dtypes function 사용, object만 제외.
flights.select_dtypes(exclude='object').sum() 

#mean(), std(), var()은 숫자형 컬럼만 계산.
flights.mean() 

#결측치를 제외하지 않을때
flights.mean(skipna=False)
```
### aggregate(func, axis=0, \*args, \*\*kwargs) 또는 agg(func, axis=0, \*args, \*\*kwargs)
- 판다스가 제공하는 집계 함수들이나 사용자 정의 집계함수를 DataFrame의 열 별로 처리해주는 함수.
- **사용자 정의 집계함수를 사용하거나 열 별로 다른 집계를 할 때 사용한다.**
- 매개변수
    - func 
        - 집계 함수 지정
            - 문자열/문자열리스트 : 집계함수의 이름. 여러 개일 경우 리스트. 판다스 제공 집계함수는 문자열로 함수명만 제공가능
   			```
			DataFrame.aggregate('sum')
			DataFrame.aggregate(['sum','mean','median'])
			DataFrame.agg(['sum','mean','median']).T
			```
			- 딕션어리 : {집계할컬럼 : 집계함수 }
			```
			# airline: 최빈값-mode, DEP_DELAY:평균-mean
			flights.agg({'AIRLINE':'mode','DEP_DELAY':'mean'})
			# airline: 최빈값-mode, DEP_DELAY:평균-mean and 표준편차-std
			# 보고싶은 값이 두개이상일때 리스트로 묶는다. ex)['mean','std']
			flights.agg({'AIRLINE':'mode','DEP_DELAY':['mean','std']})
			# ARR_DELAY,DEP_DELAY 의 min, max
			flights[['ARR_DELAY','DEP_DELAY']].agg(['min','max'])
			```
			- 함수 객체 : 사용자 정의 함수의 경우 함수이름을 전달

    - axis
        - 0 또는 'index' (기본값): 컬럼 별 집계
        - 1 또는 'columns': 행 별 집계
    - \*args, \**kwargs 
        - 함수에 전달할 매개변수. 
        - 집계함수는 첫번째 매개변수로 Series를 받는다. 그 이외의 매개변수가 있는 경우. 

# Groupby
- 특정 열을 기준으로 데이터셋을 묶는다.
- 구문
    - DF.groupby('그룹으로묶을기준컬럼')['집계할 컬럼'].집계함수()
        - 집계할 컬럼은 Fancy Indexing 으로 지정(리스트, 튜플로 전달)
    - 집계함수
        - 기술통계 함수들
        - agg()/aggregate()
            - 여러 다른 집계함수 호출시(여러 집계를 같이 볼경우)
            - 사용자정의 집계함수 호출시
            - 컬럼별로 다른 집계함수들을 호출할 경우
```
# AIRLINE별 각 컬럼의 평균
flights.groupby('AIRLINE').mean()
#AIRLINE별 AIR_TIME의 평균
flights.groupby('AIRLINE')['AIR_TIME'].mean()
#AIRLINE별 여러종류의 평균
flights.groupby('AIRLINE')[['ARR_DELAY','DEP_DELAY','...'....]].mean()
# 두종류이상의 통계함수를 사용할때는 agg함수를 이용.
flights.groupby('AIRLINE')['ARR_DELAY'].agg(['mean','std'])
#컬럼별 다른 통계량을 조회, agg함수에 dictionary를 사용.
flights.groupby('AIRLINE').agg({"DEP_DELAY":['mean','std'],
                               "ARR_DELAY":['min','max'],
                               "AIR_TIME":'count'})
#index 기준으로 groupby
df.groupby(df.index).집계함수							
```
###  복수열 기준 그룹핑
- 두개 이상의 열을 그룹으로 묶을 수 있다. 
- groupby의 매개변수에 그룹으로 묶을 컬럼들의 이름을 리스트로 전달한다.
```
df.groupby(['columnName1','ColumName2'])['조건column'].집계함수
#AIRLINE, MONTH기준
flights.groupby(['AIRLINE','MONTH'])['DEP_DELAY'].mean()
# 다수의 조건과, 다수의 집계함수를 조회하려할때.
flights.groupby(['AIRLINE','MONTH'])[['DEP_DELAY','ARR_DELAY']].agg(['mean','sum'])
```

## Goupby 집계후 집계한 것 중 특정 조건의 항목만 보기
- SQL having 절
- 집계 후 boolean indexing으로 having절 처리


## 사용자 정의 집계함수를 만들어 적용

### 사용자 정의 집계 함수 정의
- 매개변수
    1. Series 또는 DataFrame을 받을 매개변수(필수)
    2. 필요한 값을 받을 매개변수를 선언한다. (선택)

### agg() 를 사용해 사용자 정의 집계 함수 호출
- DataFrame.agg(func=None, axis=0, \*args, \*\*kwargs)
    - axis : 사용자 정의 함수에 전달할 값들(Series)의 축 지정
- Series.agg(func=None, axis=0, \*args, \*\*kwargs)
    - DataFrame의 agg와 매개변수 구조를 맞추기 위해 axis 지정한다. 
- SeriesGroupBy.agg(func=None,  \*args, \*\*kwargs)  
    - axis 지정안함
    - 사용자 함수에 Series를 group 별로 전달한다.
- DataFrameGroupBy.agg(func, \*args, \*\*kwargs)  
    - axis 지정안함.
    - 사용자 함수에 Series를 group 별로 전달한다. 
- \*args, \*\*kwargs는 사용자 정의 함수에 선언한 매개변수가 있을 경우 전달할 값을 전달한다.  