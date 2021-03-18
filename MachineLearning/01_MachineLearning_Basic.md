## 인공지능 (AI - Artificial Intelligence) 이란

### 정의
- 다트머스대학 수학과 교수인 존 매카시(John McCarthy)가 "지능이 있는 기계를 만들기 위한 과학과 공학" 이란 논문에서 처음으로 제안(1955년)
- 인간의 지능(인지, 추론, 학습 등)을 컴퓨터나 시스템 등으로 만든 것 또는, 만들 수 있는 방법론이나 실현 가능성 등을 연구하는 기술 또는 과학
    
> - 지능: 어떤 문제를 해결하기 위한 지적 활동 능력
> - 인공지능
>      - 기계가 사람의 지능을 모방하게 하는 기술
>      - 규칙기반, 데이터 학습 기반

## Strong AI vs Week AI
- **Artificial General Intelligence (AGI)**
    - 인간이 할 수 있는 모든 지적인 업무를 해낼 수 있는 (가상적인) 기계의 지능을 말한다. 인공지능 연구의 주요 목표.
- **Strong AI (강 인공지능)**
    - AGI 성능을 가지는 인공지능
    - 인공지능 연구가 목표하는 방향.
    
- **Week AI (약 인공지능)**
    - 기존에 인간은 쉽게 해결할 수 있었지만 컴퓨터로 처리하기 어려웠던 일을 컴퓨터가 수행할 수 있도록 하는 것이 목적.
    - 지각(知覺)을 가지고 있지 않으며 특정한 업무를 처리하는데 집중한다.
	
# 머신러닝 알고리즘 분류

## 지도학습(Supervised Learning)
- 모델에게 데이터의 특징(Feature)와 정답(Label)을 알려주며 학습시킨다.
- 대부분의 머신러닝은 지도학습이다.
- ### 분류(Classification):
    - 두개 이상의 클래스(범주)에서 선택을 묻는 지도 학습방법
        - 이진 분류 : 분류 대상 클래스가 2개
        - 다중 분류 : 분류 대상 클래스가 여러개
    - 의사결정트리(Decision Tree)
    - 로지스틱 회귀(Logistic Regression)
    - K-최근접 이웃(K-Nearest Neighbors, KNN)
    - 나이브 베이즈(Naive Bayes)
    - 서포트 벡터 머신(Support Vector Machine, SVM)
    - 랜덤 포레스트(Random Forest)
    - 신경망(Neural Network)
- ### 회귀(Regression):
    - 숫자(연속된값)를 예측 하는 지도학습
    - 의사결정트리(Decision Tree)
    - 선형 회귀(Linear Regression)
    - 릿지 회귀(Rige Regression)
    - 라쏘 회귀(Lasso Regression)
    - 엘라스틱 넷(Elastic Net)
    - K-최근접 이웃(K-Nearest Neighbors, KNN)
    - 나이브 베이즈(Naive Bayes)
    - 서포트 벡터 머신(Support Vector Machine, SVM)
    - 랜덤 포레스트(Random Forest)
    - 신경망(Neural Network)
- ### 강화학습
    - 학습하는 시스템이 행동을 실행하고 그 결과에 따른 보상이나 벌점을 받는 방식으로 학습.  
	학습이 계속되면서 가장 큰 보상을 얻기 위한 최상의 전략을 스스로 학습하게 한다.	




### Machine Learning 개발 절차
- Business Understanding
	- 머신러닝 개발을 통해 얻고자 하는 것 파악.
- Data Understanding
	- 데이터 수집
	- 탐색을 통해 데이터 파악
- Data Preparation  
	- 데이터 전처리
- Modeling
	- 머신러닝 모델 선정
	- 모델 학습
- Evaluation
	- 모델 평가
	- 평가 결과에 따라 위 프로세스 반복
- Deployment
    - 평가 결과가 좋으면 실제 업무에 적용


# [사이킷런(scikit-learn)](https://scikit-learn.org/stable)
파이썬 머신러닝 라이브러리가 가장 많이 사용된다. 딥러닝을 제외한 대부분의 머신러닝 알고리즘을 제공한다.

## 사이킷런의 특징
1. 파이썬 기반 다른 머신러닝 라이브러리가 사이킷런 스타일의 API를 지향할 정도로 쉽고 가장 파이썬스런 API 제공
2. 머신러닝 관련 다양한 알고리즘을 제공하며 모든 알고리즘에 일관성있는 사용법을 제공한다.

## scikit-learn(사이킷런) 설치
- `conda install scikit-learn`
- `pip install scikit-learn`

## Estimator와 Transformer
### Estimator (추정기)
- 데이터를 학습하고 예측하는 알고리즘(모델)들을 구현한 클래스들
- fit() 
    - 데이터를 학습하는 메소드
- predict()
	- 예측을 하는 메소드
### Transformer (변환기)
- 데이터 전처리를 하는 클래스들. 데이터 셋의 값의 형태를 변환한다.
- fit()
    - 어떻게 변환할지 학습하는 메소드
- transform()
    - 변환처리 하는 메소드
- fit_transform()
    - fit()과 transform()을 같이 처리하는 메소드	


## 데이터셋 확인하기

### 용어
- **레이블(Label), 타겟(Target)**
    - 결정값, 출력데이터, 종속변수
    - 예측 대상이 되는 값. 지도학습시 학습을 위해 주어지는 정답 데이터
    - 분류의 경우 레이블을 구성하는 고유값들을 **클래스(class)**라고 한다.
- **피쳐(Feature)**
    - 속성, 입력데이터, 독립변수
    - Target이 왜 그런 값을 가지게 되었는지를 설명하는 변수. 
    - Target값을 예측하기 위해 학습해야 하는 값들. 

### scikit-learn 내장 데이터셋 가져오기
- scikit-learn은 머신러닝 모델을 테스트 하기위한 데이터셋을 제공한다.
    - 이런 데이터셋을 Toy dataset이라고 한다.
- 패키지 : sklearn.datasets
- 함수   : load_xxxx()
```
from sklearn.datasets import load_iris
iris = load_iris()
# 위 데이터셋을 판다스 데이터 프레임으로 구성하는
iris_df = pd.DataFrame(iris['data'], columns = iris['feature_names'])
```
### scikit-learn 내장 데이터셋의 구성
- scikit-learn의 dataset은 딕셔너리 형태의 Bunch 클래스 객체이다.
    - keys() 함수로 key값들을 조회
- 구성
    - **target_names**: 예측하려는 값(class)을 가진 문자열 배열
    - **target**: Label(출력데이터)
    - **data**: Feature(입력변수)
    - **feature_names**: 입력변수 각 항목의 이름
    - **DESCR**: 데이터셋에 대한 설명
```python
example) iris dataset
# 1. import
from sklearn.tree import DecisionTreeClassifier
# 2. 모델생성
tree = DecisionTreeClassifier()
# 3. 모델을 학습
tree.fit(iris['data'], iris['target']) # input_data(feature), output_data(label)
# 4. 예측
# sepal(꽃받침)의 길이(length)와 폭(width), petal(꽃잎)의 길이와 폭, 예측할 값을 전달.
my_iris = [
    [5,3.5,1.4,0.25],
    [6,7,1.5,2.3],
    [2,3,5,7]
]
pred = tree.predict(my_iris)
iris['target_names'][pred]
```

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















