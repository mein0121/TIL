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





