# EDA(탐색적 데이터 분석)
> **데이터 타입** : 수치형(연속형, 이산형), 문자형, 범주형(순서형, 명목형), 불리언형  
> **기술 통계** : 평균Mean, 중앙값Median, 최빈값Mod  
> **EDA 툴 사용** : 가상환경 설정, Sweetviz 설치

## 데이터 타입
- 데이터 : seaborn의 iris 데이터, titanic 데이터
- 컬럼별 데이터 타입 확인
    - iris 데이터
        * 수치형: sepal_length	sepal_width	petal_length	petal_width
        * 범주형: species
    - titanic 데이터
        * 수치형(연속형) : age, fare
        * 수치형(이산형) : sibsp, parch
        * 범주형(순서형) : pclass   
        * 범주형(명목형) : sex, class, name, Ticket, cabin, embarked

## 기술 통계
- 데이터 : titanic
- 데이터가 어떻게 **모여** 있는지 표현하는 통계량
    - 평균(Mean), 중앙값(Median), 최빈값(Mode)
- 데이터가 어떻게 **흩어져** 있는지 표현하는 통계량
    - 최소값(Min), 최대값(Max), 분산(Var), 표준펹차(Std), 사분위수(Quartile), 첨도(Kurtosis), 왜도(Skewness)
- 시각화
```python
# 결측치 있는 경우
    ## 방법1. 결측치 제거
    df.dropna(subset=['age'],inplace=True)
    plt.boxplot(df['age'])

    ## 방법2. 아래처럼 작성
    df['age'].plot(kind='box')

# 결측치 없는 경우
plt.boxplot(df['fare'])
plt.show()
```

## EDA 툴
### 환경구성
- pandas, numpy로 인해 충돌 발생 => 가상환경에서 진행
    - 가상환경 셋팅
        - `python -m venv venv`
    - 가상환경 활성화   
        - `cd venv 후에 Scripts\activate`  
          (`source venv/Scripts/activate` 이건 안되나?)
    - pip 업그레이드
        - `python.exe -m pip install --upgrade pip`

### 툴 설치
- `pip install sweetviz`
- `pip show sweetviz`

### 가상환경용 인터프리터 사용
- F1 누른후 Python : 인터프리터 선택
- 인터프리터 경로 입력후 찾기 선택
- venv\Scripts\python.exe 선택
- 주피터 노트북에서 venv(Python 3.10.11) 커널 잡기

### Sweetviz 사용
- 단일분석
```python
import pandas as pd
import sweetviz as sv
# 경고메세지 끄기
import warnings
warnings.filterwarnings(action='ignore')

df_train = pd.read_csv('./data/train.csv')

my_report = sv.analyze(df_train)
# my_report.show_notebook(layout='widescreen',scale=0.8) #notebook, colab에서 표시하기
my_report.show_html(r'./image/sweet_report.html')
```
- 비교분석
```python
import pandas as pd
import sweetviz as sv

# 경고메세지 끄기
import warnings
warnings.filterwarnings(action='ignore')

df_train = pd.read_csv('./data/train.csv')
df_test = pd.read_csv('./data/test.csv')

compare_report = sv.compare([df_train,'Train'],[df_test,'Test'],target_feat='Survived')
# compare_report.show_notebook(layout='widescreen',scale=0.8) #notebook, colab에서 표시하기
compare_report.show_html(r'./image/sweet_report.html')
```
