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
- shape
- head()
- tail()
- info()
- isnull().sum() => 컬럼별 null 체크 (sum() 한번더 하면 총개수)
- index / columns : index와, 컬럼명 조회
- describe() : 숫자형-기술통계값, 문자열-총개수, 유니크값, 최빈값

## 컬럼이름/행이름 조회 및 변경
- DataFrame객체.columns
    - 컬럼명 조회
- DataFrame객체.index
    - 행명 조회
	
### 컬럼이름/행이름 변경
- 컬럼과 인덱스는 불변의 성격을 가짐
- columns와 index 속성으로는 통째로 바꾸는 것은 가능하나 index로 하나씩 바꾸는 것은 안된다.
    - `df.columns = ['새이름','새이름', ... , '새이름'] `
    - `df.columns[1] = '새이름' # 이건 안된다. `

### 컬럼이름/행이름 변경 관련 메소드    
- `DataFrame객체.rename(index=행이름변경설정, columns=열이름변경설정, inplace=False)`
- `DataFrame객체.rename(mappers= 행/열 이름변경설정, axis=축번호, inplace=False)`
	- `grade.rename(mapper=딕셔너리, axis=1)`: columns변경
	- axis=0: index 변경, axis=1: columns변경 
	- 개별 컬럼이름/행이름 변경 하는 메소드
    - 변경설정: 딕셔너리 사용
        - {'기존이름':'새이름', ..}
        - inplace: 원본을 변경할지 여부(boolean): default는 False. True시 원본 변경.
- `DataFrame객체.set_index(컬럼이름, inplace=False)`
    - 특정 컬럼을 행의 index 명으로 사용
    - 열이 index명이 되면서 그 컬럼은 Data Set 에서 제거된다.
- `DataFrame객체.reset_index(drop=False, inplace=False)`
    - index를 첫번째 컬럼으로 복원
	- drop=True: index명을 제거하고 순번으로 변경.

## 행 / 열 삭제
- DataFrame객체.drop(columns, index, inplace=False)
- DataFrame객체(labels=삭제할 컬럼/index이름, axis=삭제할 축)
    - columns : 삭제할 열이름 또는 열이름 리스트
    - index : 삭제할 index명 또는 index 리스트
    - inplace: 원본을 변경할지 여부(boolean):default는 False

### 다양한 열선택 기능을 제공하는 메소드들
- `select_dtypes(include=[데이터타입,..], exclude=[데이터타입,..])`
    - 전달한 데이터 타입의 열들을 조회. 
    - include : 조회할 열 데이터 타입
    - exclude : 제외하고 조회할 열 데이터 타입
- `filter (items=[], like='', regex='')`
   - 매개변수에 전달하는 열의 이름에 따라 조회
    - 각 매개변수중 하나만 사용할 수 있다.
    - items = [ ] 
        - 리스트와 일치하는 열들 조회
        - 이름이 일치 하지 않아도 Error 발생안함.
    - like = “ “ 
        - 전달한 문자열이 들어간 열들 조회
        - 부분일치 개념
    - regex = “ ”
        - 정규 표현식을 이용해 열명의 패턴으로 조회