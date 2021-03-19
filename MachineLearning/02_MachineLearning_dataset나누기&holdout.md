## 훈련데이터셋과 평가(테스트)데이터 분할
- 전체 데이터 셋을 두개의 데이터셋으로 나눠 하나는 모델을 훈련할 때 사용하고 다른 하나는 그 모델을 평가할 때 사용한다.
- 보통 훈련데이터와 테스트데이터의 비율은 8:2 또는 7:3 정도로 나누는데 데이터셋이 충분하다면 6:4까지도 나눈다.

### 데이터셋 분할시 주의
- 각 클래스(분류대상)가 같은 비율로 나뉘어야 한다. 

## scikit-learn의  train_test_split() 함수 이용 iris 데이터셋 분할
```
# Dataset을 Train dataset과 test dataset으로 분할해주는 함수.
from sklearn.model_selection import train_test_split
# input, output
X_train, X_test, Y_train, Y_test = train_test_split(iris['data'],   #input dataset
                                   iris['target'], #ouput dataset
                                   test_size=0.2,  # test set의 비율(0 ~ 1), default: 0.25
                                   stratify=iris['target'], # 각 클래스(분류대상)들을 원본데이터셋과 같은 비율로 나눠라.
                                   random__state=1) # random의 seed값 정의
```

## 평가
- 머신러닝 평가지표 함수들은 sklearn.metrics 모듈에 있다.
- accuracy(정확도)
    - 전체 데이터셋중 맞춘 개수의 비율
```
from sklearn.metrics import accuracy_score #정확도 검증하는 함수
acc_train_score = accuracy_score(Y_train, pred_train)
acc_test_score = accuracy_score(Y_test, pred_test)
print("Train Set 정확도:", acc_train_score)
print("Test Set 정확도:", acc_test_score)
```
- 혼동행렬 (Confusion Matrix)
    - 예측 한 것이 실제 무엇이었는지를 표로 구성한 평가 지표
    - 분류의 평가 지표로 사용된다.
    - axis=0: 실제, axis=1: 예측
```
from sklearn.metrics import confusion_matrix
cm_train = confusion_matrix(Y_train, pred_train)
cm_test = confusion_matrix(Y_test, pred_test)
```

# Hold Out
- 데이터셋을 Train set, Validation set, Test set으로 나눈다.
- sklearn.model_selection.train_test_split()  함수 사용

## 데이터셋
- ### Train 데이터셋 (훈련/학습 데이터셋)
    - 모델을 학습시킬 때 사용할 데이터셋.
- ### Validation 데이터셋 (검증 데이터셋)
    - Train set으로 학습한 모델의 성능을 측정하기 위한 데이터셋
- ### Test 데이터셋 (평가 데이터셋)
    - 모델의 성능을 최종적으로 측정하기 위한 데이터셋
    - **Test 데이터셋은 마지막에 모델의 성능을 측정하는 용도로 한번만 사용되야 한다.**
        - 학습과 평가를 반복하다 보면 모델이 검증때 사용한 데이터셋에 과적합되어 새로운 데이터에 대한 성능이 떨어진다.    
          그래서 데이터셋을 train 세트, validation 세터, test 세트로 나눠 train 세트와 validation 세트로 모델을 최적화 한 뒤 마지막에 test 세트로 최종 평가를 한다.

```
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split

#Dataset loading
iris = load_iris()
X, y = iris['data'], iris['target']

#Train, Test Dataset 분리
X_train, X_test, y_train, y_test = train_test_split(X, y,
                                                   test_size=0.2,
                                                   stratify=y,
                                                   random_state=1)

#Train Validation Dataset 분리
X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, 
                                                 test_size=0.2,
                                                 stratify=y_train,
                                                 random_state=1)

# 모델 생성
tree = DecisionTreeClassifier(max_depth=3, random_state=1) # 하이퍼 파라미터(hyper parameter)
# 모델 Train
tree.fit(X_train, y_train)

# 예측 및 검증 (validation set)
pred_train = tree.predict(X_train)
pred_val = tree.predict(X_val)

acc_train = accuracy_score(y_train, pred_train)
acc_val = accuracy_score(y_val, pred_val)

# test dataset으로 마지막 평가(검증)
pred_test = tree.predict(X_test)
acc_test = accuracy_score(y_test, pred_test)
print('최종 검증결과(test): ', acc_test)
```
### Holdout 방식의 단점
- train/test 셋이 어떻게 나눠 지냐에 따라 결과가 달라진다.
    - 데이터가 충분히 많을때는 변동성이 흡수되 괜찮으나 수천건 정도로 적을 때는 문제가 발생할 수 있다.
- 데이테셋의 양이 적을 경우 학습을 위한 데이터 양이 너무 적어 학습이 제대로 안될 수 있다.    

## K-겹 교차검증 (K-Fold Cross Validation)
- 데이터셋을 K 개로 나눈 뒤 하나를 검증세트로 나머지를 훈련세트로 하여 모델을 학습시키고 평가한다. 나뉜 K개의 데이터셋이 한번씩 검증세트가 되도록 K번 반복하여 모델을 학습시킨 뒤 나온 평가지표들을 평균내서 모델의 성능을 평가한다.
- 종류
### K-Fold
```
from sklearn.datasets import load_iris
from sklearn.model_selection import KFold
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

iris = load_iris()
X, y = iris['data'], iris['target']
# 객체를 생성하면서 몇개의 fold로 나눌지 (K값)을 지정. 
kfold = KFold(n_splits=5) #K=3
acc_train_list = [] #trainset으로 평가한 정확도를 저장할 리스트
acc_test_list = [] #testset으로 평가한 정확도를 저장할 리스트
# kfold.split(나누려는 InputData): Generator반환. train/test set의 index를 반환
for train_index, test_index  in kfold.split(X):
	# 데이터셋 분리
	X_train, y_train = X[train_index], y[train_index]
	X_test, y_test = X[test_index], y[test_index]
	# 모델생성	/ 학습
	tree = DecisionTreeClassifier(max_depth=2)
	tree.fit(X_train, y_train)
	# 검증
	pred_train = tree.predict(X_train)
	pred_test = tree.predict(X_test)
	acc_train = accuracy_score(y_train, pred_train)
	acc_test = accuracy_score(y_test, pred_test)
	#평가결과 리스트에 추가
	acc_train_list.append(acc_train)
	acc_test_list.append(acc_test)
```
- K-Fold의 문제점
	- 원 데이터셋의 row 순서대로 분할하기 때문에 불균형 문제가 발생할 수 있다.	

### Stratified K-Fold
- 나뉜 fold 들에 label들이 같은(또는 거의 같은) 비율로 구성 되도록 나눈다. 
```
from sklearn.model_selection import StratifiedKFold
#객체 생성시 몇개의 fold로 나눌지 지정 (K값)
s_fold = StratifiedKFold(n_splits=3)
acc_train_list = []
acc_test_list = []

#label별 동일한 분포로 분할해야 하므로 label데이터셋(y)도 같이 준다. 반환값: generator
for train_index, test_index in s_fold.split(X, y):
    # train, test set data 분리/생성
    X_train, y_train = X[train_index], y[train_index]
    X_test, y_test = X[test_index], y[test_index]
    
    # 모델 생성/학습
    tree = DecisionTreeClassifier(max_depth=2)
    tree.fit(X_train, y_train)
    
    # 검증
    pred_train = tree.predict(X_train)
    pred_test = tree.predict(X_test)
    
    acc_train_list.append(accuracy_score(y_train, pred_train))
    acc_test_list.append(accuracy_score(y_test, pred_test))
```

### cross_val_score( )
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
#Dataset loading
iris = load_iris()
X, y = iris['data'], iris['target']
# 모델 생성
tree = DecisionTreeClassifier(max_depth=2)
scores = cross_val_score(estimator=tree, # 모델 지정.
                         X=X, # feature
                         y=y, # label
                         scoring='accuracy',
                         cv=3)
```

