# 모델 평가
## 분류와 회귀의 평가방법
- sklearn.metrics 모듈을 통해 제공

### 분류 평가 지표
1. 정확도 (Accuracy)
	- 정확도(𝐴𝑐𝑐𝑢𝑟𝑎𝑐𝑦)=맞게예측한건수 / 전체예측건수
1. 정밀도 (Precision)
    - Positive(양성)으로 예측 한 것 중 실제 Positive(양성)인 비율
    - **PPV**(Positive Predictive Value) 라고도 한다.
1. 재현률/민감도 (Recall/Sensitivity)
    - 실제 Positive(양성)인 것 중에 Positive(양성)로 예측 한 것의 비율
    - **TPR**(True Positive Rate) 이라고도 한다.
1. F1점수 (F1 Score)
    - 정밀도와 재현율의 조화평균 점수
	- recall과 precision이 비슷할 수록 높은 값을 가지게 된다
1. PR Curve, AP
1. ROC, AUC

#### 기타
1. Specificity(특이도)
    - 실제 Negative(음성)인 것들 중 Negative(음성)으로 맞게 예측 한 것의 비율
    - TNR(True Negative Rate) 라고도 한다.
1. Fall out(위양성률)
    - 실제 Negative(음성)인 것들 중 Positive(양성)으로 잘못 예측한 것의 비율. `1 - 특이도`
    - **FPR** (False Positive Rate) 라고도 한다.
	- 𝐹𝑎𝑙𝑙−𝑂𝑢𝑡(𝐹𝑃𝐹) = 𝐹𝑃 / 𝑇𝑁+𝐹𝑃
	
### 회귀 평가방법
1. MSE (Mean Squared Error)
1. RMSE (Root Mean Squared Error)
3. 𝑅2(결정계수)

## 혼동 행렬(Confusion Marix)
- 분류의 평가지표의 기준으로 사용된다.
- 혼동행렬을 이용해 다양한 평가지표(정확도, 재현률, 정밀도, F1 점수, AUC 점수)를 계산할 수 있다.
- 함수: confusion_matrix(정답, 모델예측값)
- 결과의 0번축: 실제(Ground Truth) class, 1번 축: 예측 class
![image](/image/predicted.png)
- TP(True Positive) - 양성으로 예측했는데 맞은 개수
- TN(True Negative) - 음성으로 예측했는데 맞은 개수
- FP(False Positive) - 양성으로 예측했는데 틀린 개수 (음성을 양성으로 예측)
- FN(False Negative) - 음성으로 예측했는데 틀린 개수 (양성을 음성으로 예측)

## 각 평가 지표 계산 함수
- sklearn.metrics 모듈
`from sklearn.metrics import confusion_matrix, plot_confusion_matrix, recall_score, precision_score, f1_score, accuracy_score`

- ### confusion_matrix(y 실제값, y 예측값)
    - 혼돈 행렬 반환
- ### recall_score(y 실제값, y 예측값) 
  - Recall(재현율) 점수 반환 (Positive 중 Positive로 예측한 비율 (TPR))
- ### precision_score(y 실제값, y 예측값)
  - Precision(정밀도) 점수 반환 (Positive로 예측한 것 중 Positive인 것의 비율 (PPV))
- ### f1_score(y 실제값, y 예측값)
    - F1 점수 반환 (recall과 precision의 조화 평균값)
- ### classification_report(y 실제값, y 예측값) 
    - 클래스 별로 recall, precision, f1 점수와 accuracy를 종합해서 보여준다.
`from sklearn.metrics import classification_report`

## 재현율과 정밀도의 관계
#### 재현율이 더 중요한 경우
- 실제 Positive 데이터를 Negative 로 잘못 판단하면 업무상 큰 영향이 있는 경우. 
- FN(False Negative)를 낮추는데 촛점을 맞춘다.
- ex) 암환자 판정 모델, 보험사기 적발 모델

#### 정밀도가 더 중요한 경우
- 실제 Negative 데이터를 Positive 로 잘못 판단하면 업무상 큰 영향이 있는 경우.
- FP(False Positive)를 낮추는데 초점을 맞춘다.
- ex) 스팸메일 판정 모델

## 임계값(Threshold) 변경을 통한 재현율, 정밀도 변환
- 임계값 : 모델이 분류의 답을 결정할 때 기준값
- **임계값을 낮추면 재현율은 올라가고 정밀도는 낮아진다.**
- **임계값을 높이면 재현율은 낮아지고 정밀도는 올라간다.**
- 임계값을 변화시켰을때 **재현율과 정밀도는 음의 상관관계를 가진다.**

### Binarizer - 임계값 변경
- Transformer로 양성 여부를 선택하는 임계값을 변경할 수 있다.
```
from sklearn.preprocessing import Binarizer
# 머신러닝 모델에 적용
pos_proba = tree.predict_proba(X_test) #positive들의 확률 
binarizer = Binarizer(threshold=0.5) # 임계값을 지정
binarizer.fit(pos_proba)
predict = binarizer.transform(pos_proba)[:, 1]
```

## PR Curve(Precision Recall Curve-정밀도 재현율 곡선)와 AP Score(Average Precision Score)
![image](/image/prcAndaps.png)
- 임계값이 0 → 1 변화할때 재현율(recall)과 정밀도(precision)의 변화를 이용한 평가 지표
- AP Score
    - PR Curve의 선아래 면적을 계산한 값으로 높을 수록 성능이 우수하다.

## ROC curve(Receiver Operating Characteristic Curve)와 AUC(Area Under the Curve) score
![image](/image/rocAndauc.png)
- **FPR(False Positive Rate-위양성율)**
    - 위양성율 (fall-out)
    - 실제 음성중 양성으로 잘못 예측 한 비율
	- 𝐹𝑃 / 𝑇𝑁+𝐹𝑃
- **TPR(True Positive Rate-재현율/민감도)** 
    - 재현율(recall)
    - 실제 양성중 양성으로 맞게 예측한 비율
	- 𝑇𝑃 / 𝐹𝑁+𝑇𝑃
- **ROC 곡선**
    - 2진 분류의 모델 성능 평가 지표 중 하나.
    - 불균형 데이터셋을 평가할 때 사용.
- **AUC**
    - ROC 곡선 아래쪽 면적
    - 0 ~ 1 사이 실수로 나오며 클수록 좋다.
```
from sklearn.metrics import precision_recall_curve, plot_precision_recall_curve, average_precision_score

pos_proba = tree.predict_proba(X_test)[:,1]
precisions, recalls, thresholds = precision_recall_curve(y_test, pos_proba) # y, positive_예측확률

#그래프 그리기
ax = plt.gca() # Get Current Axes
plot_precision_recall_curve(tree, # model
                            X_train, # X값
                            y_train, # y값
                            ax=ax)

# APS
average_precision_score(y_test, pos_proba)
```

### ROC, AUC 점수  확인
- roc_curve(y값, 예측확률) : FPR, TPR, Thresholds (임계치)
- roc_auc_score(y값, 예측확률) : AUC 점수 반환
```
from sklearn.metrics import roc_curve, plot_roc_curve, roc_auc_score

pos_proba_tree = tree.predict_proba(X_test)[:,1]
fpr_tree, tpr_tree, threshold_tree = roc_curve(y_test, pos_proba_tree)  # y, pos_예측확률

#그래프 그리기.
ax = plt.gca() # Get Current Axes
plot_roc_curve(tree, # model
			   X_test, # X값
			   y_test, # y값
			   ax=ax)
#plt.plot(fpr_tree, tpr_tree)

#roc_auc_score 확인.
roc_auc_score(y_test, pos_proba_tree)
```








