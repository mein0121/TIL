# 1. 선 그래프 (Line Graph) 그리기
## 1.1 선 그래프(꺽은선 그래프)
- 점과 점을 선으로 연결한 그래프
- 시간의 흐름에 따른 변화를 표현할 때 많이 사용한다. (시계열)
- `plot([x], y)` 
    - 1번인수 : x값(생략가능), 2번인수 y값
    - 인수가 하나인 경우 y 축의 값으로 설정되고 X값은 (0 ~ len(y)-1) 범위로 지정된다.
    - x,y 의 인수는 리스트 형태의 객체들을 넣는다.
        - 리스트
        - 튜플
        - numpy 배열 (ndarray)
        - 판다스 Series
    - x와 y의 size는 같아야 한다.
- 하나의 axes(subplot)에 여러 개의 선 그리기
    - 같은 axes에 plot()를 여러번 실행한다.
	
## 1.2 선 스타일
- https://matplotlib.org/gallery/lines_bars_and_markers/line_styles_reference.html
```python
plt.plot(x,x+3, linewidth=5) # 선 꿇기 조절
plt.plot(x, x+2, linestyle='--') 
plt.plot(x, x+1, linestyle='-.')
plt.plot(x, x, linestyle=':')
```
## 1.3 설정
- alpha
	- 선그래프의 투명도. 두 그래프 선이 겹칠때 사용 가능.
	- alpha:투명도 - 0:투명 ~ 1:불투명
	`plt.plot(df['df1'],df['df2'],label='label_1',alpha=0.5)`
- ticks 설정
	- 틱을 원하는 범위로 설정하기.
	- rotation: ticks의 각도 설정.
	```
	plt.xticks(df['df_1'], rotation=45) # df에 설정된 숫자로 설정 
	plt.yticks(range(0,5)) # range로 설정가능.
	```
- twin x, twin y
	- 두 그래프의 값의 차이가 클때, 하나의 축만 공유하고, 다른 x축 또는 y축을 설정할때
	- twinx, y 사용시 color와 legend 위치도 공유, 서로 다른 color와 legend위치 지정필요.

- legend box 위치 지정
	- loc="수직방향위치 수평방향위치"
		- axes box내에 legend box를 위치시킬때 사용
		- 수직방향위치: lower, upper, center
		- 수평방향위치: left, right, center
		- default: best
	- bbox_to_anchor, loc
		- axes box 밖에 legend box를 위치시킬때 사용.
		- bbox_to_anchor: 0 ~ 1사이 실수. legend box를 axes box 기준 어디에둘것인지를 지정.(x축위치,y축위치)
		- loc: **legend box를 기준**으로 anchor가 어느부분에 있을 것인지.
	```
	ex)
	fig, ax1 = plt.subplots(figsize=(15,5)) #행, 열개수를 생략 -> axes(subplot) 1개
	ax2 = ax1.twinx() # ax와 x축을 공유하는 쌍둥이 axes

	# color가 같다. color를 다르게 설정 필요.
	ax1.plot(df['공유축'],df['다른축1'], label='label1', color='r')
	ax2.plot(df['공유축'],df['다른축2'], label='label2', color='b')

	# legend위치가 같다, legend위치 설정 필요. 
	ax1.legend(loc="upper left")
	ax2.legend(loc="upper right")
	plt.show()

	# 두 legend의 위치는 같다.
	bbox_to_anchor 예제1)
	plt.legend(loc="upper right")
	plt.legend(bbox_to_anchor=(1,1),loc='upper right')
	bbox_to_anchor 예제2)
	plt.legend(loc="lower right")
	plt.legend(bbox_to_anchor=(1,0),loc='lower right')
	```
	- fontsize
		- 폰트사이즈
	- fancybox, shadow
		- 표미기
	- ncol
		- regend 항목이 많을때 column으로 나눌수있다.
	`ax1.legend(loc="upper left", fontsize=15, fancybox=True,shadow=True, ncol=2)`

# 2. 산점도 (Scatter Plot) 그리기
### 2.1 산점도(산포도)
- X와 Y축을 가지는 좌표평면상 관측값들을 점을 찍어 표시하는 그래프
- 변수(Feature)간의 상관성이나 관측값들 간의 군집 분류를 확인할 수 있다.
- `scatter()` 메소드 사용
    - 1번인수 : x값, 2번인수 y값
    - x와 y값들을 모두 매개변수로 전달해야 한다.
    - x,y 의 인수는 스칼라 실수나 리스트 형태의 객체들을 넣는다.
        - 리스트
        - 튜플
        - numpy 배열 (ndarray)
        - 판다스 Series
    - x와 y의 원소의 수는 같아야 한다.

### 2.2 설정
- marker (마커)
    - marker란 점의 모양을 말하며 미리정의된 값으로 변경할 수있다.
    - scatter() 메소드의 marker 매개변수를 이용해 변경한다. 
    - https://matplotlib.org/api/markers_api.html
	```
	# market: 점모양, s: size, color = color
	plt.scatter(x = np.random.randint(1,5,30),
				y = np.random.randint(1,5,30),
			    marker = 'x', 
				s = 100,
			    color = 'r')
	# 선그래프에 산점도 그리기			
	plt.plot([1,2,3,4,5],[10,20,30,40,50], marker='o', markersize=10)
	```
- s
    - 마커의 크기
- alpha    
    - 하나의 마커에 대한 투명도
    - 0 ~ 1 사이 실수를 지정 (default 1)

- 상관계수
    - 두 변수(컬럼) 간의 상관관계를 계산한 값.
    - 양의 상관관계: 변수 하나의 값이 증가할 때 다른 하나도 같이 증가.
        - 0 ~ 1 (양수)
    - 음의 상관관계: 변수 하난의 값이 증가할 때 다른 하나는 감소.
        - -1 ~ 0 (음수)
    - numpy.corrcoef(변수, 변수) - 변수는 array_like(배열, list, Series)
    - pd.corr()
        - -1 ~ 1
        - 1 ~ 0.7 : 아주 강한 상관관계
        - 0.7 ~ 0.3 : 강한 상관관계
        - 0.3 ~ 0.1 : 약한 상관관계
        - 0.1 ~ 0: 관계없다
#### 상관관계를 heatmap으로 시각화
- heatmap: 값의 차이를 색농도의 차이로 표현
```
plt.figure(figsize=(10,10))
plt.imshow(df.corr(), cmap='Blues', vmin=-1, vmax=1) # vmin최소값, vmax최대값.

# ticks 설정.
plt.yticks(ticks=range(df.columns.size),labels=df.columns) # 숫자대신 데이터프레임의 문자열로 표현
plt.xticks(ticks=range(df.columns.size),labels=df.columns, rotation= -45) # rotation으로 문자열 회전.

plt.colorbar() # 어떤색이 큰값인지 바형태로 표현.
plt.show()
```

# 3. 막대그래프 (Bar plot) 그리기
## 3.1 막대그래프(Bar plot)

- 수량을 막대 형식으로 나타낸 그래프
- axes.bar(x, height) 메소드 사용
    - x : x값, height:  막대 높이
        - X는 분류값, height는 개수
- axes.barh(y, width) 메소드
    - 수평막대 그래프
    - 1번인수: y값, 2번인수: 막대 너비   
```
fig, axes = plt.subplots(2,1,figsize=(10,15))
axes[0].bar(list1, list2, width=0.1) # 그래프 width 조절 0~1, default는 0.8, 1:두꺼움, 0: 얇음
axes[0].set_title('수직막대그래프')
axes[0].set_xlabel('list1')
axes[0].set_ylabel('list2')
axes[0].grid(True)

axes[1].barh(list1,list2, height=0.1)# bar그래프의 width와 같음.
axes[1].set_title('수평막대그래프')
axes[1].set_xlabel('list1')
axes[1].set_ylabel('list2')
axes[1].grid(True)
```
	
# 4. 파이차트 그리기
## 4.1 파이차트
- 각 범주(Category)가 데이터에서 차지하는 비율을 나타내는데 사용
- `pie(x, labels)` 이용
    - x: 값 (값들을 100을 기준으로 비율을 계산해 크기 설정)
    - labels : 값들의 label
    - autopct: 조각내에 표시될 비율의 문자열 형식. '%fmt문자' 
        - fmt문자: f(실수), d(정수), %% (%)
```
plt.figure(figsize=(10,10))
plt.pie(amount,labels=labels, autopct="%.2f%%", # autopct: 비율표현 
       explode=[0,0.2,0,0,0.2], shadow=True) # explode:특정 값을 강조할때, 숫자만큼 빼준다.
                                             # shadow: 차트 꾸미기, 입체적으로 보이게한다.
plt.show()
```		
		
	