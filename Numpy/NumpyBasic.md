## 넘파이 (Numpy) 
- Numerical Python
- 고성능 과학연산을 위한 패키지로 데이터분석, 머신러닝등에  필수로 사용.
- 강력한 다차원 배열(array)  지원
- 벡터 연산 지원
- 다양한 수학관련 함수
- 선형대수, 난수 생성, 푸리에 변환 기능 지원

### 넘파이에서 데이터 구조
- 스칼라 (Scalar)
	- 하나의 숫자로 이루어진 데이터
- 벡터 (Vector): 1D Array (1차원 배열)
	- 여러개의 숫자들을 특정한 **순서대로** 모아놓은 데이터 모음(데이터 레코드)
- 행렬 (Matrix): 2D Array (2차원 배열)
	- 벡터들을 모아놓은 데이터 집합
- 텐서 (Tensor): ND Array (다차원 배열)
	- 같은 크기의 행렬들(텐서들)을 모아놓은 데이터 집합
	
```
ex) array에 따른 rank(축의 수), shape(축별 데이터 수), size(원소 수).
arr=[
	[1, 2, 3, 4]
	[5, 6, 7, 8]
	[9,10,11,12]ㄴ
	]
rank = 2, shape = (3,4), size = 12

arr=[	
		[
			[1,2,3],
			[3,4,5]
		],
		[
			[6,7,8],
			[8,9,10]
		]
	]
rank = 3, shape = (2,2,3), size = 12
```

### 넘파이 배열(ndarray)
- Numpy에서 제공하는 n-dimensional array
- **같은 타입의 값**들만 가질 수 있다.
- 빠르고 메모리를 효율적으로 사용하며 벡터 연산과 브로드캐스팅 기능을 제공한다. 
- 서로 다른 타입의 값을 배열에 넣으면 가장 큰 타입으로 통일 변환한다.
	- bool < int < float < str

### 배열 생성 함수
- array(배열형태 객체 [, dtype])
	- 배열형태 객체(array-like)  
		- 리스트, 튜플, 넘파이배열(ndarray), Series

### 데이터 타입
- ndarray 는 같은 타입의 데이터만 모아서 관리한다.
- 배열 생성시 dtype 속성을 이용해 데이터 타입 설정 가능
- ndarray.astype(데이터타입): 데이터타입 변환
```python
a2 = a1.astype(np.int) 
# a1배열의 타입을 int(int32)로 변환. 변환한 값을을 가진 새로운 배열을 반환.
```
#### rank,shape,size반환 메소드.
- 배열.ndim = rank를 반환.
- 배열.Shape = 튜플로 shape를 반환.
- 배열.size = 총원소수를 반환.

### 배열 생성 메소드
- zeros(shape, dtype): 영벡터(행렬) 생성
- ones(shape, dtype): 일벡터 생성
- full(shape, fill_value, dtype)
	- 원소들을 원하는 값으로 채운 배열 생성
- arange(start, stop, step, dtype) 
	- start에서 stop 범위에서 step(default:1)의 일정한 간격의 값들로 구성된 배열 리턴 
```python
ex)
np.zeros((3,4,5,6),dtype=np.float32)
np.ones((3,4,5,6),dtype=np.float32)
np.full((4,3,3,3),7,dtype =np.int)
np.arange(1,10,2,dtype=np.int32)
np.arange(0,20).reshape(2,2,5) # 0~20까지 배열을 2,2,5 shape로 변환.
```
- XXX_like(배열)
	- zeros_like(배열),ones_like(배열)
	- 매개변수로 받은 배열과 같은 shape의 배열을 0,1로 채운후 반환.
```
np.zeros_like(배열)
np.ones_like(배열)
```
- linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
	- start부터 stop까지 num수로 분할, num의 default값 = 50
	- endpoint : stop을 포함시킬 것인지 여부. default값 = True
	- retstep : 생성된 배열 샘플과 함께 간격(step)도 리턴할지 여부. True일경우 간격도 리턴(sample, step) => 튜플로 받는다.
```
np.linspace(1,10,5,false,True)
return > (array([1. , 2.8, 4.6, 6.4, 8.2]), 1.8)
```
- identity(N)
	- 대각에만 값(1) 나머지는 0으로 채운 행렬.
- eye(N, M=None, k=0, dtype=<class 'float'>) 
	- N: 행수, M: 컬럼수, k=시작할 대각선의 index.
	- N행수와 M컬럼수를 가지고 k인덱스부터 대각으로 1, 그외는 0을 채운 행렬.
	
### 난수를 원소로 하는 ndarray 생성
- numpy의 서브패키지인 random 패키지에서 제공하는 함수들
- np.random.seed(정수) : 시드값 설정
- np.random.rand(axis0[, axis1, axis2, ...])
- np.random.normal(loc=0.0, scale=1.0, size=None)
- np.random.randint(low, high=None, size=None, dtype='l')
- np.random.choice(a, size=None, replace=True, p=None)
