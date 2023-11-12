# Matplotlib
> 폰트설정  
> 기본구조와 사용  
> 시각화 : hist, boxplot, heatmap

## 폰트 설정
**pass**

## 기본구조
```python
plt.plot([10, 20, 30, 40])

y = [10, 20, 30, 40]
plt.plot(y)

x = [1,2,3,4]
y = [12, 43, 25, 15]
plt.plot(x,y)
```
- 제목 + 라인스타일/색 + label 이름 지정
```python
plt.title('color') # 제목 설정
plt.plot([10, 20, 30, 40], color ='skyblue',linestyle(ls도가능) ='--', label ='skyblue') # 하늘색 그래프
plt.plot([40, 30, 20, 10], 'pink', label ='pink') # 분홍색 그래프
plt.legend() # 범례 표시
plt.show()
```
- label 마커 지정
```python
plt.title('marker') # 제목 설정

plt.plot([10, 20, 30, 40], 'r.', label ='circle') # 빨간색 원형 마커 그래프
plt.plot([40, 30, 20, 10], 'g^', label ='triangle up')

# 초록색 삼각형 마커 그래프
plt.legend() # 범례 표시
plt.show()
```

- x축, y축 범위 지정
```python
y = [10, 20, 30, 40]
plt.plot(y)
plt.xlim((-5,5)) #축 범위 지정
plt.ylim((-10,60)) #축 범위 지정
plt.grid(True)
plt.show()
```

- 여러개의 그래프
```python
import numpy as np
import matplotlib.pyplot as plt

x1 = np.arange(1,11)
y1 = np.exp(-x1)

plt.subplot(2, 1, 1)     # nrows=2, ncols=1, index=1 => 2행 1열
plt.plot(x1, y1, 'o-')
plt.title('1nd Graph')

plt.subplot(2, 1, 2)     # nrows=2, ncols=1, index=2
plt.plot(x1, y1, '.-')
plt.title('2nd Graph')

plt.tight_layout()  # 전체 레이아웃을 조정
plt.show()


# for문으로 => 1행 5열
plt.figure(figsize=(8,4))

for i in range(5):
    x1 = np.arange(1,11)
    y1 = np.exp(-x1)

    plt.subplot(1, 5, i+1)    
    plt.plot(x1, y1, 'o-')
    plt.title(f'{i+1}nd Graph')

plt.tight_layout()
plt.show()


# 여러개의 그래프 꾸미기
x = np.arange(1, 5)     # [1, 2, 3, 4]

fig, ax = plt.subplots(3, 2, sharex=True, sharey=True, squeeze=True)
ax[0][0].plot(x, np.sqrt(x), 'gray', linewidth=3)
ax[0][1].plot(x, x, 'g^-', markersize=10)
ax[1][0].plot(x, -x+5, 'ro--')
ax[1][1].plot(x, np.sqrt(-x+5), 'b.-.')
```

## 시각화
### Hist
- 분포빈도로 1차원의 수치범주에 해당 
- 주의 : 수치형 데이터의 경우 bins(구간)를 어느 정도로 나누느냐에 따라 해석이 달라질 수 있으므로 구간을 여러개 설정해보고, 적절한 구간을 찾아야 한다.  
`df['지역'].hist()`  
`df['분양가'].hist(bins=50)`   

### Boxplot
- 데이터의 분포를 나타내며 1차원의 수치에 해당
- 6개의 값 확인 가능 : 최소/최대값, 제1/2/3사분위수, 이상치
- 기본구조
```python
- 하나의 컬럼
y = df['분양가'].values
plt.boxplot(y) 
plt.show()

- 여러개의 컬럼
# 방법1
iris.boxplot()

# 방법2
x = iris[['sepal_length','sepal_width','petal_length','petal_width']].values
plt.boxplot(x) 
plt.show()
```
- IQR 값 구하기 ( IQR : Q3(제3사분위수)-Q1(제1사분위수) )  
`iris['sepal_width'].quantile(0.75) - iris['sepal_width'].quantile(0.25)`
- Max 값 구하기 (Q3 + 1.5*IQR)  
`max = iris['sepal_width'].quantile(0.75) + 1.5*(iris['sepal_width'].quantile(0.75) - iris['sepal_width'].quantile(0.25))`
- Outliar의 관측치 구하기  
`iris[iris['sepal_width']>max]`

## Heatmap
- 특정 영역의 패턴을 보여주기 위한 목적 / 변수간의 관계 파악
- 양의 상관관계 / 0(관계X) / 음의 상관관계
- 피어슨 상관계수  
`titanic.corr(numeric_only=True)`

- 타이타닉 데이터에서 생존(Survived)과 가장 상관도가 높은 피처는?   
`sns.heatmap(titanic.corr(numeric_only=True),annot=True, annot_kws={'size':7},\
                    cmap=plt.cm.RdYlBu_r, fmt=".2f", linewidth=.5,vmin=-1.0,square=False)`

