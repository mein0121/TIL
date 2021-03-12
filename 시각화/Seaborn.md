# Seaborn
- matplotlib을 기반으로 다양한 테마와 그래프를 제공하는 파이썬 시각화 패키지.-
- http://seaborn.pydata.org/
    - 공식 사이트의 [gallary](http://seaborn.pydata.org/examples/index.html)에 제공하는 다양한 그래프와 예제를 확인 할 수 있다.
- 설치: 아나콘다에는 포함되있다.
```
pip install seaborn
conda install seaborn
```

## rugplot, kdeplot, distplot
- 1차원 연속형 값들의 분포를 시각화 하는 그래프
- rugplot 
	- 각 데이터들의 위치를 보여준다.
	```
	plt.figure(figsize=(5,5))
	sns.rugplot(x=DataFrame['컬럼명'])
	# 컬럼명: 문자열, data : DataFrame객체
	sns.rugplot("컬럼명", data=DataFrame)
	plt.show()
	```
	
- kdeplot
	- 히스토그램을 부드러운 곡선 형태로 표현한다. 
	- KDE(Kernel Density Estimation) : 확률밀도추정
	```
	plt.figure(figsize=(5,5))
	sns.kdeplot(x=DataFrame['컬럼명'])
	sns.kdeplot("컬럼명", data=DataFrame)
	plt.show()
	```
	
- distplot
	- 히스토그램에 kdeplot, rugplot 한번에 그린다.
    - kdeplot은 default로 나오고 rugplot은 default로 안나온다.
	```
	plt.figure(figsize=(5,5))
	sns.distplot(tips['total_bill'],
				 hist=True,
				 kde=True,
				 rug=True)
	# hist가 default로 나온다.
	sns.displot(tips['total_bill'],
             kde=True,
             rug=False)
	```

- boxplot(), violinplot(), swamplot()
	- 연속형 데이터(양적데이터)들의 분포를 확인하는 그래프를 그린다.
	- 범주별로 연속형 데이터의 분포를 비교할 수 있다.
	```
	tips = sns.load_dataset('tips')
	#x(y)축: 분포를 보려는 연속형 값의 컬럼, y(x)축: 그룹을 나누려는 범주형 컬럼
	sns.boxplot(y='total_bill',x='smoker',data=tips)
	# hue='컬럼명': 다른색으로 다시 보여주는것
	# hue: x에나온 컬럼을 hue에 넣은 컬럼기준으로 다시 나눈다.
	sns.boxplot(y='total_bill', x='smoker', hue='day', data=tips)
	```
	
- violin plot
	- boxplot 위에 분포 밀도(kernel density)를 좌우 대칭으로 덮어쓰는 방식으로 데이터의 분포를 표현하므로 boxplot 보다 좀더 정확한 데이터의 분포를 볼 수 있다.
	- 매개변수는 boxplot과 동일	
	```
	sns.violinplot(y='tip',x='day',data=tips) #요일별
	sns.violinplot(y='tip',x='day',hue='smoker',data=tips) # 요일-흡연여부
	```

- swarmplot
	- 실제 값들을 점으로 찍어 준다. 
	- boxplot이나 violin plot의 보안해주는 역할로 쓰인다.
	- swarmplot은 가운데 분류를 기준으로 분포시키는데 실제 값이 있는 위치에 점을 찍으므로 좀더 정확하게 값이 어디에 있는지 알 수 있다.
	```
	sns.boxplot(y='tip', data=tips)
	sns.swarmplot(y='tip', data=tips, color='k')
	
	sns.boxplot(y='tip',x='day',hue='smoker',data=tips)
	sns.swarmplot(y='tip',x='day',hue='smoker',data=tips)
	```
	
- countplot() 
	- 막대그래프(bar plot)을 그리는 함수
	- 범주형 변수의 고유값의 개수를 표시
	- matplotlib의 bar()
	`sns.countplot(x='day', hue='sex',data=tips)`
	
## scatterplot, lmplot, jointplot, pairplot
- 산점도를 그린다.
- scatterplot
	- 팔레트 - https://seaborn.pydata.org/tutorial/color_palettes.html#palette-tutorial
	```
	# total_bill과 tip의 상관관계를 성별로 나눠서 확인
	sns.scatterplot(x='total_bill',y='tip',hue='sex', data=tips, alpha=0.5)
	# colormap지정: matplot/pandas = cmap, seaborn = palette
	sns.scatterplot(x='total_bill',y='tip',hue='sex', data=tips, alpha=0.5, palette='cool')
	```
	
- lmplot()
	- linear model
	- 선형회귀 적합선을 포함한 산점도를 그린다.
	
- jointplot()
	- scatter plot 과 각 변수의 히스토그램을 같이 그린다.
	- pandas **DataFrame**만 사용할 수 있다.

- paireplot
	- 다변수(다차원) 데이터들 간의 산점도를 보여준다. 
	- 데이터프레임을 인수로 받아 그리드(grid) 형태로 각 변수간의 산점도를 그린다. 같은 변수가 만나는 대각선 영역에는 해당 데이터의 히스토그램을 그린다.
	- 문자열 컬럼은 제외, 컬럼 미지정시 숫자형 컬럼 전체 이용.
	```
	sns.pairplot(data=tips[['total_bill','tip']])
	sns.pairplot(data=tips)
	```
	
- heatmap()
	- 값들에 비례해서 색깔을 다르게 해 2차원 자료로 시각화
	```
	# annot: 수치표시, cmap: 컬러변경.
	sns.heatmap(tips.corr(), annot=True, cmap='Blues')
	```
- lineplot
	- 선그래프
	- 시간의 흐름에 따른 값의 변화를 보여주는데 유용하다. (시계열 데이터)










