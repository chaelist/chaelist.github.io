---
layout: default
title: Pandas 데이터 결합 & 요약
parent: Pandas
nav_order: 4
---

# Pandas 데이터 결합 & 요약
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


## 데이터 요약 집계

### df.groupby()
: 그룹별로 요약해서 집계하기

```python
titanic_df = pd.read_csv('data/titanic.csv')   ## 데이터 출처: kaggle
titanic_df.head()
```

<div class="code-example" markdown="1">

|    |   PassengerId |   Survived |   Pclass | Name                                                | Sex    |   Age |   SibSp |   Parch | Ticket           |    Fare | Cabin   | Embarked   |
|---:|--------------:|-----------:|---------:|:----------------------------------------------------|:-------|------:|--------:|--------:|:-----------------|--------:|:--------|:-----------|
|  0 |             1 |          0 |        3 | Braund, Mr. Owen Harris                             | male   |    22 |       1 |       0 | A/5 21171        |  7.25   | nan     | S          |
|  1 |             2 |          1 |        1 | Cumings, Mrs. John Bradley (Florence Briggs Thayer) | female |    38 |       1 |       0 | PC 17599         | 71.2833 | C85     | C          |
|  2 |             3 |          1 |        3 | Heikkinen, Miss. Laina                              | female |    26 |       0 |       0 | STON/O2. 3101282 |  7.925  | nan     | S          |
|  3 |             4 |          1 |        1 | Futrelle, Mrs. Jacques Heath (Lily May Peel)        | female |    35 |       1 |       0 | 113803           | 53.1    | C123    | S          |
|  4 |             5 |          0 |        3 | Allen, Mr. William Henry                            | male   |    35 |       0 |       0 | 373450           |  8.05   | nan     | S          |

</div>


1. df.groupby('칼럼명').함수()
- DataFrame에 groupby()를 호출해 반환된 결과에 aggregation 함수를 호출하면, groupby() 대상 칼럼을 제외한 모든 칼럼에 aggregation 함수를 적용한 결과를 보여준다
- aggregation 함수 종류: count, sum, max, mean, nunique 등. (*<u>nunique</u>: unique count)
- 대상 칼럼이 index로 들어감

    ```python
    # 'Pclass' 기준으로 데이터 수를 집계
    titanic_df.groupby('Pclass').count()
        ## 3등석이 가장 사람이 많음을 유추 가능.
    ```

    <div class="code-example" markdown="1">

    |   Pclass |   PassengerId |   Survived |   Name |   Sex |   Age |   SibSp |   Parch |   Ticket |   Fare |   Cabin |   Embarked |
    |---------:|--------------:|-----------:|-------:|------:|------:|--------:|--------:|---------:|-------:|--------:|-----------:|
    |        1 |           216 |        216 |    216 |   216 |   186 |     216 |     216 |      216 |    216 |     176 |        214 |
    |        2 |           184 |        184 |    184 |   184 |   173 |     184 |     184 |      184 |    184 |      16 |        184 |
    |        3 |           491 |        491 |    491 |   491 |   355 |     491 |     491 |      491 |    491 |      12 |        491 |

    </div>

1. as_index=False: 대상 칼럼이 index가 아닌 칼럼으로 들어감
    ```python
    # 이렇게 as_index=False를 넣어주면 index가 아닌 칼럼으로 들어감
    titanic_df.groupby('Pclass', as_index=False).count()
    ```

    <div class="code-example" markdown="1">

    |    |   Pclass |   PassengerId |   Survived |   Name |   Sex |   Age |   SibSp |   Parch |   Ticket |   Fare |   Cabin |   Embarked |
    |---:|---------:|--------------:|-----------:|-------:|------:|------:|--------:|--------:|---------:|-------:|--------:|-----------:|
    |  0 |        1 |           216 |        216 |    216 |   216 |   186 |     216 |     216 |      216 |    216 |     176 |        214 |
    |  1 |        2 |           184 |        184 |    184 |   184 |   173 |     184 |     184 |      184 |    184 |      16 |        184 |
    |  2 |        3 |           491 |        491 |    491 |   491 |   355 |     491 |     491 |      491 |    491 |      12 |        491 |

    </div>

1. 특정 칼럼만 필터링해서 함수 적용

    ```python
    titanic_df.groupby('Pclass')[['PassengerId', 'Survived']].count()
    ```

    <div class="code-example" markdown="1">

    |   Pclass |   PassengerId |   Survived |
    |---------:|--------------:|-----------:|
    |        1 |           216 |        216 |
    |        2 |           184 |        184 |
    |        3 |           491 |        491 |

    </div>

    +) 이렇게 하는 것도 가능:
    ```python
    titanic_df.groupby('Pclass', as_index=False)[['PassengerId', 'Survived']].count()
    ```

    <div class="code-example" markdown="1">

    |    |   Pclass |   PassengerId |   Survived |
    |---:|---------:|--------------:|-----------:|
    |  0 |        1 |           216 |        216 |
    |  1 |        2 |           184 |        184 |
    |  2 |        3 |           491 |        491 |

    </div>

1. 여러 개의 aggregation 함수를 적용
- 적용하려는 함수명을 agg( ) 내에 인자로 입력해서 사용해야 함

    ```python
    titanic_df.groupby('Pclass')['Age'].agg([max, min])
    ```

    <div class="code-example" markdown="1">

    |   Pclass |   max |   min |
    |---------:|------:|------:|
    |        1 |    80 |  0.92 |
    |        2 |    70 |  0.67 |
    |        3 |    74 |  0.42 |

    </div>

1. 여러 칼럼에 여러 aggregation 함수 적용

    ```python
    titanic_df.groupby('Pclass')[['Age', 'Fare', 'SibSp']].agg([max, min])
    ```

    <div class="code-example" markdown="1">

    |          |:  Age        ||:  Fare        ||:  SibSp       ||
    |   Pclass |   max |   min |     max |  min |    max |   min |   
    |---------:|------:|------:|--------:|-----:|-------:|------:|
    |        1 |    80 |  0.92 | 512.329 |    0 |      3 |     0 |
    |        2 |    70 |  0.67 |  73.5   |    0 |      3 |     0 |
    |        3 |    74 |  0.42 |  69.55  |    0 |      8 |     0 |

    </div>


1. 여러 칼럼에 서로 다른 Aggregation 함수를 적용
- dictionary 형태로 함수를 적용할 칼럼과 함수명을 입력
- ※ aggregation 방식 중 nunique의 경우, `'Age': pd.Series.nunique`와 같은 방식으로 표기해줘야 한다

    ```python
    agg_format = {'Age':'max', 'SibSp':'sum', 'Fare':'mean'}  
    titanic_df.groupby('Pclass').agg(agg_format)
    ```

    <div class="code-example" markdown="1">

    |   Pclass |  Age |   SibSp |    Fare |
    |---------:|------:|--------:|--------:|
    |        1 |    80 |      90 | 84.1547 |
    |        2 |    70 |      74 | 20.6622 |
    |        3 |    74 |     302 | 13.6756 |

    </div>


**+) string value groupby**

```python
# sample dataframe 생성: 이용자와 이용한 앱이 매칭되어 있는 데이터

users = ['A', 'B', 'A', 'B', 'C', 'D', 'E', 'D', 'E']
apps_used = ['Google', 'Naver', 'YouTube', 'Google', 'YouTube', 'Facebook', 'Instagram', 'Naver', 'Google']

sample_df = pd.DataFrame({'users':users, 'apps_used':apps_used})
sample_df
```

<div class="code-example" markdown="1">

|    | users   | apps_used   |
|---:|:--------|:------------|
|  0 | A       | Google      |
|  1 | B       | Naver       |
|  2 | A       | YouTube     |
|  3 | B       | Google      |
|  4 | C       | YouTube     |
|  5 | D       | Facebook    |
|  6 | E       | Instagram   |
|  7 | D       | Naver       |
|  8 | E       | Google      |

</div>

1. list로 묶기: 각 이용자별 이용한 앱 조합
```python
sample_df.groupby('users')['apps_used'].apply(list)
```
```
users
A      [Google, YouTube]
B        [Naver, Google]
C              [YouTube]
D      [Facebook, Naver]
E    [Instagram, Google]
Name: apps_used, dtype: object
```

1. set으로 묶기: 순서 고려X
```python
sample_df.groupby('users')['apps_used'].apply(set)
```
```
users
A      {Google, YouTube}
B        {Google, Naver}
C              {YouTube}
D      {Facebook, Naver}
E    {Google, Instagram}
Name: apps_used, dtype: object
```

1. apply lambda를 활용해 원하는 모양으로 결합
```python
sample_df.groupby('users')['apps_used'].apply(lambda x: ', '.join(x))
```
```
users
A      Google, YouTube
B        Naver, Google
C              YouTube
D      Facebook, Naver
E    Instagram, Google
Name: apps_used, dtype: object
```

&nbsp;
{: .fs-1 .lh-0}

### pd.pivot_table()
: 엑셀에서처럼 pivot 돌리기 (엑셀에서 pivot 돌리는 것의 동작 방식을 생각하면 쉬움)
- ex) `pd.pivot_table(df, index='칼럼1', columns=['칼럼2','칼럼3'], values='칼럼4', fill_value=0, aggfunc='sum')`
  - index(열)나 columns(행)에 str 하나만 넣어도 되고, [ ] 리스트 형태로 여러 개 넣으면 multiindex / multiheader되는 것
  - values가 실제 pivot을 채울 값
  - aggfunc에는 집계 함수를 적어 줌. - 'sum'이면 합산 / 'mean'이면 평균
  - fill_value=0은 NaN 값들을 0으로 채워서 return하겠다는 뜻

```python
titanic_df.head()
```

<div class="code-example" markdown="1">

|    |   PassengerId |   Survived |   Pclass | Name                                                | Sex    |   Age |   SibSp |   Parch | Ticket           |    Fare | Cabin   | Embarked   |
|---:|--------------:|-----------:|---------:|:----------------------------------------------------|:-------|------:|--------:|--------:|:-----------------|--------:|:--------|:-----------|
|  0 |             1 |          0 |        3 | Braund, Mr. Owen Harris                             | male   |    22 |       1 |       0 | A/5 21171        |  7.25   | nan     | S          |
|  1 |             2 |          1 |        1 | Cumings, Mrs. John Bradley (Florence Briggs Thayer) | female |    38 |       1 |       0 | PC 17599         | 71.2833 | C85     | C          |
|  2 |             3 |          1 |        3 | Heikkinen, Miss. Laina                              | female |    26 |       0 |       0 | STON/O2. 3101282 |  7.925  | nan     | S          |
|  3 |             4 |          1 |        1 | Futrelle, Mrs. Jacques Heath (Lily May Peel)        | female |    35 |       1 |       0 | 113803           | 53.1    | C123    | S          |
|  4 |             5 |          0 |        3 | Allen, Mr. William Henry                            | male   |    35 |       0 |       0 | 373450           |  8.05   | nan     | S          |

</div>

1. 기본 예시

    ```python
    pd.pivot_table(titanic_df,        # 피벗할 데이터프레임
                  index='Pclass',    # 행 위치에 들어갈 열
                  columns='Sex',     # 열 위치에 들어갈 열
                  values='Age',      # 데이터로 사용할 열
                  aggfunc='mean')    # 데이터 집계함수
    ```

    <div class="code-example" markdown="1">

    |   Sex    |   female |    male |
    |   Pclass |          |         |
    |---------:|---------:|--------:|
    |        1 |  34.6118 | 41.2814 |
    |        2 |  28.723  | 30.7407 |
    |        3 |  21.75   | 26.5076 |

    </div>

1. 여러 개의 데이터 집계함수 넣기
    ```python
    pd.pivot_table(titanic_df, index='Pclass', columns='Sex', 
                   values='Survived', aggfunc=['mean', 'sum'])  
    ```

    <div class="code-example" markdown="1">

    | |: mean ||: sum||
    | Sex | female | male | female | male |
    |   Pclass |   | | | |
    |---------:|---------------------:|-------------------:|--------------------:|------------------:|
    |        1 |             0.968085 |           0.368852 |                  91 |                45 |
    |        2 |             0.921053 |           0.157407 |                  70 |                17 |
    |        3 |             0.5      |           0.135447 |                  72 |                47 |

    </div>

1. 많은 칼럼을 동시에 인자로 입력
- `index=`, `columns=`, `values=`에 모두 list 형태로 여러 개의 칼럼을 인자로 넣을 수 있다

    ```python
    pd.pivot_table(titanic_df, index=['Pclass', 'Sex'], columns='Survived', 
                   values=['Age', 'Fare'], aggfunc=['mean', 'max'])  
    ```

    <div class="code-example" markdown="1">

    |       |    |: mean  ||||: max  ||||
    |       |    |: Age ||: Fare ||: Age ||: Fare ||
    |       |Survived|: 0 |: 1 |: 0 |: 1 |: 0 |: 1 |: 0 |: 1 |
    |Pclass | Sex   | | | | | | | | |
    |:------|:------|---------------------:|---------------------:|----------------------:|----------------------:|--------------------:|--------------------:|---------------------:|---------------------:|
    |1      | female|              25.6667 |              34.939  |              110.604  |              105.978  |                  50 |                  63 |               151.55 |             512.329  |
    |^^     |   male|              44.582  |              36.248  |               62.8949 |               74.6373 |                  71 |                  80 |               263    |             512.329  |
    |2      | female|              36      |              28.0809 |               18.25   |               22.289  |                  57 |                  55 |                26    |              65      |
    |^^     |   male|              33.369  |              16.022  |               19.489  |               21.0951 |                  70 |                  62 |                73.5  |              39      |
    |3      | female|              23.8182 |              19.3298 |               19.7731 |               12.4645 |                  48 |                  63 |                69.55 |              31.3875 |
    |^^     |   male|              27.2558 |              22.2742 |               12.2045 |               15.5797 |                  74 |                  45 |                69.55 |              56.4958 |

    </div>


### df.pivot()
: 데이터 형태를 변경해준다. 원하는 형태로 데이터를 변형해준다는 점에서 `pd.pivot_table`과 유사하지만, pivot_table과 달리 데이터를 요약 집계해주는 기능은 없음

```python
pivot_sample = titanic_df.groupby(['Pclass', 'Sex'])[['Fare']].mean().reset_index()
pivot_sample
```

<div class="code-example" markdown="1">

|    |   Pclass | Sex    |     Fare |
|---:|---------:|:-------|---------:|
|  0 |        1 | female | 106.126  |
|  1 |        1 | male   |  67.2261 |
|  2 |        2 | female |  21.9701 |
|  3 |        2 | male   |  19.7418 |
|  4 |        3 | female |  16.1188 |
|  5 |        3 | male   |  12.6616 |

</div>

→ 'Pclass' 정보가 index로, 'Sex' 정보가 column으로, 그리고 'Fare'가 value로 들어가도록 reshape:
```python
pivot_sample.pivot(index='Pclass', columns='Sex', values='Fare')
```

<div class="code-example" markdown="1">

|   Pclass |   female |    male |
|---------:|---------:|--------:|
|        1 | 106.126  | 67.2261 |
|        2 |  21.9701 | 19.7418 |
|        3 |  16.1188 | 12.6616 |

</div>


## Multi-Index, Multi-Header 다루기
: groupby나 pivot_table로 데이터를 요약 집계하다 보면, multi-index나 multi-header를 다루게 된다

### Multi-Index 다루기

```python
gdf = titanic_df.groupby(['Pclass', 'Sex'])[['Age', 'Fare', 'Survived']].mean()
gdf
```

<div class="code-example" markdown="1">

|       |       |:    Age |:    Fare |:  Survived |
|Pclass | Sex   | | | |
|:--------------|--------:|---------:|-----------:|
|1      | female| 34.6118 | 106.126  |   0.968085 |
|^^     |   male| 41.2814 |  67.2261 |   0.368852 |
|2      | female| 28.723  |  21.9701 |   0.921053 |
|^^     |   male| 30.7407 |  19.7418 |   0.157407 |
|3      | female| 21.75   |  16.1188 |   0.5      |
|^^     |   male| 26.5076 |  12.6616 |   0.135447 |

</div>

1. 인덱싱: 1<sup>st</sup> level
    ```python
    # [Pclass]열의 [1]행만 인덱싱 (index_number=1이 아니라, 인덱스 이름이 1)
    gdf.loc[1]   ## 1은 int 타입이라서 '1'이라고 쓰지 않음
    ```

    <div class="code-example" markdown="1">

    | Sex    |     Age |     Fare |   Survived |
    |:-------|--------:|---------:|-----------:|
    | female | 34.6118 | 106.126  |   0.968085 |
    | male   | 41.2814 |  67.2261 |   0.368852 |

    </div>

1. 인덱싱: 1<sup>st</sup> level - 2<sup>nd</sup> level (순서대로, 차근차근)
    ```python
    # [Pclass]열의 [1]행 중, [Sex] 열의 [female]행만 인덱싱 (순서: 1, female)
    ## 이렇게 tuple 형태로 넣어주려면, 더 밖에 있는 index부터 차근차근 접근해야 한다
    
    gdf.loc[(1, 'female')]
        ## pivot_df.loc[1].loc['female'] 이렇게 해도 동일한 결과.
    ```
    ```
    Age          34.611765
    Fare        106.125798
    Survived      0.968085
    Name: (1, female), dtype: float64
    ```

1. 인덱싱: 2<sup>nd</sup> level에 바로 접근
- 멀티인덱서 **xs**를 이용하면 그룹 범주와 상관 없이 수준(level)만 명시해주면 인덱싱이 가능 (cf.loc와 iloc를 이용하려면 큰 그룹부터 순차적으로 인덱싱을 해야 함)

    ```python
    # Sex그룹의 male값을 갖는 행을 모두 추출, 즉 등급(class)별 male에 대한 자료를 인덱싱
    gdf.xs('male', level='Sex')
    ```

    <div class="code-example" markdown="1">

    |   Pclass |     Age |    Fare |   Survived |
    |---------:|--------:|--------:|-----------:|
    |        1 | 41.2814 | 67.2261 |   0.368852 |
    |        2 | 30.7407 | 19.7418 |   0.157407 |
    |        3 | 26.5076 | 12.6616 |   0.135447 |

    </div>

    +) `level=`에는 인덱스명을 적어줘도 되고, 해당 인덱스의 레벨을 int로 넣어줘도 된다. (0부터 시작)
    ```python
    ## 'Sex' level은 두번째 level이므로, level=1이라고 넣어줘도 위와 같은 결과.
    gdf.xs('male', level=1) 
    ```

1. 멀티 인덱스 해제: `reset_index()` 활용

    ```python
    gdf.reset_index()
    ```

    <div class="code-example" markdown="1">

    |    |   Pclass | Sex    |     Age |     Fare |   Survived |
    |---:|---------:|:-------|--------:|---------:|-----------:|
    |  0 |        1 | female | 34.6118 | 106.126  |   0.968085 |
    |  1 |        1 | male   | 41.2814 |  67.2261 |   0.368852 |
    |  2 |        2 | female | 28.723  |  21.9701 |   0.921053 |
    |  3 |        2 | male   | 30.7407 |  19.7418 |   0.157407 |
    |  4 |        3 | female | 21.75   |  16.1188 |   0.5      |
    |  5 |        3 | male   | 26.5076 |  12.6616 |   0.135447 |

    </div>

### Multi-Header 다루기

```python
gdf2 = titanic_df.groupby('Pclass')[['Age', 'Fare']].agg(['mean', 'max'])
gdf2
```

<div class="code-example" markdown="1">

| |:  Age  ||:    Fare ||
| |: mean |: max |: mean |: max | 
|   Pclass |  | | | |
|---------:|------------------:|-----------------:|-------------------:|------------------:|
|        1 |           38.2334 |               80 |            84.1547 |           512.329 |
|        2 |           29.8776 |               70 |            20.6622 |            73.5   |
|        3 |           25.1406 |               74 |            13.6756 |            69.55  |

</div> 

1. 인덱싱: 1<sup>st</sup> level

    ```python
    # 'Age' 열을 indexing
    gdf2['Age']
    ```

    <div class="code-example" markdown="1">

    |   Pclass |    mean |   max |
    |---------:|--------:|------:|
    |        1 | 38.2334 |    80 |
    |        2 | 29.8776 |    70 |
    |        3 | 25.1406 |    74 |

    </div>

1. 인덱싱: 1<sup>st</sup> level - 2<sup>nd</sup> level (순서대로, 차근차근)

    ```python
    # 'Age' 열의 'mean' 열을 indexing
    gdf2['Age']['mean']

    ## gdf2[('Age', 'mean')] 이렇게 넣어줘도 동일한 결과.
    ```
    ```
    Pclass
    1    38.233441
    2    29.877630
    3    25.140620
    Name: mean, dtype: float64
    ```

1. 인덱싱: 2<sup>nd</sup> level에 바로 접근
- 열 방향 멀티인덱싱에도 **xs**를 사용 -- `axis=1`이라고 하면 열 방향을 의미

    ```python
    # 'mean' 행을 모두 추출 -- 'Age'와 'Fare'의 'mean'을 모두 추출
    gdf2.xs('mean', level=1,  axis=1)
    ```

    <div class="code-example" markdown="1">

    |   Pclass |     Age |    Fare |
    |---------:|--------:|--------:|
    |        1 | 38.2334 | 84.1547 |
    |        2 | 29.8776 | 20.6622 |
    |        3 | 25.1406 | 13.6756 |

    </div>


1. 멀티 헤더 해제: column명을 아래와 같이 새로 넣어주면 된다

    ```python
    # 이렇게 column명을 넣어주면 된다 (두 레벨의 칼럼명을 모두 담은 이름으로 적어야 좋음)
    gdf2.columns = ['Age_mean', 'Age_max', 'Fare_mean','Fare_max']

    gdf2
    ```

    <div class="code-example" markdown="1">

    |   Pclass |   Age_mean |   Age_max |   Fare_mean |   Fare_max |
    |---------:|-----------:|----------:|------------:|-----------:|
    |        1 |    38.2334 |        80 |     84.1547 |    512.329 |
    |        2 |    29.8776 |        70 |     20.6622 |     73.5   |
    |        3 |    25.1406 |        74 |     13.6756 |     69.55  |

    </div>

    
    +) `map` 사용해서 새로운 칼럼명 생성하기

    ```python
    gdf2.columns = gdf2.columns.map('{0[0]}_{0[1]}'.format)  # column1_column2의 형태로 새 column명을 생성해줌
    gdf2
    ```

    <div class="code-example" markdown="1">

    |   Pclass |   Age_mean |   Age_max |   Fare_mean |   Fare_max |
    |---------:|-----------:|----------:|------------:|-----------:|
    |        1 |    38.2334 |        80 |     84.1547 |    512.329 |
    |        2 |    29.8776 |        70 |     20.6622 |     73.5   |
    |        3 |    25.1406 |        74 |     13.6756 |     69.55  |

    </div>


## unstack, stack으로 pivot

### df.unstack()
: index의 특정 level을 column 방향으로 pivot해주는 함수 (multi-index를 풀어준다)

```python
gdf3 = titanic_df.groupby(['Pclass', 'Sex'])[['Age']].mean()
gdf3

```

<div class="code-example" markdown="1">

|       ||:    Age |
|Pclass | Sex   |         |
|:--------------|--------:|
|1      | female| 34.6118 |
|^^     |   male| 41.2814 |
|2      | female| 28.723  |
|^^     |   male| 30.7407 |
|3      | female| 21.75   |
|^^     |   male| 26.5076 |

</div>

1. level=0: 1<sup>st</sup> level 인덱스를 column 방향으로 pivot
    ```python
    gdf3.unstack(level=0)
    ```

    <div class="code-example" markdown="1">

    |        |: Age     |||
    | Pclass |: 1 |: 2 |: 3 |
    | Sex    |   |   |   |
    |:-------|-------------:|-------------:|-------------:|
    | female |      34.6118 |      28.723  |      21.75   |
    | male   |      41.2814 |      30.7407 |      26.5076 |

    </div>

1. level=1: 2<sup>nd</sup> level 인덱스를 column 방향으로 pivot

    ```python
    gdf3.unstack(level=1)    
    ```

    <div class="code-example" markdown="1">

    |        | :Age     ||
    | Sex | :female  | :male |
    |   Pclass |    |      |
    |---------:|--------------------:|------------------:|
    |        1 |             34.6118 |           41.2814 |
    |        2 |             28.723  |           30.7407 |
    |        3 |             21.75   |           26.5076 |

    </div>

    +) 이 경우, 2nd level이 마지막 level이므로, `level=-1`라고 적어도 동일한 결과


### df.stack()
: column의 특정 level을 index 방향으로 pivot해주는 함수 (unstack과 반대: multi-index를 만들어준다)

```python
gdf4 = titanic_df.groupby('Pclass')[['Age']].agg(['mean', 'max'])
gdf4
```

<div class="code-example" markdown="1">

| |:  Age  ||
| |: mean |: max |
|   Pclass |  | |
|---------:|------------------:|-----------------:|
|        1 |           38.2334 |               80 |
|        2 |           29.8776 |               70 |
|        3 |           25.1406 |               74 |

</div> 

→ 2번째 level (마지막 level)을 index 방향으로 pivot

```python
gdf4.stack(level=-1)
```

<div class="code-example" markdown="1">

|              ||:    Age |
|Pclass |       |         |
|:--------------|--------:|
| 1  |  mean | 38.2334 |
| ^^ |  max  | 80      |
| 2 | mean | 29.8776 |
| ^^ | max  | 70      |
| 3 | mean | 25.1406 |
| ^^ | max  | 74      |

</div>


