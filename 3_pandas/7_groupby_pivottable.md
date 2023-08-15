## apply() - 함수적용
- apply()는 데이터 전처리시 굉장히 많이 활용하는 기능
- 좀 더 복잡한 logic을 컬럼 혹은 DataFrame에 적용하고자 할 때 사용

### 함수(Function) 정의
- who 컬럼을 man은 남자, woman은 여자, child는 아이로 변경하고자 apply 활용하여 해결
```python
def transform_who(x):
    if x == 'man':
        return '남자'
    elif x == 'woman':
        return '여자'
    else:
        return '아이'
```
```python
df['who'].apply(transform_who)
```
- 컬럼끼리 함수사용하여 계산
```python
def transform_who(x):
    return x['fare'] / x['age']
```
```python
df.apply(transform_who, axis=1)
```

### apply() - lambda 함수
- 간단한 logic은 함수를 굳이 정의하지 않고, lambda 함수로 쉽게 해결할 수 있습니다.
- 0:사망, 1:생존으로 변경
- lambda ver
```python
df['survived'].apply(lambda x: '생존' if x == 1 else '사망')
```
- function Ver
```python
def survived(x):
    if x == 0:
        return '사망'
    else:
        return '생존'
df['survived'].apply(survived)
```
## groupby() - 그룹
- 데이터를 특정 기준으로 그룹핑할 때 활용
- groupby()를 사용할 때는 반드시 aggregate하는 통계함수와 일반적으로 같이 적용함.
- 타이타닉 호의 생존자와 사망자를 성별 기준으로 그룹핑하여 평균내기
`df.groupby('sex')['survived'].mean()`

### 2개 이상의 컬럼으로 그룹
- 2개 이상의 컬럼으로 그룹핑할 때도 list로 묶어서 지정
- 성별, 좌석등급 별 통계
`df.groupby(['sex', 'pclass'])['survived'].mean()`
- 예쁘게 출력하려면 pd.DataFrame()으로 감싸주거나, survived 컬럼을 []로 한 번 더 감싸주면 됨.
`pd.DataFrame(df.groupby(['sex', 'pclass'])['survived'].mean())`
`df.groupby(['sex', 'pclass'])[['survived']].mean()`

### reset_index() : 인덱스 초기화
- reset_index() : 그룹핑된 데이터프레임의 index를 초기화하여 새로운 데이터프레임을 생성
`df.groupby(['sex', 'pclass'])['survived'].mean().reset_index()`

### 다중 컬럼에 대한 결과 도출
`df.groupby(['sex', 'pclass'])[['survived', 'age']].mean()`

### 다중 통계 함수 적용
`df.groupby(['sex', 'pclass'])[['survived', 'age']].agg(['mean', 'sum'])`

### numpy의 통계 함수도 적용 가능
`df.groupby(['sex', 'pclass'])[['survived', 'age']].agg([np.mean, np.sum])`

## pivot_table()
- 피벗테이블은 groupby()와도 동작이 유사
- 기본 동작 원리는 index, columns, values를 지정하여 피벗한다.
### 1개 그룹에 대한 단일 컬럼 결과
- index에 그룹을 표기
`df.pivot_table(index='who', values='survived')`
- columns에 그룹을 표기
`df.pivot_table(columns='who', values='survived')`

### 다중 그룹에 대한 단일 컬럼 결과
`df.pivot_table(index=['who', 'pclass'], values='survived')`

### index에 컬럼을 중첩하지 않고 행과 열로 펼친 결과
`df.pivot_table(index='who', columns='pclass', values='survived')`

### 다중 통계함수 적용
`df.pivot_table(index='who', columns='pclass', values='survived', aggfunc=['sum', 'mean'])`