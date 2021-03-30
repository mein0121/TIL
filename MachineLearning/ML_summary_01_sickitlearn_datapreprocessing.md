# Scikit_learn
## 사이킷런 주요모듈.
![image](/images/scikit_learn.png)

### scikit_learn 개발 패턴
	1. 데이터 분할
		- training, validation, test 셋으로 분리.
	2. 모델 생성
		- 예측 목적에 맞는 모델생성
		- 하이퍼 파라미터 설정.
	3. 모델 학습
		- fit, 훈련데이터로 모델 학습 또는 특징 추출
	4. 예측
		- predict / predict_prob (예측), transform(변환)
	5. 평가
		- 모델 성능 평가, 정확도,R2,MSE 등, 적절한 평가함수 이용 결과 확인.

### Scikit_learn 데이터셋 
- sklearn.datasets.load_xxxx 이용
- 구성
    - **target_names**: 예측하려는 값(class)을 가진 문자열 배열
    - **target**: Label(출력데이터)
    - **data**: Feature(입력변수)
    - **feature_names**: 입력변수 각 항목의 이름
    - **DESCR**: 데이터셋에 대한 설명

### 데이터셋 분할
- scikit-learn의 train_test_split() 함수 이용
#### Holdout
- 데이터셋을 Train set, Validation set, Test set으로 나눈다.
- 단점
	- train/test 셋이 어떻게 나눠 지냐에 따라 결과가 달라진다.
	- 데이테셋의 양이 적을 경우 학습을 위한 데이터 양이 너무 적어 학습이 제대로 안될 수 있다.
```
X_train, X_test, Y_train, Y_test = train_test_split(df['data'],   #input dataset
												    df['target'], #ouput dataset
												    test_size=0.2,  # test set의 비율(0 ~ 1), default: 0.25
												    stratify=df['target'], # 각 클래스(분류대상)들을 원본데이터셋과 같은 비율로 나눠라.
												    random_state=1) # random의 seed값 정의
```

#### K-fold cross validation
- 데이터셋을 K 개로 나눈 뒤 하나를 검증세트로 나머지를 훈련세트로 하여 모델을 학습시키고 평가한다. 
- 나뉜 K개의 데이터셋이 한번씩 검증세트가 되도록 K번 반복하여 모델을 학습시킨 뒤 나온 평가지표들을 평균내서 모델의 성능을 평가한다.
- 종류
    - K-Fold
    - Stratified K-Fold
```
from sklearn.model_selection import KFold
# 객체를 생성하면서 몇개의 fold로 나눌지 (K값)을 지정. 
kfold = KFold(n_splits=5)

# kfold.split(나누려는 InputData): Generator반환. train/test set의 index를 반환
for train_index, test_index  in kfold.split(X):
    # 데이터셋 분리
    X_train, y_train = X[train_index], y[train_index]
    X_test, y_test = X[test_index], y[test_index]
```
- 단점
	- 원 데이터셋의 row 순서대로 분할하기 때문에 불균형 문제가 발생할 수 있다.

#### Stratified K 폴드
- 나뉜 fold 들에 label들이 같은(또는 거의 같은) 비율로 구성 되도록 나눈다. 
- from sklearn.model_selection.StratifiedKFold 이용
```
from sklearn.model_selection import StratifiedKFold
s_fold = StratifiedKFold(n_splits=3) #객체 생성시 몇개의 fold로 나눌지 지정 (K값)

# s_fold.split(나누려는 InputData): Generator반환. train/test set의 index를 반환
for train_index, test_index in s_fold.split(X, y):
    # train, test set data 분리/생성
    X_train, y_train = X[train_index], y[train_index]
    X_test, y_test = X[test_index], y[test_index]
```

### Decision Tree 모델성.
1. import model
`from sklearn.tree import DecisionTreeClassifier`
2. 모델 생성
`tree = DecisionTreeClassifier()`
3. 모델 학습
`tree.fit(X_train, y_train']) # input_data(feature), output_data(label)`
4. 예측
`tree.predict(X_train)`

### 평가
- sklearn.metrics 이용
`from sklearn.metrics import accuracy_score #정확도 검증하는 함수`

#### cross_val_score()
- 데이터셋을 K개로 나누고 K번 반복하면서 평가하는 작업을 처리해 주는 함수
- 주요매개변수
    - estimator: 학습할 평가모델객체
    - X: feature
    - y: label
    - scoring: 평가지표
    - cv: 나눌 개수 (K)
- 반환값: array - 각 반복마다의 평가점수   
```
from sklearn.model_selection import cross_val_score
scores = cross_val_score(estimator=tree, # 모델 지정.
                         X=X, # feature
                         y=y, # label
                         scoring='accuracy',
                         cv=3)
```

## Data preprocessing		
### 레이블 인코딩(Label encoding)
- 문자열(범주형) 값을 오름차순 정렬 후 0 부터 1씩 증가하는 값으로 변환
- sklearn.preprocessing.LabelEncoder 이용
    - inverse_transform():숫자를 문자열로 변환
    - classes_ : 인코딩한 클래스 조회

### One-Hot encoding
- N개의 클래스를 N 차원의 One-Hot 벡터로 표현되도록 변환
    - 고유값들을 피처로 만들고 정답에 해당하는 열은 1로 나머진 0으로 표시한다.
- 학습시킬경우 2차원배열로 변환하여 적용
- sklearn.preprocessing.OneHotEncoder 이용
`onehotencoding.fit(items.reshape(-1,1))`
`ohe2.fit_transform(items[..., np.newaxis])`
`pd.get_dummies(df, columns=['column1', 'column2']) # columns에 인코딩 대상 컬럼들을 지정한다.`

### StandardScaler (표준화)
- 피쳐의 값들이 평균이 0이고 표준편차가 1인 범위(표준정규분포)에 있도록 변환한다.
    - 0을 기준으로 모든 데이터들이 모여있게 된다
- sklearn.preprocessing.StandardScaler 이용

### MinMaxScaler
- 데이터셋의 모든 값을 0과 1 사이의 값으로 변환한다.
- 𝑁𝑒𝑤_𝑥𝑖 = 𝑥𝑖−𝑚𝑖𝑛(𝑋) / 𝑚𝑎𝑥(𝑋)−𝑚𝑖𝑛(𝑋)
- sklearn.preprocessing.MinMaxScaler 이용

