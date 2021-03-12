## concat() 이용
- 수직, 조인을 이용한 수평 결합 모두 지원한다.
- 조인(수평결함)의 경우 full outer join과 inner join을 지원한다.
    - full outer join이 기본 방식
    - 조인 기준: index가 같은 행 끼리 합친다. (equi-join)
- pd.concat(objs,  [, key=리스트]), axis=0, join='outer' )
    - 매개변수
        - objs: 합칠 DataFrame들을 리스트로 전달
        - keys=[] 를 이용해 합친 행들을 구분하기 위한 다중 인덱스 처리
        - axis
            - 0 또는 index : 수직결합
            - 1 또는 columns : 수평결합
        - join: 
            - 조인방식
            - 'outer'(기본값) 또는 'inner'
			
- 수직으로 합칠때 index따로 주기. 개별 df의 index가 순번일 경우
	- ignore_index = True 개별 index를 부여. defalt값은 False
	- `pd.concat(objs, ignore_index=True)`
- index를 지정해서 합칠때
	- `df.set_index('index_name').concat(objs)`
# 조인(join)
> - 여러 데이터프레임에 흩어져 있는 정보 중 필요한 정보만 모아서 결합하기 위한 것.
> - 두개 이상의 데이터프레임을 특정 컬럼(열)의 값이 같은 행 끼리 수평 결합하는 것.
> - Inner Join, Left Outer Join, Right Outer Join, Full Outer Join

### join()
- dataframe객체.join(others, how='left', lsuffix='', rsuffix='') 
- `df_A.join(df_b)`, `df_A.join([df_b, df_c, df_d])`
- **두개 이상의 DataFrame들을 조인 할 수 있다.**
    - **조인 기준**: index가 같은 값인 행끼리 합친다. (equi-join)
    - **조인 기본 방식**: Left Outer Join
- 매개변수
    - lsuffix, rsuffix
        - 조인 대상 DataFrame에 같은 이름의 컬럼이 있으면 에러 발생.
        - 같은 이름이 있는 경우 붙일 접미어 지정
		- 한개이상의 rsuffix 지정 안됨.
		- 두개이상의 dataframe을 조인시
			- 컬럼명에 suffix(접미어)를 붙인다.
			- `df.add_suffix('name')`
    - how :조인방식. 'left', 'right', 'outer', 'inner'. left가 기본
		- 두개이상의 dataframe을 조인할때 right 조인은 불가능.
- index를 지정해서 합칠때
	- `df.set_index('index_name').join(objs.set_index('index_name))`

### merge()
- `df_a.merge(df_b)`
- **두개의 DataFrame 조인만 지원**
    - **조인 기준**: 같은 컬럼명을 기준으로 equi-join이 기본. **조인기준을 다양하게 정할 수 있다.**
    - **조인 기본 방식**: inner join
- `dataframe.merge(합칠dataframe, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False)`  
- 매개변수
    - on : 같은 컬럼명이 여러개일때 join 대상 컬럼을 선택	
    - right_on, left_on : 조인할 때 사용할 왼쪽,오른쪽 Dataframe의 컬럼명. 
    - left_index, right_index: 조인 할때 index를 사용할 경우 True로 지정 
    - how : 조인 방식.  'left', 'right', 'outer', 'inner'. 기본: inner 
    - suffixes: 두 DataFrame에 같은 이름의 컬럼명이 있을 경우 구분을 위해 붙인 접미어를 리스트로 설정
        - 생략시 x, y를 붙인다.     
		
> - 수직으로 합치는 경우(Union) : concat() 사용
> - **두개 이상의** DataFrame을 조인할 때는 하는 경우 : join() 사용
> - 두개의 DataFrame을 조인할 때는 **merge()** 를 사용한다. => 컨트롤이 편하다.






