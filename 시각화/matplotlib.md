# Matplotlib
- 데이터의 시각화를 위한 파이썬 패키지
- 2차원 그래프를 위한 패키지이나 확장 API들을 이용해 3D 그래프등 다양한 형식의 시각화를 지원
- 파이썬 기반의 다른 시각화 패키지의 기본이 된다.
    - Seabone, Pandas 등이 Matplotlib를 기반으로 사용한다.

## 1.1 장점
- 동작하는 OS를 가리지 않는다.
- MATLAB과 유사한 사용자 인터페이스를 가진다.
- 그래프에 대한 상세한 설정을 할 수 있다.
- 다양한 출력 형식으로 저장할 수 있다.

## 1.2 matplotlib 그래프 구성요소
![image](/images/matplot01.png)

- figure
    - 전체 그래프가 위치할 기본 틀
    - https://matplotlib.org/api/_as_gen/matplotlib.pyplot.figure.html
- axes(subplot)
    - 하나의 그래프를 그리기 위한 공간
        - figure에 한개 이상의 axes(subplot)로 구성해서 각 axes에 그래프를 그린다.
    - https://matplotlib.org/api/axes_api.html
- axis 
    - 축 (x축, y축)
    - axis label (x, y) : 축의 레이블(설명)
- ticks : 축선의 표시 
    - Major tick
    - Minor tick
- title : 플롯 제목   
- legend (범례)
    - 하나의 axes내에 여러 그래프를 그린 경우 그것에 대한 설명
	
# 2. 그래프 그리기
1. matplotlib.pyplot 모듈을 import
    - 2차원 그래프(axis가 두개인 그래프)를 그리기위한 함수를 제공하는 모듈
    - 별칭(alias) 로 plt를 관례적으로 사용한다.
    - `import matplotlib.pyplot as plt`

2. 그래프를 그린다.
    - 2가지 방식
        - pyplot 모듈을 이용해 그린다.
        - Figure와 Axes 객체를 생성해서 그린다.

3. 그래프에 필요한 설정을 한다.

4. 화면에 그린다.
    - 지연 랜더링(Deferred rendering) 메카니즘
    - 마지막에 `pyplot.show()` 호출 시 그래프를 그린다.
        - 주피터 노트북 맨 마지막 코드에 `;`를 붙이는 것으로 대체할 수 있다.
		
## 3.1 pyplot 모듈을 이용해 그리기
- pyplot 모듈이 그래프 그리는 함수와 Axes(Subplot) 설정 관련 함수를 제공
```
ex)
import matplotlib.pyplot as plt

# figure의 크기
plt.figure(figsize=(10,10)) # 가로,세로(길이) - inch

# 첫번째 subplot지정
plt.subplot(2,1,1) #그래프를 그릴 axes(subplot)을 지정
# 그래프 그리기
plt.plot([1,2,3],[10,20,30])
# 추가 설정
plt.title("첫번째")
plt.xlabel("X축값")
plt.ylabel("Y축값")

# 두번째 subplot지정
plt.subplot(2,1,2)
# 그래프그리기
plt.scatter([1,2,3],[10,20,30])
# 추가설정
plt.title('두번째')
plt.xlabel("X축값")
plt.ylabel("Y축값")

plt.show()
```