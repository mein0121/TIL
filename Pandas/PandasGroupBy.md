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

