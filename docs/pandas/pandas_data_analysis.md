---
layout: default
title: Pandas 데이터 분석
parent: Pandas
nav_order: 3
---

# Pandas 데이터 분석
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


## 큰 DataFrame 살펴보기

### 데이터 분포 파악
1. **df.head()**: 맨 위의 행 5개만 보여줌. 
- 데이터가 큰 경우, 이렇게 위의 행만 일부 가져와서 데이터 구조를 살펴보면 도움이 된다.

    ```python
    laptops_df = pd.read_csv('data/laptops.csv')  ## 데이터 출처: codeit

    laptops_df.head()
    ```

    <div class="code-example" markdown="1">

    |    | brand   | model            |   ram | hd_type   |   hd_size |   screen_size |   price | processor_brand   | processor_model   |   clock_speed | graphic_card_brand   |   graphic_card_size | os    |   weight |   comments |
    |---:|:--------|:-----------------|------:|:----------|----------:|--------------:|--------:|:------------------|:------------------|--------------:|:---------------------|--------------------:|:------|---------:|-----------:|
    |  0 | Dell    | Inspiron 15-3567 |     4 | hdd       |      1024 |          15.6 |   40000 | intel             | i5                |           2.5 | intel                |                 nan | linux |     2.5  |        nan |
    |  1 | Apple   | MacBook Air      |     8 | ssd       |       128 |          13.3 |   55499 | intel             | i5                |           1.8 | intel                |                   2 | mac   |     1.35 |        nan |
    |  2 | Apple   | MacBook Air      |     8 | ssd       |       256 |          13.3 |   71500 | intel             | i5                |           1.8 | intel                |                   2 | mac   |     1.35 |        nan |
    |  3 | Apple   | MacBook Pro      |     8 | ssd       |       128 |          13.3 |   96890 | intel             | i5                |           2.3 | intel                |                   2 | mac   |     3.02 |        nan |
    |  4 | Apple   | MacBook Pro      |     8 | ssd       |       256 |          13.3 |  112666 | intel             | i5                |           2.3 | intel                |                   2 | mac   |     3.02 |        nan |

    </div>  

    +) 원하는 수만큼의 행을 불러올 수도 있음
    ```python
    laptops_df.head(7)   ## 위의 7행만 불러온다는 뜻
    ```

    <div class="code-example" markdown="1">

    |    | brand   | model                  |   ram | hd_type   |   hd_size |   screen_size |   price | processor_brand   | processor_model   |   clock_speed | graphic_card_brand   |   graphic_card_size | os    |   weight |   comments |
    |---:|:--------|:-----------------------|------:|:----------|----------:|--------------:|--------:|:------------------|:------------------|--------------:|:---------------------|--------------------:|:------|---------:|-----------:|
    |  0 | Dell    | Inspiron 15-3567       |     4 | hdd       |      1024 |          15.6 |   40000 | intel             | i5                |           2.5 | intel                |                 nan | linux |     2.5  |        nan |
    |  1 | Apple   | MacBook Air            |     8 | ssd       |       128 |          13.3 |   55499 | intel             | i5                |           1.8 | intel                |                   2 | mac   |     1.35 |        nan |
    |  2 | Apple   | MacBook Air            |     8 | ssd       |       256 |          13.3 |   71500 | intel             | i5                |           1.8 | intel                |                   2 | mac   |     1.35 |        nan |
    |  3 | Apple   | MacBook Pro            |     8 | ssd       |       128 |          13.3 |   96890 | intel             | i5                |           2.3 | intel                |                   2 | mac   |     3.02 |        nan |
    |  4 | Apple   | MacBook Pro            |     8 | ssd       |       256 |          13.3 |  112666 | intel             | i5                |           2.3 | intel                |                   2 | mac   |     3.02 |        nan |
    |  5 | Apple   | MacBook Pro (TouchBar) |    16 | ssd       |       512 |          15   |  226000 | intel             | i7                |           2.7 | intel                |                   2 | mac   |     2.5  |        nan |
    |  6 | Apple   | MacBook Pro (TouchBar) |    16 | ssd       |       512 |          13.3 |  158000 | intel             | i5                |           2.9 | intel                |                   2 | mac   |     1.37 |        nan |
    
    </div>

1. **df.tail()**: 맨 뒤의 5개 행만 보여줌. 
- head()와 마찬가지로, `tail(10)`과 같이 수를 지정하는 것도 가능.
  
    ```python
    laptops_df.tail()
    ```

    <div class="code-example" markdown="1">

    |     | brand   | model          |   ram | hd_type   |   hd_size |   screen_size |   price | processor_brand   | processor_model   |   clock_speed | graphic_card_brand   |   graphic_card_size | os      |   weight |   comments |
    |----:|:--------|:---------------|------:|:----------|----------:|--------------:|--------:|:------------------|:------------------|--------------:|:---------------------|--------------------:|:--------|---------:|-----------:|
    | 162 | Asus    | A555LF         |     8 | hdd       |      1024 |          15.6 |   39961 | intel             | i3 4th gen        |           1.7 | nvidia               |                   2 | windows |      2.3 |        nan |
    | 163 | Asus    | X555LA-XX172D  |     4 | hdd       |       500 |          15.6 |   28489 | intel             | i3 4th gen        |           1.9 | intel                |                 nan | linux   |      2.3 |        nan |
    | 164 | Asus    | X554LD         |     2 | hdd       |       500 |          15.6 |   29199 | intel             | i3 4th gen        |           1.9 | intel                |                   1 | linux   |      2.3 |        nan |
    | 165 | Asus    | X550LAV-XX771D |     2 | hdd       |       500 |          15.6 |   29990 | intel             | i3 4th gen        |           1.7 | intel                |                 nan | linux   |      2.5 |        nan |
    | 166 | Asus    | X540LA-XX538T  |     4 | hdd       |      1024 |          15.6 |   30899 | intel             | i3 5th gen        |           2   | intel                |                 nan | windows |      2.3 |        nan |

    </div>

1. **df.shape**: 행과 열의 개수를 알려줌. - 데이터의 분포를 한 눈에 확인할 수 있다.
```python
laptops_df.shape    # 167개의 행과 15개의 열로 구성된 데이터라는 뜻
```
```
(167, 15)
```

1. **df.columns**: column명을 모두 추출. - 어떤 column들이 있는지 한 눈에 확인할 수 있다.
```python
laptops_df.columns
```
```
Index(['brand', 'model', 'ram', 'hd_type', 'hd_size', 'screen_size', 'price', 'processor_brand', 
       'processor_model', 'clock_speed', 'graphic_card_brand', 'graphic_card_size', 'os', 'weight', 
       'comments'],  dtype='object')
```

1. **df.info()**: 총 데이터 건수와 데이터 타입, Null 개수를 알 수 있다
    ```python
    laptops_df.info()
      # 예를 들어, weight 칼럼에서 non_null 값이 160개라는 것은, 167개 데이터 중 7개는 Null이라는 의미
    ```

    ```
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 167 entries, 0 to 166
    Data columns (total 15 columns):
    #   Column              Non-Null Count  Dtype  
    ---  ------              --------------  -----  
    0   brand               167 non-null    object 
    1   model               167 non-null    object 
    2   ram                 167 non-null    int64  
    3   hd_type             167 non-null    object 
    4   hd_size             167 non-null    int64  
    5   screen_size         167 non-null    float64
    6   price               167 non-null    int64  
    7   processor_brand     167 non-null    object 
    8   processor_model     167 non-null    object 
    9   clock_speed         166 non-null    float64
    10  graphic_card_brand  163 non-null    object 
    11  graphic_card_size   81 non-null     float64
    12  os                  167 non-null    object 
    13  weight              160 non-null    float64
    14  comments            55 non-null     object 
    dtypes: float64(4), int64(3), object(8)
    memory usage: 19.7+ KB
    ``` 

1. **df.describe()**: 데이터의 분포도 파악
- 숫자형(int, float 등) 칼럼의 분포도만 조사하며, object 타입의 칼럼은 출력에서 제외됨
- count는 Not Null인 데이터 건수
- mean은 전체 데이터의 평균값, std는 표준편차, min은 최솟값, max는 최댓값

    ```python
    laptops_df.describe()
    ```

    <div class="code-example" markdown="1">

    |       |       ram |   hd_size |   screen_size |    price |   clock_speed |   graphic_card_size |     weight |
    |:------|----------:|----------:|--------------:|---------:|--------------:|--------------------:|-----------:|
    | count | 167       |   167     |     167       |    167   |    166        |             81      | 160        |
    | mean  |   6.8982  |   768.91  |      14.7752  |  64132.9 |      2.32108  |             52.1605 |   2.25081  |
    | std   |   3.78748 |   392.991 |       1.37653 |  42797.7 |      0.554187 |            444.134  |   0.648446 |
    | min   |   2       |    32     |      10.1     |  13872   |      1.1      |              1      |   0.78     |
    | 25%   |   4       |   500     |      14       |  35457.5 |      1.9      |              2      |   1.9      |
    | 50%   |   8       |  1024     |      15.6     |  47990   |      2.3      |              2      |   2.2      |
    | 75%   |   8       |  1024     |      15.6     |  77494.5 |      2.6      |              4      |   2.6      |
    | max   |  16       |  2048     |      17.6     | 226000   |      3.8      |           4000      |   4.2      |

    </div>

    ※ 데이터의 분포도를 아는 것은 머신러닝 알고리즘의 성능을 향상시키는 중요한 요소이다.
    (ex. 회귀에서 결정 값이 정규분포를 따르지 않고 특정 값으로 왜곡되어 있다면 예측 성능이 저하됨)

### 데이터 정렬
: **df.sort_values()**로 특정 열 기준으로 정렬하기
- `inplace=True`를 써주면 dataframe 자체가 바뀌고, 써주지 않으면 그냥 정렬된 결과가 return되고 원본 dataframe은 바뀌지 않는다.

`# 오름차순 정렬
{: .fs-3  .text-grey-dk-000} 
```python
## 가격 기준으로 오름차순 정렬 (ascending=True가 default라, 안써줘도 됨.)
laptops_df.sort_values(by='price').head()  ##다 확인하면 너무 많으니, 가격이 낮은 순으로 top5만 확인
```

<div class="code-example" markdown="1">

|     | brand   | model                     |   ram | hd_type   |   hd_size |   screen_size |   price | processor_brand   | processor_model   |   clock_speed | graphic_card_brand   |   graphic_card_size | os      |   weight |   comments |
|----:|:--------|:--------------------------|------:|:----------|----------:|--------------:|--------:|:------------------|:------------------|--------------:|:---------------------|--------------------:|:--------|---------:|-----------:|
| 148 | Acer    | Aspire SW3-016            |     2 | ssd       |        32 |          10.1 |   13872 | intel             | Atom Z8300        |          1.44 | intel                |                 nan | windows |      1.2 |        nan |
|  83 | Acer    | A315-31CDC UN.GNTSI.001   |     2 | ssd       |       500 |          15.6 |   17990 | intel             | Celeron           |          1.1  | intel                |                 nan | windows |      2.1 |        nan |
| 108 | Acer    | Aspire ES-15 NX.GKYSI.010 |     4 | hdd       |       500 |          15.6 |   17990 | amd               | A4-7210           |          1.8  | amd                  |                 nan | windows |      2.4 |        nan |
| 100 | Acer    | A315-31-P4CRUN.GNTSI.002  |     4 | hdd       |       500 |          15.6 |   18990 | intel             | pentium           |          1.1  | intel                |                 nan | windows |    nan   |        nan |
|  73 | Acer    | Aspire ES1-523            |     4 | hdd       |      1024 |          15.6 |   19465 | amd               | A4-7210           |          1.8  | amd                  |                 nan | linux   |      2.4 |        nan |

</div>

\# 내림차순 정렬
{: .fs-3  .text-grey-dk-000} 
```python
 # 가격 기준으로 내림차순 정렬 (가격 높은 것부터 순서대로.)
laptops_df.sort_values(by='price', ascending=False).head()   ##가격 높은 top5만 확인
```

<div class="code-example" markdown="1">

|     | brand     | model                  |   ram | hd_type   |   hd_size |   screen_size |   price | processor_brand   | processor_model   |   clock_speed | graphic_card_brand   |   graphic_card_size | os      |   weight | comments                                                                                                 |
|----:|:----------|:-----------------------|------:|:----------|----------:|--------------:|--------:|:------------------|:------------------|--------------:|:---------------------|--------------------:|:--------|---------:|:---------------------------------------------------------------------------------------------------------|
|   5 | Apple     | MacBook Pro (TouchBar) |    16 | ssd       |       512 |          15   |  226000 | intel             | i7                |           2.7 | intel                |                   2 | mac     |      2.5 | nan                                                                                                      |
|  90 | Alienware | 15 Notebook            |    16 | hdd       |      1024 |          15.6 |  199000 | intel             | i7                |           2.6 | nvidia               |                   8 | windows |      3.5 | Maximum Display Resolution : 1920 x 1080 pixel                                                           |
|  96 | Alienware | AW13R3-7000SLV-PUS     |     8 | ssd       |       256 |          13.3 |  190256 | intel             | i7                |           3   | nvidia               |                   6 | windows |      2.6 | 13.3 inch FHD (1920 x 1080) IPS Anti-Glare 300-nits Display 1 Lithium ion batteries required. (included) |
|  31 | Acer      | Predator 17            |    16 | ssd       |       256 |          17.3 |  178912 | intel             | i7                |           2.6 | nvidia               |                 nan | windows |      4.2 | Integrated Graphics                                                                                      |
| 154 | Microsoft | Surface Book CR9-00013 |     8 | ssd       |       128 |          13.5 |  178799 | intel             | i5                |           1.8 | intel                |                 nan | windows |      1.5 | nan                                                                                                      |

</div>

### Aggregation 함수로 데이터 속성 파악
: min(), max(), sum(), count() 등 사용 가능.

1. 전체 dataframe의 속성 일괄 파악
```python
laptops_df.count()   # 모든 칼럼의 count를 반환한다 (NaN이 아닌 값만 셈)
```
```
brand                   167
model                   167
ram                     167
hd_type                 167
hd_size                 167
screen_size             167
price                   167
processor_brand         167
processor_model         167
clock_speed             166
graphic_card_brand      163
graphic_card_size        81
os                      167
weight                  160
comments                 55
model_len               167
Expensive_Affordable    167
price_cat               167
dtype: int64
```

1. 특정 칼럼들의 속성만 확인
```python
laptops_df[['screen_size', 'price']].mean()  # 특정 칼럼들만 추출해서 함수 적용
```
```
screen_size       14.775210
price          64132.898204
dtype: float64
```

### Series별로 추출해서 살펴보기

1. **srs.unique()**: 중복을 제외하고 어떤 값이 있나 파악
- dataframe에는 적용되지 않고 series에만 적용되는 함수

    ```python
    laptops_df['brand'].unique()  # 겹치는 걸 제외하고 총 몇 개의 브랜드가 있나 살펴봄
    ```
    ```
    array(['Dell', 'Apple', 'Acer', 'HP', 'Lenovo', 'Alienware', 'Microsoft', 'Asus'], dtype=object)
    ```

    +) unique한 값의 수 구하기
    ```python
    laptops_df['brand'].nunique()    ## len(laptops_df['brand'].unique())와 동일한 결과를 냄
    ```
    ```
    8
    ```

1. **srs.value_counts()**: 각 값이 몇 번씩 나오는지 파악
```python
laptops_df['brand'].value_counts()  # 각 브랜드가 몇 번 나오는지 살펴봄
```
```
HP           55
Acer         35
Dell         31
Lenovo       18
Asus          9
Apple         7
Alienware     6
Microsoft     6
Name: brand, dtype: int64
```

1. **srs.describe()**: `df.describe()`를 할 때와 같은 효과. (데이터 분포 요약)
```python
laptops_df['brand'].describe()   ## 'freq'는 'top' 빈도로 등장하는 'HP'가 55번 나온다는 뜻
```
```
count     167
unique      8
top        HP
freq       55
Name: brand, dtype: object
```


## 결손 데이터 (NaN) 처리
- 결측치가 있으면 머신러닝을 할 수 없기에, 결측치를 삭제하거나 다른 방법으로 채워줘야 한다

### isna()
: 각 값이 NaN인지 아닌지를 True나 False로 알려준다

```python
laptops_df.isna().head(3) 
```

<div class="code-example" markdown="1">

|    |   brand |   model |   ram |   hd_type |   hd_size |   screen_size |   price |   processor_brand |   processor_model |   clock_speed |   graphic_card_brand |   graphic_card_size |   os |   weight |   comments |
|---:|--------:|--------:|------:|----------:|----------:|--------------:|--------:|------------------:|------------------:|--------------:|---------------------:|--------------------:|-----:|---------:|-----------:|
|  0 |       False |       False |     False |         False |         False |             False |       False |                 False |                 False |             False |                    False |                   True |    False |        False |          True |
|  1 |       False |       False |     False |         False |         False |             False |       False |                 False |                 False |             False |                    False |                   False |    False |        False |          True |
|  2 |       False |       False |     False |         False |         False |             False |       False |                 False |                 False |             False |                    False |                   False |    False |        False |          True |

</div>

+) 칼럼별 결손 데이터 수 구하기
- **df.isna().sum()**하면 True는 1, False는 0으로 계산되므로 각 칼럼별 결손 데이터 수를 구할 수 있다 

```python
laptops_df.isna().sum()  
```
```
brand                   0
model                   0
ram                     0
hd_type                 0
hd_size                 0
screen_size             0
price                   0
processor_brand         0
processor_model         0
clock_speed             1
graphic_card_brand      4
graphic_card_size      86
os                      0
weight                  7
comments              112
dtype: int64
```

### fillna()
: 결손 데이터 대체
- `fillna(대체할 값)`으로 적어주면 NaN(결측치)가 모두 '대체할 값'으로 바뀐다
- `inplace=True`를 해주면 dataframe 자체가 변경됨

```python
# graphic_card_brand 칼럼의 NaN 값을 모두 'N/A'로 대체
laptops_df['graphic_card_brand'].fillna('N/A', inplace=True)  
laptops_df.isna().sum()   ## graphic_card_brand 칼럼의 NaN 값이 모두 없어짐
```
```
brand                   0
model                   0
ram                     0
hd_type                 0
hd_size                 0
screen_size             0
price                   0
processor_brand         0
processor_model         0
clock_speed             1
graphic_card_brand      0
graphic_card_size      86
os                      0
weight                  7
comments              112
dtype: int64
```

+) NaN 값을 해당 칼럼의 평균값으로 대체하기
```python
# graphic_card_size 칼럼의 NaN 값을 평균값으로 대체해보기
laptops_df['graphic_card_size'].fillna(laptops_df['graphic_card_size'].mean(), inplace=True)
laptops_df.head(3)
    ## index=0 행의 graphic_card_size 값이 원래는 NaN이었는데, 평균값인 '52.1605'로 바뀐 것을 확인할 수 있다
```

<div class="code-example" markdown="1">

|    | brand   | model            |   ram | hd_type   |   hd_size |   screen_size |   price | processor_brand   | processor_model   |   clock_speed | graphic_card_brand   |   graphic_card_size | os    |   weight |   comments |
|---:|:--------|:-----------------|------:|:----------|----------:|--------------:|--------:|:------------------|:------------------|--------------:|:---------------------|--------------------:|:------|---------:|-----------:|
|  0 | Dell    | Inspiron 15-3567 |     4 | hdd       |      1024 |          15.6 |   40000 | intel             | i5                |           2.5 | intel                |             52.1605 | linux |     2.5  |        nan |
|  1 | Apple   | MacBook Air      |     8 | ssd       |       128 |          13.3 |   55499 | intel             | i5                |           1.8 | intel                |              2      | mac   |     1.35 |        nan |
|  2 | Apple   | MacBook Air      |     8 | ssd       |       256 |          13.3 |   71500 | intel             | i5                |           1.8 | intel                |              2      | mac   |     1.35 |        nan |

</div>

### dropna()
: 결측치가 있는 행 삭제
- NaN 값을 적절한 값으로 대체해줘도 좋지만, 적절하게 대체되지 않는 경우 머신러닝의 성능에 악영향을 줄 수 있다. 그렇기에 아예 결측치가 있는 행을 삭제해버리는 것도 좋은 방법이다.
- `df.dropna()`라고만 하면 하나의 칼럼에라도 결측치가 있는 모든 행이 삭제됨
- `df.dropna(subset=['칼럼명'])`이라고 제한을 걸면, 해당 칼럼에 결측치가 있는 데이터만 삭제된다

```python
laptops_df.dropna(subset=['weight'], inplace=True)  ## 'weight' 칼럼에 결측치가 있는 행만 삭제
    # 이 경우, 'comment' 칼럼은 결측치가 많은 것이 당연하기에, dropna()라고만 하면 대부분의 데이터가 삭제됨 
laptops_df.isna().sum()
```
```
brand                   0
model                   0
ram                     0
hd_type                 0
hd_size                 0
screen_size             0
price                   0
processor_brand         0
processor_model         0
clock_speed             1
graphic_card_brand      3
graphic_card_size       0
os                      0
weight                  0
comments              105
dtype: int64
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
