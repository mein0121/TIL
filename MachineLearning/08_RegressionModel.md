# 회귀(Regression)
지도 학습(Supervised Learning)으로 예측할 Target이 연속형(continuous) 데이터(float)인 경우

## 회귀의 주요 평가 지표
예측값과 실제 값간의 차이를 구한다

- ### MSE (Mean Squared Error)
    - 실제 값과 예측값의 차를 제곱해 평균 낸 것
    - mean_squared_error() 
    - 'neg_mean_squared_error'
- ### RMSE (Root Mean Squared Error)
    - MSE는 오차의 제곱한 값이므로 실제 오차의 평균보다 큰 값이 나온다.  MSE의 제곱근이 RMSE이다.
    - scikit-learn은 함수를 지원하지 않는다. (MSE를 구한 뒤 np.sqrt()로 제곱근을 구한다.)
- ### 𝑅^2 (R square, 결정계수)
    - 평균으로 예측했을 때 오차(총오차) 보다 모델을 사용했을 때 얼마 만큼 더 좋은 성능을 내는지를 비율로 나타낸 값. 
    - 1에 가까울 수록 좋은 모델.
    - r2_score()
    - 'r2'
	
## 선형회귀 개요
- 선형 회귀(線型回歸, Linear regression)는 종속 변수 y와 한 개 이상의 독립 변수X와의 선형 상관 관계를 모델링하는 회귀분석 기법.
-`from sklearn.linear_model import LinearRegression`

## 손실(loss)함수/오차(error)함수/비용(cost)함수/목적(objective)함수
- 모델이 출력한 예측값과 실제 값 사이의 차이를 계산하는 함수
- 평가 지표로 사용되기도 하고 모델을 최적화하는데 사용된다.

## 최적화(Optimize)
- 손실함수의 값이 최소화 되도록 모델을 학습하는 과정.
- 최적화의 두가지 방법
    - 정규방정식
    - 경사하강법
	
## 전처리
선형회귀 모델사용시 전처리
- 범주형: 원핫 인코딩
- Feature Scaling을 통해서 각 컬럼들의 값의 단위를 맞춰준다.
    - StandardScaler를 사용해 scaling하는 경우 성능이 더 잘나오는 경향이 있다.

## 다항회귀(Polynomial Regression)
- 단순한 직선형 보다 복잡한 비선형의 데이터셋을 학습하기 위한 방식
- Feature들을 거듭제곱한 것과 Feature들을 곱한 새로운 특성을 추가한 뒤 선형모델로 훈련시킨다.
- `PolynomialFeatures` Transformer 를 사용.

## 규제 (Regularization)
- 선형 회귀 모델에서 과적합 문제를 해결하기 위해 가중치(회귀계수)에 페널티 값을 적용하는 것.
- 입력데이터의 Feature들이 너무 많은 경우 과적합이 발생.
    - Feature수에 비해 관측치 수가 적은 경우 모델이 복잡해 지면서 과적합이 발생한다.
- 해결
    - 데이터를 더 수집한다. 
    - Feature selection
        - 불필요한 Features들을 제거한다.
    - 규제 (Regularization) 을 통해 Feature들에 곱해지는 가중치가 커지지 않도록 제한한다.

## Ridge Regression
!(/images/ridgeregression.png)

