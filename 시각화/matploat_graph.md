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

### alpha
- 선그래프의 투명도. 두 그래프 선이 겹칠때 사용 가능.
- alpha:투명도 - 0:투명 ~ 1:불투명
`plt.plot(df['df1'],df['df2'],label='label_1',alpha=0.5)`
### ticks 설정
- 틱을 원하는 범위로 설정하기.
- rotation: ticks의 각도 설정.
```
plt.xticks(df['df_1'], rotation=45) # df에 설정된 숫자로 설정 
plt.yticks(range(0,5)) # range로 설정가능.
```
### twin x, twin y
- 두 그래프의 값의 차이가 클때, 하나의 축만 공유하고, 다른 x축 또는 y축을 설정할때
- twinx, y 사용시 color와 legend 위치도 공유, 서로 다른 color와 legend위치 지정필요.

### legend box 위치 지정
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