# Pandas 시각화
판다시 자체적으로 matplotlib 를 기반으로 한 시각화기능을 지원한다.    
Series나 DataFrame에 plot() 함수나 plot accessor를 사용한다.
- https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html

## plot() 
- kind 매개변수에 지정한 값에 따라 다양한 그래프를 그릴 수 있다.
- kind : 그래프 종류 지정
    - 'line' : line plot (default)
    - 'bar' : vertical bar plot
    - 'barh' : horizontal bar plot
    - 'hist' : histogram
    - 'box' : boxplot
    - 'kde' : Kernel Density Estimation plot
    - 'pie' : pie plot
    - 'scatter' : scatter plot
	`data.plot(kind='line'..[추가설정])`
	`data.plot.line(..[추가설정])`
- matplotlib을 사용하여 추가 설정 가능.

## pandas pivot_table, grouby를 이용한 시각화
- DF의 index : ticks- 1차 그룹, columns- 각 ticks마다 나눠져서 나옴. - 2차 그룹.
```
# 요일(day)-성별(sex) 손님의 총수(size)
# pivot_table을 이용한 시각화
df.pivot_table(index='day', columns='sex', values='size', aggfunc='sum').plot.bar()
# grouby를 이용한 시각화
df.groupby(['day','sex'])['size'].sum().plot.bar()
```


## 판다스에서 datetime 사용
### python datetime 
- https://docs.python.org/ko/3/library/datetime.html#strftime-strptime-behavior

### accessor
- plot = 그래프를 이용하는 기능을 제공
- str = 문자열을 이용하는 기능을 제공
- dt - datatime 타입의 값들을 처리하는 기능을 제공
	- day: 일, hour: 시간, minute:분, second:초
	- week: 주
	- dayofweek: 요일(0:월 6:일)
	- dayofyear
	- isocalendar() - (년,주차,요일) 1:월, 7:일
```
df['df'].dt.day, df['day'].dt.hour, df['day'].dt.minute, df['day'].dt.second
```

### datetime 타입의 index를 생성
- 규칙적으로 증가/감소 하는 datetime값을 가지는 index를 생성
`pd.date_range(시작날짜, freq='변화규칙', periods='개수')`
- 2000/1/1 부터 1개월씩 증가하는 날짜 5개 생성. 기준이 마지막날
 `pd.date_range('2000/1/1', freq='M', periods=5)`
- freq - 간격을 지정(Y:YEAR, M:MONTH, D: DAY, H:HOUR, T:MINUTE, S: SECOND)
- Y, M, H, T, S: 처음날짜
- YS, MS, HS, TS, SS: 마지막날짜
- 문자앞에 정수: 간격
	`pd.date_range('2000/1/1', freq='5Y', periods=5)`


 
