# 과대적합(Overfitting )
- ### 일반화 (Generalization) 
    - 모델이 새로운 데이터셋(테스트 데이터)에 대하여 정확히 예측하면 이것을 (훈련데이터에서 테스트데이터로) 일반화 되었다고 말한다. 
    - 모델이 훈련 데이터로 평가한 결과와 테스트 데이터로 평가한 결과의 차이가 거의 없고 좋은 평가지표를 보여준다.
- ### 과대적합 (Overfitting)
    - 모델이 훈련 데이터에 대한 예측성능은 **너무** 좋지만 일반성이 떨어져 새로운 데이터(테스트 데이터)에 대해선 성능이 좋지 않은 것. 
    - 모델이 훈련 데이터 세트의 특징을 너무 맞춰서 학습 되었기 때문에 일반화 되지 않아 새로운 데이터셋(테스트세트)에 대한 예측 성능이 떨져 발생한다.
	- Overfitting(과대적합)의 원인
		- 모델이 너무 복잡한 경우
			- Overfitting을 줄이기 위한 규제 하이퍼파라미터 설정
			- Feature 개수 줄이기
		- 데이터의 문제
			- 데이터 전처리를 통해 질 좋은 데이터 생성.
			- 데이터를 더 수집한다. 
- ### 과소적합 (Underfitting)
    - 모델이 너무 간단하여 훈련 데이터에 대해 충분히 학습하지 못해 데이터셋의 패턴들을 다 찾아내지 못해서 발생, 훈련 데이터과 테스트 데이터셋 모두에서 성능이 안좋은 것을 말한다.

## Decision Tree 복잡도 제어(규제)
- Decision Tree 모델을 복잡하게 하는 것은 노드가 너무 많이 만들어 지는 것이다. 
    - 노드가 많이 만들어 질수록 훈련데이터셋에 과대적합된다.
- 규제 하이퍼 파라미터
```
DecisionTreeClassifier(max_depth=num, # 트리의 최대 깊이를 제한.
      				   max_leaf_nodes=num, # leaf node의 최대 개수를 제한.
					   min_samples_leaf=num) # leaf node가 되기위한 sample 수지정.
```

# GridSearch (그리드 서치)
## Grid Search 를 이용한 하이퍼파라미터 튜닝
- 하이퍼 파라미터 (Hyper Parameter)
    - 머신러닝 모델을 생성할 때 사용자가 직접 설정하는 값
    - 하이퍼 파라미터의 설정에 따라 모델의 성능이 달라진다.

## 최적의 하이퍼파라미터 찾기
1. 만족할 만한 하이퍼파라미터들의 값의 조합을 찾을 때 까지 일일이 수동으로 조정
2. GridSearch 사용
        - 하이퍼파라미터들을 지정하면 모든 조합에 대해 교차검증 후 제일 좋은 성능을 내는 하이퍼파라미터 조합을 찾는다.
        - 하이퍼파라미터와 값들이 많아질수록 많은 시간이 걸린다.
3. Random Search 사용    
        - 모든 조합을 다 시도하지 않고 각 반복마다 임의의 값만 대입해 지정한 횟수만큼만 평가한다.

### GridSearchCV 매개변수및 결과조회
- 주요 매개변수
    - estimator: 모델객체 지정
    - params : 하이퍼파라미터 목록을 dictionary로 전달 '파라미터명':[파라미터값 list] 형식
    - scoring: 평가 지표
    - cv : 교차검증시 fold 개수. 
    - n_jobs : 사용할 CPU 코어 개수 (None:1(기본값), -1: 모든 코어 다 사용)
- 메소드
    - fit(X, y) : 학습
    - predict(X): 제일 좋은 성능을 낸 모델로 predict()
    - predict_proba(X): 제일 좋은 성능을 낸 모델로 predict_proba() 호출
- 결과 조회 변수
    - cv_results_ : 파라미터 조합별 결과 조회
    - best_params_ : 가장 좋은 성능을 낸 parameter 조합 조회
    - best_estimator_ : 가장 좋은 성능을 낸 모델 반환
```
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import GridSearchCV

tree = DecisionTreeClassifier()
# 하이퍼 파라미터 후보들을 딕셔너리로 지정. 파라미터이름:[후보]
# 지정하지 않은 것들은 default값을 사용한다.
param_grid = {
    "max_depth":range(1,11), # depth를 1에서 10까지
    "max_leaf_nodes": [3,5,7,9] # leaf node수를 3,5,7,9개로
}

grid_search = GridSearchCV(tree, # 학습시킬 모델
                          param_grid=param_grid, # 하이퍼 파라미터
                          #scoring='accuracy', # 평가 지표 하나일때는 refit 불필요.
                          scoring=['accuracy','recall','precision'], # 평가 지표를 여러개 지정시 리스트로
                          refit="accuracy", # 평가지표가 여러개일시 어떤지표로 best_estimator를 만들지 지정해줘야한다.
                          cv=5, # 교차검증(Cross Validation)의 folder개수(몇개로 나눌 것인지)
                          n_jobs=-1) #모든 코어를 다 사용
# 학습(train) - 최적의 하이퍼파라미터 조합
grid_search.fit(X_train, y_train)
# 파라미터 조합별 결과 조회
grid_search.cv_results_
# 가장 좋은 성능을 낸 parameter 조합 조회
grid_search.best_params_
#가장 좋은 성능을 낸 모델 반환
grid_search.best_estimator_
#accuracy score확인
pred_test = grid_search.best_estimator_.predict(X_test)
accuracy_score(y_test, pred_test)
```

### RandomizedSearchCV
- 주요 매개변수 (GridSearchCV와 비슷)
    - estimator: 모델객체 지정
    - param_distributions : 하이퍼파라미터 목록을 dictionary로 전달 '파라미터명':[파라미터값 list] 형식
    - **n_iter : 파라미터 검색 횟수**
    - scoring: 평가 지표
    - cv : 교차검증시 fold 개수. 
    - n_jobs : 사용할 CPU 코어 개수 (None:1(기본값), -1: 모든 코어 다 사용)
- 메소드
    - fit(X, y) : 학습
    - predict(X): 제일 좋은 성능을 낸 모델로 predict()
    - predict_proba(X): 제일 좋은 성능을 낸 모델로 predict_proba() 호출
- 결과 조회 변수
    - cv_results_ : 파라미터 조합별 결과 조회
    - best_params_ : 가장 좋은 성능을 낸 parameter 조합 조회
    - best_estimator_ : 가장 좋은 성능을 낸 모델 반환
```
tree = DecisionTreeClassifier()
param_grid = {
    'max_depth':range(1,21), # depth 1~20, 20개
    'max_leaf_nodes':range(2,11), # leaf개수 9개
    'criterion':['gini', 'entropy'] # 판단기준(불순도 계산방식) 2개 
}
#총 360개의 조합(20*9*2)중 임의의 50개의 조합만 확인
n_iter = 50 # 확인할 조합의 개수. default: 10
randomized_search = RandomizedSearchCV(tree,
                                       param_distributions=param_grid,
                                       n_iter=n_iter, 
                                       scoring='accuracy', 
                                       cv=3, 
                                       n_jobs=-1)
randomized_search.fit(X_train, y_train)
# 파라미터 조합별 결과 조회
randomized_search.cv_results_
# 가장 좋은 성능을 낸 parameter 조합 조회
randomized_search.best_params_
#가장 좋은 성능을 낸 모델 반환
randomized_search.best_estimator_
```


