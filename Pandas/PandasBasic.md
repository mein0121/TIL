# Pandas 개요
- 데이터 분석과 관련된 다양한 기능을 제공하는 파이썬 패키지
    - 데이터 셋을 이용한 다양한 통계 처리 기능을 제공한다.
    - 표 형태의 데이터를 다루는데 특화된 파이썬 모듈. (엑셀과 비슷)
    - 표 형태의 데이터를 다루기 위한 **시리즈(Series)**와 **데이터프레임(DataFrame)** 클래스 제공
        - Series : 1차원 자료구조를 표현 (행 또는 열)
        - DataFrame : 행렬의 표를 표현 (테이블, 행렬)
	- numpy: 1차원 배열이 모여서 2차원 배열
	- sql: column과 row가 모여서 table
	- Pandas: series가 모여서 dataframe.

## Series 개요
- 1차원 자료구조
- DataFrame(표)의 한 행이나 한 열을 표현한다.
- 각 원소는 index로 접근할 수 있다.
    - index는 순번과 지정한 이름 두가지로 구성된다.
        - index명을 명시적으로 지정하지 않으면 순번이 index명이 된다.
    - 순번은 0부터 1씩 증가하는 정수.  
- 벡터화 연산을 지원
    - Series 객체에 연산을 하면 각각의 Series 원소들에 연산이 된다.
- Series를 구성하는 원소들을 다루는 다양한 메소드 제공  

## Series생성
- 구문
    - `Series(배열형태 자료구조)`
> #### 배열형태 자료구조    
> - 리스트
> - 튜플
> - 넘파이 배열(ndarray)

- Series.shape: 시리즈의 형태 조회
- Series.ndim: 시리즈의 차원 (1)
- Series.dtype: 데이터 타입 조회
- Series.size: 원소의 총개수
```python
ex)
# 리스트형태의 배열
nums = [10,20,30,40,50,60,70,80]
s1 = pd.Series(nums)
#넘파이 배열
n = np.random.normal(10,5,size=10)
s2 = pd.Series(n)
# index명을 지정
s3 = pd.Series([70, 100, 90, 80], index=['국어', '영어', '수학', '과학'])
```
### Indexing
- Series index명은 중복가능
	- 중복된 index명 조회시 전체 조회.
- **index 순번으로 조회**
    - Series[순번]
    - Series.iloc[순번]
	- 음수 index로 조회시
		- index명 지정시: indexer([음수])로 조회가능
		```
		Series[음수]
		```
		- index명 미지정시: iloc indexer를 사용.
		```
		Series.iloc[음수]
		```
- **index 이름으로 조회**
    - Series[index명]
    - Series.loc[index명]
    - Series.index명
        - index명이 문자열일 경우 `. 표기법` 사용가능
		- index명이 숫자로 시작하는 경우, 변수이름에 줄 수 없는 글자가 들어간 경우에는 사용불가
    - index명이 문자열이면 문자열(" ") 로, 정수이면 정수로 호출
		```python
		s3 = pd.Series([70, 100, 90, 80], index=['국어', '영어', '수학', '과학'])
		# 순번으로 조회
		s3[1]
		s3.iloc[1]
		# index 이름으로 조회
		s3['영어']
		s3.loc['영어']
		s3.영어
		```

- **팬시(fancy) 인덱싱**
    - Series[index리스트] 
    - 여러 원소 조회 시 조회할 index를 list로 전달
	```python
	s3[[1, 2, 3]]
	s3[['국어', '영어', '수학']]
	```
    
### Slicing
- **Series[start index :  end index : step]**
    - start index 생략 : 0번 부터
    - end index
        - **index 순번일 경우는 포함 하지않는다.**
        - **index 명의 경우는 포함한다.**
    - end index 생략 : 마지막 index까지
    - step 생략 : 1씩 증가
	```python
	s3[:'수학']
	s3[:3]
	s3['영어':'과학':2]
	s3[1::2]
	```
- **Slicing의 결과는 원본의 참조(View)를 반환**
	- shallow copy(얕은 복사)
		- Series, DataFrame, 넘파이 배열(ndarray)은 slicing 조회시 shallow copy
		- slicing한 결과를 변경시 **원본도 같이 바뀐다.**
	- deep copy(깊은 복사)
	    - Series.copy() : Series를 복사한 새로운 객체 반환
		- 원본의 카피본을 반환하여 **값 변경시 원본이 변경되지 않는다.**
		- 파이썬 리스트는 slicing시 deep copy
		- indexing은 deep copy
### Boolean 인덱싱
- Series 의 indexing 연산자에 boolean 리스트를 넣으면 True인 index의 값들만 조회한다. 
    - Boolean 연산자들을 이용해 원하는 조건의 값들을 조회할 수 있다
    - 다중 조건인 경우 반드시 ( )로 조건을 묶어야 한다. and, or 사용불가
	```python
	#example, 
	Series[Series>100]
	Series[~(Series>100)] # '~' not 조건절
	```
### 주요 메소드
- head(정수), tail(정수): 원하는 원소를 정수 갯수만큼 head와 tail에서 조회.
- value_counts(): Series반환, index명: 고윳값, value:개수
	- normalize=True 지정시 비율 반환.
	- value_counts(dropna=False): NaN을 포함한 고유값의 개수.
```
Series.value_count()
Series.value_count(normalize=True)
Series.value_counts(dropna=False)
```
- index : index명 조회, **원본의 index명 변경**, 개별적인 변경은 안됨.
```
#원본 Series의 index변경
Series.index =['indexName1','indexName2'...]
```
- rename: index명 변경. 복사본을 반환. inplace=True시 원본을 변경
```
Series.rename({'orginalIndexName':'IndexName'}) # 변경한 복사본을 반환
Series.rename({'orginalIndexName':'IndexName'}, inplace=True) # 원본을 변경. default값은 False
```
- count() 결측치 제외한 원소 개수 조회
```
결측치 삽입. 판다스의 결측치 넘파이의 nan
Series[index] = np.nan 
```
- sort_values(): 값으로 정렬. ascending=False 내림차순, True 오름차순
- sort_index(): index로 정렬. ascending=False 내림차순, True 오름차순
- unique(), nunique(): 고유값과, 고유값의 수를 조회. Nan값을 제외. 
- isin([values]): 각원소가 value값중 하나와 같으면 True, 다르면 False 반환.

### 통계량
- 판다스의 기술통계함수는 NA값을 무시(제거)하고 계산하는 것이 default.
	- skipna=False를 이용해서 NA값까지 계산 가능.
	```
	Series.sum(skipna=False)
	Series.mean(skipna=False)
	```
- 데이터셋의 데이터들의 특징을 하나의 숫자로 요약한 것.
- ### 평균 
    - 전체 데이터셋의 데이터들은 평균값 근처에 분포되어 데이터셋의 대표값으로 사용한다.
    - 이상치(너무 크거나 작은 값)의 영향을 많이 받는다.

- ### 중앙값
    - 분포된 값들을 작은값 부터 순서대로 나열한 뒤 그 중앙에 위치한 값
    - 이상치에 영향을 받지 않아 평균대신 집단의 대표값으로 사용한다.
- ### 표준편차/분산
    - 값들이 흩어져있는 상태(분포)를 추정하는 통계량으로 분포된 값들이 평균에서 부터 얼마나 떨어져 있는지를 나타내는 통계량.
    - 각 데이터가 평균으로 부터 얼마나 차이가 있는지를 편차(Deviation)라고 한다. ($평균-데이터$)
    - 분산 : 편차 제곱의 합을 총 개수로 나눈 값 
    
    - 표준편차
        - 분산의 제곱근
        - 분산은 원래 값에 제곱을 했으므로 다시 원래 단위로 계산한 값.
   
- ### 최빈값(mode)
    - 데이터 셋에서 가장 많이 있는 값.
- ###  분위수(Quantile)
    - 데이터의 크기 순서에 따른 위치값
        - 데이터셋을 크기순으로 정렬한뒤 N등분했을 때 특정 위치에서의 값 (단면)
        - N등분한 특정위치의 값들 통해 전체 데이터셋을 분포를 파악한다.
        - 대표적인 분위수 : 4분위, 10분위, 100분위
    - 데이터의 분포를 파악할 때 사용
    - 이상치 중 극단값들을 찾을 때 사용 (4분위수)

- 극단값(이상치): [실수]범위가 클수록 정상범위 증가 작을수록 정상범위 감소. 기본1.5
	- 극단적으로 작은값 < Q1 - IQR*[실수]
	- 극단적으로 큰값 > Q3 + IQR*[1.5]
	
## 결측치 (Missing Value, Not Available, N/A)
- 판다스에서 결측치
    - None, numpy.nan, numpy.NAN
    
### 결측치 확인
- Numpy
    - np.isnan(배열)
    ```python
	import numpy as np
	a = np.array([1,np.nan])
	np.isnan(a)
	```
- Series
    - Series객체.isnull()
    - Series.notnull()
- DataFrame
    - DataFrame객체.isnull(), DataFrame객체.isna()
    - DataFrame객체.notnull(), DataFrame객체.notna()
	
### 결측치 처리
- 제거 
    - dropna()
- 다른값으로 대체 
    - fillna() 
	
