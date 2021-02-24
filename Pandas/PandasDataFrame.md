# Pandas DataFrame
## DataFrame 개요
- 표(테이블-행렬)를 다루는 Pandas 클래스
    - 데이터베이스의 Table이나 R의 data.frame이나 Excel의 표와 동일한 역할
- 분석할 데이터를 가지는 판다스의 가장 핵심적인 클래스
- 행이름: index 열이름: column
    - 행이름과 열이름은 명시적으로 지정할 수 있다.
    - 명시적으로 지정하지 않으면 순번 (0부터 1씩 증가) 이 index, column 명으로 사용된다.
    - 하나의 행과 하나의 열은 Series로 구성된다.
- 직접 데이터를 넣어 생성하거나 데이터 셋을 파일(csv, 엑셀, DB 등)로 부터 읽어와 생성한다.

## DataFrame 생성
### 직접 생성
- `pd.DataFrame(data [, index=None, columns=None])`
- data 
    - DataFrame을 구성할 값을 설정
        - Series, List, ndarray를 담은 2차원 배열
        - 열이름을 key로 컬럼의 값 value로 하는 딕션어리(사전)
    - index
        - index명으로 사용할 값 배열로 설정
    - columns
        - 컬럼명으로 사용할 값 배열로 설정

### DataFrame 파일로 저장
- ### DataFrame객체.to_파일타입()
- DataFrame객체.to_csv(파일경로,sep=',', index=True, header=True, encoding)
    - 텍스트 파일로 저장
    - 파일경로: 저장할 파일경로(경로/파일명)
    - sep : 데이터 구분자
		- sep로 저장할경우 불러오기할때도 같은 sep로 맞춰서 불러와야한다.
    - index, header: 인덱스/헤더 저장 여부
    - encoding
        - 파일인코딩
        - 생략시 운영체제 기본 encoding 방식
- DataFrame객체.to_excel(파일경로, index=True, header=True)
    - 엑셀파일로 저장
	
### 파일로 부터 데이터셋을 읽어와 생성하기
### csv 파일 등 텍스트 파일로 부터 읽어와 생성
- `pd.read_csv(파일경로, sep=',', header, index_col, na_values, encoding)`
    - 파일경로 : 읽어올 파일의 경로
    - sep
        - 데이터 구분자. 
        - 기본값: 쉼표
    - header=정수
        - 열이름(컬럼이름)으로 사용할 행 지정
        - 기본값: 첫번째 행
        - None 설정: 첫번째 행부터 데이터로 사용하고 header(컬럼명)는 0부터 자동증가하는 값을 붙인다.
    - index_col=정수,컬럼명
        - index 명으로 사용할 열이름(문자열)이나 열의 순번(정수)을 지정.
        - 생략시 0부터 자동증가하는 값을 붙인다.
    - na_values
        - 읽어올 데이터셋의 값 중 결측치로 처리할 문자열 지정. 
        - NA, N/A, 빈값 => 결측치로 자동인식
    - encoding
        - 파일 인코딩
        - 생략시 운영체제 기본 encoding 방식
		
### 데이터 프레임의 기본 정보 조회
- csv 파일 읽기
- shape
- head()
- tail()
- info()
- isnull().sum() => 컬럼별 null 체크 (sum() 한번더 하면 총개수)
- index / columns : index와, 컬럼명 조회
- describe() : 숫자형-기술통계값, 문자열-총개수, 유니크값, 최빈값