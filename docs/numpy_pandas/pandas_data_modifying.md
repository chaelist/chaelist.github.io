---
layout: default
title: Pandas 데이터 가공
parent: Numpy & Pandas
nav_order: 3
---

# Pandas 데이터 가공
{: .no_toc }
<br/>

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }


1. TOC
{:toc}
</details>
---


## Pandas DataFrame 변경하기

```python
iphone_df = pd.read_csv('data/iphone.csv', index_col=0)  ## 데이터 출처: codeit
iphone_df
```

<div class="code-example" markdown="1">

|               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
|:--------------|:-----------|-------------:|:---------|:------------|:----------|
| iPhone 7      | 2016-09-16 |          4.7 | 2GB      | iOS 10.0    | No        |
| iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
| iPhone 8      | 2017-09-22 |          4.7 | 2GB      | iOS 11.0    | No        |
| iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
| iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
| iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
| iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

</div>

### 데이터 수정하기

```python
iphone_df.loc['iPhone 8', '메모리'] = '2.5GB'
iphone_df
```

<div class="code-example" markdown="1">

|               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
|:--------------|:-----------|-------------:|:---------|:------------|:----------|
| iPhone 7      | 2016-09-16 |          4.7 | 2GB      | iOS 10.0    | No        |
| iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
| iPhone 8      | 2017-09-22 |          4.7 | 2.5GB      | iOS 11.0    | No        |
| iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
| iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
| iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
| iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

</div>

### 새로운 행/열 추가

1. 행 추가
    ```python
    iphone_df.loc['iPhone XR'] = ['2018-10-26', 6.1, '3GB', 'iOS 12.0.1', 'Yes'] 
    iphone_df
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone 7      | 2016-09-16 |          4.7 | 2GB      | iOS 10.0    | No        |
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
    | iPhone 8      | 2017-09-22 |          4.7 | 2.5GB    | iOS 11.0    | No        |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XR     | 2018-10-26 |          6.1 | 3GB      | iOS 12.0.1  | Yes       |

    </div>

1. 열 추가
    ```python
    iphone_df['제조사'] = 'Apple' 
    iphone_df
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   | 제조사   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|:---------|
    | iPhone 7      | 2016-09-16 |          4.7 | 2GB      | iOS 10.0    | No        | Apple    |
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        | Apple    |
    | iPhone 8      | 2017-09-22 |          4.7 | 2.5GB    | iOS 11.0    | No        | Apple    |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        | Apple    |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       | Apple    |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       | Apple    |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       | Apple    |
    | iPhone XR     | 2018-10-26 |          6.1 | 3GB      | iOS 12.0.1  | Yes       | Apple    |

    </div>

### df.drop()
: 행/열 삭제

1. 행 삭제: `axis=0` 혹은 `axis='index'`
    ```python
    iphone_df.drop('iPhone XR', axis='index', inplace=True)
    # inplace=True라고 해야 원본 df 자체가 변경됨. 
    # inplace=False라고 하면 원본 df는 그대로 있고, 행/열이 삭제된 새로운 df가 결과로 반환됨

    iphone_df
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   | 제조사   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|:---------|
    | iPhone 7      | 2016-09-16 |          4.7 | 2GB      | iOS 10.0    | No        | Apple    |
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        | Apple    |
    | iPhone 8      | 2017-09-22 |          4.7 | 2.5GB    | iOS 11.0    | No        | Apple    |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        | Apple    |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       | Apple    |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       | Apple    |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       | Apple    |

    </div>

1. 열 삭제: `axis=1` 혹은 `axis='columns'`
    ```python
    iphone_df.drop('제조사', axis='columns', inplace=True)
    iphone_df
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone 7      | 2016-09-16 |          4.7 | 2GB      | iOS 10.0    | No        |
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
    | iPhone 8      | 2017-09-22 |          4.7 | 2.5GB    | iOS 11.0    | No        |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

    </div>

1. +) 여러 줄 한 번에 삭제 

    ```python
    iphone_df.drop(['iPhone 7', 'iPhone 8','iPhone X'], axis='index', inplace=True)
    iphone_df
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

    </div>
 
### df.insert()
: 특정 위치에 열을 삽입해주는 함수

```python
iphone_df.insert(loc=3, column='순위', value=np.arange(1, len(iphone_df)+1))
# loc=3이면 4번째 열에 삽입하겠다는 뜻
# column에는 새로 삽입할 열의 이름을 입력
# value에는 새로 삽입할 열에 들어가는 값들을 입력 (list나 np array 형태)
## 이 경우에는, 1부터 해당 df의 열 개수만큼 증가하게 1,2,3,... 이런 식의 열을 추가

iphone_df
```

<div class="code-example" markdown="1">

|               | 출시일     |   디스플레이 | 메모리   |   순위 | 출시 버전   | Face ID   |
|:--------------|:-----------|-------------:|:---------|-------:|:------------|:----------|
| iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      |      1 | iOS 10.0    | No        |
| iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      |      2 | iOS 11.0    | No        |
| iPhone XS     | 2018-09-21 |          5.8 | 4GB      |      3 | iOS 12.0    | Yes       |
| iPhone XS Max | 2018-09-21 |          6.5 | 4GB      |      4 | iOS 12.0    | Yes       |

</div>

### df.transpose()
(혹은 **df.T**): 행과 열 바꾸기 (numpy에 transpose하는 것과 동일)

```python
iphone_df.transpose()   # iphone_df.T라고 해도 동일 
```

<div class="code-example" markdown="1">

|            | iPhone 7 Plus   | iPhone 8 Plus   | iPhone XS   | iPhone XS Max   |
|:-----------|:----------------|:----------------|:------------|:----------------|
| 출시일     | 2016-09-16      | 2017-09-22      | 2018-09-21  | 2018-09-21      |
| 디스플레이 | 5.5             | 5.5             | 5.8         | 6.5             |
| 메모리     | 3GB             | 3GB             | 4GB         | 4GB             |
| 순위       | 1               | 2               | 3           | 4               |
| 출시 버전  | iOS 10.0        | iOS 11.0        | iOS 12.0    | iOS 12.0        |
| Face ID    | No              | No              | Yes         | Yes             |

</div>


## Index, 칼럼 변경하기

```python
liverpool_df = pd.read_csv('data/liverpool.csv', index_col=0)  ## 데이터 출처: codeit
liverpool_df
```

<div class="code-example" markdown="1">

|                 | position   |   born | number   | nationality   |
|:----------------|:-----------|-------:|:---------|:--------------|
| Roberto Firmino | FW         |   1991 | no. 9    | Brazil        |
| Sadio Mane      | FW         |   1992 | no. 10   | Senegal       |
| Mohamed Salah   | FW         |   1992 | no. 11   | Egypt         |
| Joe Gomez       | DF         |   1997 | no. 12   | England       |
| Alisson Becker  | GK         |   1992 | no. 13   | Brazil        |

</div>

### df.rename()
: 칼럼명 바꾸기

```python
liverpool_df.rename(columns={'position':'Position'}, inplace=True)
liverpool_df
```

<div class="code-example" markdown="1">

|                 | Position   |   born | number   | nationality   |
|:----------------|:-----------|-------:|:---------|:--------------|
| Roberto Firmino | FW         |   1991 | no. 9    | Brazil        |
| Sadio Mane      | FW         |   1992 | no. 10   | Senegal       |
| Mohamed Salah   | FW         |   1992 | no. 11   | Egypt         |
| Joe Gomez       | DF         |   1997 | no. 12   | England       |
| Alisson Becker  | GK         |   1992 | no. 13   | Brazil        |

</div>

+) 칼럼명 여러 개 한번에 바꾸기
```python
liverpool_df.rename(columns={'born':'Born', 'number':'Number', 'nationality':'Nationality'}, inplace=True)
liverpool_df
```

<div class="code-example" markdown="1">

|                 | Position   |   Born | Number   | Nationality   |
|:----------------|:-----------|-------:|:---------|:--------------|
| Roberto Firmino | FW         |   1991 | no. 9    | Brazil        |
| Sadio Mane      | FW         |   1992 | no. 10   | Senegal       |
| Mohamed Salah   | FW         |   1992 | no. 11   | Egypt         |
| Joe Gomez       | DF         |   1997 | no. 12   | England       |
| Alisson Becker  | GK         |   1992 | no. 13   | Brazil        |

</div>

### Index 이름 붙이기
: **df.index.name** 활용
```python
liverpool_df.index.name = 'Player Name'
liverpool_df
```

<div class="code-example" markdown="1">

| Player Name     | Position   |   Born | Number   | Nationality   |
|:----------------|:-----------|-------:|:---------|:--------------|
| Roberto Firmino | FW         |   1991 | no. 9    | Brazil        |
| Sadio Mane      | FW         |   1992 | no. 10   | Senegal       |
| Mohamed Salah   | FW         |   1992 | no. 11   | Egypt         |
| Joe Gomez       | DF         |   1997 | no. 12   | England       |
| Alisson Becker  | GK         |   1992 | no. 13   | Brazil        |

</div>

**`df.index.name`은 Index 이름을 출력해주는 코드
```python
liverpool_df.index.name
```
```
'Player Name'
```

### df.reset_index()
: 새로운 숫자형 index를 생성함과 동시에 기존 index를 칼럼으로 추가
- 인덱스가 연속된 int 숫자형 데이터가 아닐 경우에 다시 이를 연속 int 숫자형 데이터로 바꿔줄 때 주로 사용
- +) 참고: 만약 기존 index가 이름이 없었다면, 'index'라는 이름의 칼럼명으로 추가됨

```python
liverpool_df.reset_index()   # inplace=True를 해주지 않았기에 df 자체는 바뀌지 않음
    ## 기존 index였던 'Player Name'가 칼럼으로 들어가고, 0부터 연속 숫자형으로 새롭게 index가 할당됨
```

<div class="code-example" markdown="1">

|    | Player Name     | Position   |   Born | Number   | Nationality   |
|---:|:----------------|:-----------|-------:|:---------|:--------------|
|  0 | Roberto Firmino | FW         |   1991 | no. 9    | Brazil        |
|  1 | Sadio Mane      | FW         |   1992 | no. 10   | Senegal       |
|  2 | Mohamed Salah   | FW         |   1992 | no. 11   | Egypt         |
|  3 | Joe Gomez       | DF         |   1997 | no. 12   | England       |
|  4 | Alisson Becker  | GK         |   1992 | no. 13   | Brazil        |

</div>

+) `drop=True`로 하면 이전 index는 없애고 새로운 index만 남는다
```python
liverpool_df.reset_index(drop=True)  # inplace=True를 해주지 않았기에 df 자체는 바뀌지 않음
    ## 기존 index였던 'Player Name'는 없어지고, 0부터 연속 숫자형으로 새롭게 index가 할당됨
```

<div class="code-example" markdown="1">

|    | Position   |   Born | Number   | Nationality   |
|---:|:-----------|-------:|:---------|:--------------|
|  0 | FW         |   1991 | no. 9    | Brazil        |
|  1 | FW         |   1992 | no. 10   | Senegal       |
|  2 | FW         |   1992 | no. 11   | Egypt         |
|  3 | DF         |   1997 | no. 12   | England       |
|  4 | GK         |   1992 | no. 13   | Brazil        |

</div>


### df.set_index()
: 다른 칼럼을 index로 설정, 기존의 index는 없어진다

```python
liverpool_df.set_index('Number')  # inplace=True를 해주지 않았기에 df 자체는 바뀌지 않음
    ## 기존 index였던 'Player Name'는 없어지고, 'Number' 칼럼이 index가 됨

```

<div class="code-example" markdown="1">

| Number   | Position   |   Born | Nationality   |
|:---------|:-----------|-------:|:--------------|
| no. 9    | FW         |   1991 | Brazil        |
| no. 10   | FW         |   1992 | Senegal       |
| no. 11   | FW         |   1992 | Egypt         |
| no. 12   | DF         |   1997 | England       |
| no. 13   | GK         |   1992 | Brazil        |

</div>

+) 기존 index를 잃지 않고 set_index 하려면 미리 index를 별도의 열에 저장해주면 된다
```python
liverpool_df['Player Name'] = liverpool_df.index  # 기존 index를 별도의 열에 저장
liverpool_df.set_index('Number', inplace=True) 

# 다음도 같은 효과:
# liverpool_df = liverpool_df.reset_index().set_index('Number')

liverpool_df
```

<div class="code-example" markdown="1">

| Number   | Position   |   Born | Nationality   | Player Name     |
|:---------|:-----------|-------:|:--------------|:----------------|
| no. 9    | FW         |   1991 | Brazil        | Roberto Firmino |
| no. 10   | FW         |   1992 | Senegal       | Sadio Mane      |
| no. 11   | FW         |   1992 | Egypt         | Mohamed Salah   |
| no. 12   | DF         |   1997 | England       | Joe Gomez       |
| no. 13   | GK         |   1992 | Brazil        | Alisson Becker  |

</div>

### 칼럼 순서 변경하기

```python
liverpool_df = pd.read_csv('data/liverpool.csv', index_col=0)  ## 다시 불러옴
liverpool_df
```

<div class="code-example" markdown="1">

|                 | position   |   born | number   | nationality   |
|:----------------|:-----------|-------:|:---------|:--------------|
| Roberto Firmino | FW         |   1991 | no. 9    | Brazil        |
| Sadio Mane      | FW         |   1992 | no. 10   | Senegal       |
| Mohamed Salah   | FW         |   1992 | no. 11   | Egypt         |
| Joe Gomez       | DF         |   1997 | no. 12   | England       |
| Alisson Becker  | GK         |   1992 | no. 13   | Brazil        |

</div>

1. **sorted()**를 활용하여 alphabetical order로 column명 정렬

    ```python
    columns = list(liverpool_df.columns)  # column을 list로 받아옴
    srt_col = sorted(columns)  # column명 알파벳순 정렬

    liverpool_df_srt = liverpool_df[srt_col]  # 원하는 순서의 열 list를 이렇게 넣어주면 됨
    liverpool_df_srt  ## 열 순서: born, nationality, number, position
    ```

    <div class="code-example" markdown="1">

    |                 |   born | nationality   | number   | position   |
    |:----------------|-------:|:--------------|:---------|:-----------|
    | Roberto Firmino |   1991 | Brazil        | no. 9    | FW         |
    | Sadio Mane      |   1992 | Senegal       | no. 10   | FW         |
    | Mohamed Salah   |   1992 | Egypt         | no. 11   | FW         |
    | Joe Gomez       |   1997 | England       | no. 12   | DF         |
    | Alisson Becker  |   1992 | Brazil        | no. 13   | GK         |

    </div>

    +) `list(df.columns)`: 칼럼명들을 list로 출력해준다
    ```python
    list(liverpool_df.columns)
    ```
    ```
    ['position', 'born', 'number', 'nationality'] 
    ```

1. **reversed()**를 활용하여 원래 순서의 반대로 column명 정렬

    ```python
    rvs_col = list(reversed(columns)) # 원래 column 순서의 반대로 정렬

    liverpool_df_rvs = liverpool_df[rvs_col]
    liverpool_df_rsv  ## 열 순서: nationality, number, born, position
    ```

    <div class="code-example" markdown="1">

    |                 | nationality   | number   |   born | position   |
    |:----------------|:--------------|:---------|-------:|:-----------|
    | Roberto Firmino | Brazil        | no. 9    |   1991 | FW         |
    | Sadio Mane      | Senegal       | no. 10   |   1992 | FW         |
    | Mohamed Salah   | Egypt         | no. 11   |   1992 | FW         |
    | Joe Gomez       | England       | no. 12   |   1997 | DF         |
    | Alisson Becker  | Brazil        | no. 13   |   1992 | GK         |

    </div>

## Merge, Join, Concatenate

### pd.merge(df1, df2)

```python
df1 = pd.DataFrame({'customer_id': [1001, 1002, 1003, 1004, 1005],
                    'name': ['Annie', 'Chloe', 'Jacob', 'David', 'Ellie'],
                    'sex': ['F', 'F', 'M', 'M', 'F']})

df2 = pd.DataFrame({'customer_id': [1001, 1001, 1005, 1004, 1002, 1008],
                    'amount_spent': [10000, 20000, 12000, 5000, 30000, 35000]})

print(df1, '\n')
print(df2)
```
```
   customer_id   name sex
0         1001  Annie   F
1         1002  Chloe   F
2         1003  Jacob   M
3         1004  David   M
4         1005  Ellie   F 

   customer_id  amount_spent
0         1001         10000
1         1001         20000
2         1005         12000
3         1004          5000
4         1002         30000
5         1008         35000
```

1. inner join (default): 두 DataFrame에 모두 데이터가 존재하는 행만 남는다
    ```python
    pd.merge(df1, df2)
    # 보통 default로 'inner join' 방식이 사용되며, 자동으로 이름이 같은 column을 기준으로 merge된다
    ```

    <div class="code-example" markdown="1">

    |    |   customer_id | name   | sex   |   amount_spent |
    |---:|--------------:|:-------|:------|---------------:|
    |  0 |          1001 | Annie  | F     |          10000 |
    |  1 |          1001 | Annie  | F     |          20000 |
    |  2 |          1002 | Chloe  | F     |          30000 |
    |  3 |          1004 | David  | M     |           5000 |
    |  4 |          1005 | Ellie  | F     |          12000 |

    </div>

1. 'on'으로 통합할 열 지정
    ```python
    pd.merge(df1, df2, on='customer_id')
    # on에 어떤 열을 기준으로 통합할 것인지 지정해줄 수 있다. (두 df에 모두 존재하는 열이여야 함)
    ## 이 예시에서는 상관 없지만, 기준 열이 아니면서 이름이 같은 열이 2개 이상일 경우에는 꼭 on을 지정해줘야 함
    ```

    <div class="code-example" markdown="1">

    |    |   customer_id | name   | sex   |   amount_spent |
    |---:|--------------:|:-------|:------|---------------:|
    |  0 |          1001 | Annie  | F     |          10000 |
    |  1 |          1001 | Annie  | F     |          20000 |
    |  2 |          1002 | Chloe  | F     |          30000 |
    |  3 |          1004 | David  | M     |           5000 |
    |  4 |          1005 | Ellie  | F     |          12000 |

    </div>

1. outer join: 어느 한 쪽의 DataFrame에만 있는 값이라도 다 보여주는 방식
    ```python
    pd.merge(df1, df2, how='outer')
    # 한 쪽에만 데이터가 있다면, 반대쪽 DataFrame의 해당 부분은 NaN으로 채워짐
    ```

    <div class="code-example" markdown="1">

    |    |   customer_id | name   | sex   |   amount_spent |
    |---:|--------------:|:-------|:------|---------------:|
    |  0 |          1001 | Annie  | F     |          10000 |
    |  1 |          1001 | Annie  | F     |          20000 |
    |  2 |          1002 | Chloe  | F     |          30000 |
    |  3 |          1003 | Jacob  | M     |            NaN |
    |  4 |          1004 | David  | M     |           5000 |
    |  5 |          1005 | Ellie  | F     |          12000 |
    |  6 |          1008 | NaN    | NaN   |          35000 |

    </div>

1. left outer join: 왼쪽 DataFrame (df1)에 존재하는 값을 기준으로 merge
    ```python
    pd.merge(df1, df2, how='left')
    ```

    <div class="code-example" markdown="1">

    |    |   customer_id | name   | sex   |   amount_spent |
    |---:|--------------:|:-------|:------|---------------:|
    |  0 |          1001 | Annie  | F     |          10000 |
    |  1 |          1001 | Annie  | F     |          20000 |
    |  2 |          1002 | Chloe  | F     |          30000 |
    |  3 |          1003 | Jacob  | M     |            NaN |
    |  4 |          1004 | David  | M     |           5000 |
    |  5 |          1005 | Ellie  | F     |          12000 |

    </div>

1. right outer join: 오른쪽 DataFrame (df2)에 존재하는 값을 기준으로 merge
    ```python
    pd.merge(df1, df2, how='right')
    ```

    <div class="code-example" markdown="1">

    |    |   customer_id | name   | sex   |   amount_spent |
    |---:|--------------:|:-------|:------|---------------:|
    |  0 |          1001 | Annie  | F     |          10000 |
    |  1 |          1001 | Annie  | F     |          20000 |
    |  2 |          1005 | Ellie  | F     |          12000 |
    |  3 |          1004 | David  | M     |           5000 |
    |  4 |          1002 | Chloe  | F     |          30000 |
    |  5 |          1008 | NaN    | NaN   |          35000 |

    </div>

1. 키가 되는 기준열의 이름이 두 데이터프레임에서 다른 경우
    ```python
    df1 = pd.DataFrame({'customer_id': [1001, 1002, 1003, 1004, 1005],
                        'name': ['Annie', 'Chloe', 'Jacob', 'David', 'Ellie'],
                        'sex': ['F', 'F', 'M', 'M', 'F']})

    df3 = pd.DataFrame({'customer': ['Chloe', 'Jacob', 'Ellie', 'Haley', 'Serene'],
                        'VIP_type': ['VIP', 'Gold', 'VIP', 'Silver', 'Gold']})

    print(df1, '\n')
    print(df3)
    ```
    ```
    customer_id   name sex
    0         1001  Annie   F
    1         1002  Chloe   F
    2         1003  Jacob   M
    3         1004  David   M
    4         1005  Ellie   F 

    customer VIP_type
    0    Chloe      VIP
    1    Jacob     Gold
    2    Ellie      VIP
    3    Haley   Silver
    4   Serene     Gold
    ```

    # 키가 되는 기준열의 이름이 두 데이터프레임에서 다른 경우: left_on, right_on 인수를 사용하여 기준열을 명시
    {: .fs-3  .text-grey-dk-000}

    ```python
    pd.merge(df1, df3, left_on='name', right_on="customer")  # default: inner join
    ```

    <div class="code-example" markdown="1">

    |    |   customer_id | name   | sex   | customer   | VIP_type   |
    |---:|--------------:|:-------|:------|:-----------|:-----------|
    |  0 |          1002 | Chloe  | F     | Chloe      | VIP        |
    |  1 |          1003 | Jacob  | M     | Jacob      | Gold       |
    |  2 |          1005 | Ellie  | F     | Ellie      | VIP        |

    </div>

### df1.join(df2)
- join 메소드를 사용해도 merge와 같은 결과를 낼 수 있다
- 하지만 join()은 index를 기준으로 결합하는 것이 기본이고, how='left'가 default다

```python
# join을 쓰기 위해, df1과 df2 모두 결합할 기준이 되는 열인 'customer_id'를 index로 지정해준다

df1.set_index('customer_id', inplace=True)
df2.set_index('customer_id', inplace=True)

print(df1, '\n')  
print(df2)
```
```
              name sex
customer_id           
1001         Annie   F
1002         Chloe   F
1003         Jacob   M
1004         David   M
1005         Ellie   F 

             amount_spent
customer_id              
1001                10000
1001                20000
1005                12000
1004                 5000
1002                30000
1008                35000
```
1. left outer join (default)
    ```python
    df1.join(df2)   # how='left'가 default값
    ```

    <div class="code-example" markdown="1">

    |   customer_id | name   | sex   |   amount_spent |
    |--------------:|:-------|:------|---------------:|
    |          1001 | Annie  | F     |          10000 |
    |          1001 | Annie  | F     |          20000 |
    |          1002 | Chloe  | F     |          30000 |
    |          1003 | Jacob  | M     |            NaN |
    |          1004 | David  | M     |           5000 |
    |          1005 | Ellie  | F     |          12000 |

    </div>

1. inner join
    ```python
    df1.join(df2, how='inner')  # how에 'right', 'outer', 'innner' 모두 사용 가능 
    ```

    <div class="code-example" markdown="1">

    |   customer_id | name   | sex   |   amount_spent |
    |--------------:|:-------|:------|---------------:|
    |          1001 | Annie  | F     |          10000 |
    |          1001 | Annie  | F     |          20000 |
    |          1002 | Chloe  | F     |          30000 |
    |          1004 | David  | M     |           5000 |
    |          1005 | Ellie  | F     |          12000 |

    </div>

### pd.concat([df1, df2])
- 기준 열(key column)을 사용하지 않고 단순히 데이터를 연결(concatenate)한다
- 기본적으로는 위/아래로 데이터 행을 연결하며, axis=1로 인수를 설정하면 옆으로 연결해준다

```python
import numpy as np

df1 = pd.DataFrame(
    np.arange(6).reshape(3, 2),
    columns=['a', 'b'])

df2 = pd.DataFrame(
    5 + np.arange(6).reshape(2,3),
    columns=['a', 'b', 'c'])

print(df1, '\n')
print(df2)
```
```
   a  b
0  0  1
1  2  3
2  4  5 

   a  b   c
0  5  6   7
1  8  9  10
```

1. 위 아래로 연결 (default)
    ```python
    pd.concat([df1, df2])  ## 위 아래로 연결됨. (column명 기준)
    ```

    <div class="code-example" markdown="1">

    |    |   a |   b |   c |
    |---:|----:|----:|----:|
    |  0 |   0 |   1 | NaN |
    |  1 |   2 |   3 | NaN |
    |  2 |   4 |   5 | NaN |
    |  0 |   5 |   6 |   7 |
    |  1 |   8 |   9 |  10 |

    </div>

    +) 그냥 이어붙이면 행 인덱스번호도 그대로 가져오기에, `ignore_index=True`로 인덱스를 재배열할 수 있다
    ```python
    pd.concat([df1, df2], ignore_index=True)
    ```

    <div class="code-example" markdown="1">

    |    |   a |   b |   c |
    |---:|----:|----:|----:|
    |  0 |   0 |   1 | NaN |
    |  1 |   2 |   3 | NaN |
    |  2 |   4 |   5 | NaN |
    |  3 |   5 |   6 |   7 |
    |  4 |   8 |   9 |  10 |

    </div> 

1. 옆으로 연결: `axis=1` 설정
    ```python
    pd.concat([df1, df2], axis=1)  # axis=1로 설정해주면, 옆으로 연결된다 (index 기준)
    ```

    <div class="code-example" markdown="1">

    |    |   a |   b |   a |   b |   c |
    |---:|----:|----:|----:|----:|----:|
    |  0 |   0 |   1 |   5 |   6 |   7 |
    |  1 |   2 |   3 |   8 |   9 |  10 |
    |  2 |   4 |   5 | NaN | NaN | NaN |

    </div>

1. Series 간 결합
    ```python
    sr1 = pd.Series(['e0','e1','e2','e3'], name = 'e')
    sr2 = pd.Series(['g0','g1','g2','g3'], name = 'g')

    result1 = pd.concat([sr1, sr2], axis = 1)  #열방향 연결, type = 데이터프레임이 됨
    print(result1, '\n')

    result2 = pd.concat([sr1, sr2], axis = 0)  #행방향 연결, type = 시리즈
    print(result2, '\n')

    ## 사실, pd.concat([sr1, sr2], axis = 0)는 sr1.append(sr2)와 동일한 결과
    result3 = sr1.append(sr2)   #이렇게 하면 행방향 concat과 동일
    print(result3)
    ```
    ```
        e   g
    0  e0  g0
    1  e1  g1
    2  e2  g2
    3  e3  g3

    0    e0
    1    e1
    2    e2
    3    e3
    0    g0
    1    g1
    2    g2
    3    g3
    dtype: object

    0    e0
    1    e1
    2    e2
    3    e3
    0    g0
    1    g1
    2    g2
    3    g3
    dtype: object 
    ```

    +) 참고) pd.merge()는 Series간 결합에는 사용할 수 없다. (공통의 column이 있어야 하는 게 전제조건)
    ```python
    pd.merge(sr1, sr2)  ## series끼리 merge하려고 하면 error.
    ```
    ```
    MergeError                                Traceback (most recent call last)
    <ipython-input-43-176c5dc5c9a2> in <module>()
    ----> 1 pd.merge(sr1, sr2)
    ```
