# Data Processing
## 범주형 변수(Categorical Variable)
- 몇 개의 범주 중 하나에 속하는 값들로 구성된 변수. 어떤 분류에 대한 속성을 가지는 변수를 말한다.
    - 예) 성별 - 남/녀, 혈액형 - A, B, AB, O, 성적 - A,B,C,D,F
- 비서열(Unordered) 변수 
    - 범주에 속한 값간에 서열(순위)가 없는 변수
    - 성별, 혈액형
- 서열 (Ordered) 변수
    - 범주에 속한 값 간에 서열(순위)가 있는 변수
    - 성적, 직급
- 사이킷런은 문자열 값을 입력 값으로 처리 하지 않기 때문에 숫자 형으로 변환해야 한다.
    - 범주형 변수의 경우 전처리를 통해 정수값으로 변환한다.
    - 범주형이 아닌 단순 문자열인 경우 일반적으로 제거한다.
	
## 범주형 Feature의 처리
## 레이블 인코딩(Label encoding)
- 문자열(범주형) 값을 오름차순 정렬 후 0 부터 1씩 증가하는 값으로 변환
- **숫자의 차이가 모델에 영향을 주지 않는 트리 계열 모델(의사결정나무, 랜덤포레스트)에 적용한다.**
- **숫자의 차이가 모델에 영향을 미치는 선형 계열 모델(로지스틱회귀, SVM, 신경망)에는 사용하면 안된다.**
- **sklearn.preprocessing.LabelEncoder** 사용
    - fit(): 어떻게 변환할 지 학습
    - transform(): 문자열를 숫자로 변환
    - fit_transform(): 학습과 변환을 한번에 처리
    - inverse_transform():숫자를 문자열로 변환
    - classes_ : 인코딩한 클래스 조회
```
from sklearn.preprocessing import LabelEncoder

items = ['TV','냉장고', '냉장고','컴퓨터', 'TV','컴퓨터','에어콘','TV','에어콘','에어콘']
label_encoder = LabelEncoder()
# 학습: 어떻게 바꿀지 학습.
label_encoder.fit(items) 
# transform(): 문자열를 숫자로 변환
# 1차원 배열형태: List, Series, ndarray, 반환타입: ndarray
labels = label_encoder.transform(items)

# 학습데이터와 변환할 데이터가 동일한 경우
label_encoder2 = LabelEncoder()
# 학습과 변환을 동시에
labels2 = label_encoder2.fit_transform(items) 
```


