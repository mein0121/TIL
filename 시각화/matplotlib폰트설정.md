# matplotlib 한글처리
matplotlib에 설정되있는 폰트가 한글을 지원하지 않기 때문에 그래프의 한글이 깨져서 나온다.

## 설정방법
1. 설정파일을 변경한다. 
    - 한번만 하면 된다.
2. 프로그램상에서 변경한다.
    - 프로그램이 로딩 될때마다 (노트북 파일이나 파이썬 스크립트 실행시마다) 코드를 실행해야 한다.
    - 전체 설정에서 변경하고 싶은 것을 재설정한다.

## OS에 설치된 폰트명 조회

- 폰트 cache 파일을 삭제 한다.
```
import matplotlib as mpl
import matplotlib.font_manager as fm

# cache 파일 조회
# 다음 실행 결과로 나온 경로의 파일을 삭제한다. 
mpl.get_cachedir()

#전체 폰트 조회
for f in fm.fontManager.ttflist:
    print(f.name, f.fname, sep="::::")  # 폰트이름, 폰트파일경로
	
#원하는 폰트명 찾기
#전체 폰트중 mal이 들어간 폰트 찾기.
[(f.name,f.fname) for f in fm.fontManager.ttflist if 'malg' in f.name.lower()]  
#mac : AppleGothic
# 설정시 폰트 이름을 사용
### 함수를 이용해 설정
- `matplotlib.rcParam['설정'] = 값` 으로 변경
import matplotlib as mpl
from matplotlib import font_manager as fm

# 한글 폰트 설정
font_name = fm.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()

mpl.rcParams["font.family"] = font_name
mpl.rcParams["font.size"] = 15
mpl.rcParams['xtick.labelsize'] = 12
mpl.rcParams['ytick.labelsize'] = 12
mpl.rcParams['axes.labelsize'] = 15
# tick의 음수기호 '-' 가 깨지는 것 처리
mpl.rcParams['axes.unicode_minus'] = False
```
## 폰트등 환경 설정하기 
### 설정파일 변경
- 한번만 하면 되므로 편리하다.

- 설정파일 경로찾기: `matplotlib.matplotlib_fname()` matplotlib 관련 전역 설정들을 찾아 바꿔준다.
- 폰트 관련 설정
```
font.family:Malgun Gothic
font.size:12
xtick.labelsize:12
ytick.labelsize:12 
axes.labelsize:12  
axes.titlesize:20
axes.unicode_minus:False
```
```
mpl.matplotlib_fname()
```