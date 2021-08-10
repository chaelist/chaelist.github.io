---
layout: default
title: Pandas 기초
parent: Pandas
nav_order: 1
---

# Pandas 기초
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


*Pandas: 데이터 분석 목적으로 널리 사용되는 라이브러리
{: .text-purple-300}

## Pandas DataFrame 만들기
### From lists of lists, array of arrays, list of series
: 2차원 리스트, 2차원 numpy array, pandas Series를 담고 있는 리스트로 DataFrame을 만들기

```python
import pandas as pd    # import해야 사용 가능; 보통 pd로 줄여서 import

# 2차원 리스트
two_dimensional_list = [['Emily', 50, 86], ['Abby', 89, 31], ['Cornelia', 68, 91], ['Kai', 88, 75]]

# 2차원 numpy array
two_dimensional_array = np.array(two_dimensional_list)

# pandas series를 담고 있는 리스트
list_of_series = [
    pd.Series(['Emily', 50, 86]), 
    pd.Series(['Abby', 89, 31]), 
    pd.Series(['Cornelia', 68, 91]), 
    pd.Series(['Kai', 88, 75])
]

# 아래 셋은 모두 동일한 결과 반환
df1 = pd.DataFrame(two_dimensional_list)
df2 = pd.DataFrame(two_dimensional_array)
df3 = pd.DataFrame(list_of_series)

df1  ## 따로 column과 row(index)에 대한 설정이 없으면 그냥 0, 1, 2, ... 순서로 값이 매겨짐
```

<div class="code-example" markdown="1">

|    | 0        |   1 |   2 |
|---:|:---------|----:|----:|
|  0 | Emily    |  50 |  86 |
|  1 | Abby     |  89 |  31 |
|  2 | Cornelia |  68 |  91 |
|  3 | Kai      |  88 |  75 |

</div>

+) 추가: 칼럼명과 index명을 지어주기
```python
two_dimensional_list = [['Emily', 50, 86], ['Abby', 89, 31], ['Cornelia', 68, 91], ['Kai', 88, 75]]
my_df = pd.DataFrame(two_dimensional_list, columns=['name', 'french_score', 'math_score'], 
                     index=['a', 'b', 'c', 'd'])
my_df
```

<div class="code-example" markdown="1">

|    | name     |   french_score |   math_score |
|:---|:---------|---------------:|-------------:|
| a  | Emily    |             50 |           86 |
| b  | Abby     |             89 |           31 |
| c  | Cornelia |             68 |           91 |
| d  | Kai      |             88 |           75 |

</div>

### From dict of lists, dict of arrays, dict of series
: 파이썬 사전(dictionary)으로 DataFrame 만들기
- 사전의 key로는 column 이름을 쓰고, 그 column에 해당하는 리스트, numpy array, 혹은 pandas Series를 사전의 value로 넣어주면 된다

```python
names = ['Emily', 'Abby', 'Cornelia', 'Kai']
french_scores = [50, 89, 68, 88]
math_scores = [86, 31, 91, 75]

dict1 = {
    'name': names, 
    'french_score': french_scores, 
    'math_score': math_scores
}

dict2 = {
    'name': np.array(names), 
    'french_score': np.array(french_scores), 
    'math_score': np.array(math_scores)
}

dict3 = {
    'name': pd.Series(names), 
    'french_score': pd.Series(french_scores), 
    'math_score': pd.Series(math_scores)
}


# 아래 셋은 모두 동일한 결과 반환
df1 = pd.DataFrame(dict1)
df2 = pd.DataFrame(dict2)
df3 = pd.DataFrame(dict3)

df1
```

<div class="code-example" markdown="1">

|    | name     |   french_score |   math_score |
|---:|:---------|---------------:|-------------:|
|  0 | Emily    |             50 |           86 |
|  1 | Abby     |             89 |           31 |
|  2 | Cornelia |             68 |           91 |
|  3 | Kai      |             88 |           75 |

</div>

&nbsp;
{: .fs-1 .lh-0}


+) **DataFrame.from_dict()**  
\: from_dict()를 이용하면 dictionary 형태의 데이터를 row 방향이나 column 방향 중 어떤식으로든 DataFrame으로 만들 수 있다. 

- (default) dictionary의 key를 column으로:
    ```python
    data = {
        'name': ['Emily', 'Abby', 'Cornelia', 'Kai'], 
        'french_score': [50, 89, 68, 88], 
        'math_score': [86, 31, 91, 75]
    }

    pd.DataFrame.from_dict(data)   # pd.DataFrame(data)과 동일한 결과
    ```

    <div class="code-example" markdown="1">

    |    | name     |   french_score |   math_score |
    |---:|:---------|---------------:|-------------:|
    |  0 | Emily    |             50 |           86 |
    |  1 | Abby     |             89 |           31 |
    |  2 | Cornelia |             68 |           91 |
    |  3 | Kai      |             88 |           75 |

    </div>

- dictionary의 key를 row로:
    ```python
    data = {
        'row1': ['Emily', 50, 86],
        'row2': ['Abby', 89, 31],
        'row3': ['Cornelia', 68, 91],
        'row4': ['Kai', 88, 75]
    }

    pd.DataFrame.from_dict(data, orient='index', columns=['name', 'french_score', 'math_score'])
    ```

    <div class="code-example" markdown="1">

    |    | name     |   french_score |   math_score |
    |---:|:---------|---------------:|-------------:|
    | row1 | Emily    |             50 |           86 |
    | row2 | Abby     |             89 |           31 |
    | row3 | Cornelia |             68 |           91 |
    | row4 | Kai      |             88 |           75 |

    </div>



### From list of dicts
: 사전이 담긴 리스트로 DataFrame 만들기

```python
my_list = [
    {'name': 'Emily', 'french_score': 50, 'math_score': 86},
    {'name': 'Abby', 'french_score': 89, 'math_score': 31},
    {'name': 'Cornelia', 'french_score': 68, 'math_score': 91},
    {'name': 'Kai', 'french_score': 88, 'math_score': 75}
]

df = pd.DataFrame(my_list)
df
```

<div class="code-example" markdown="1">

|    | name     |   french_score |   math_score |
|---:|:---------|---------------:|-------------:|
|  0 | Emily    |             50 |           86 |
|  1 | Abby     |             89 |           31 |
|  2 | Cornelia |             68 |           91 |
|  3 | Kai      |             88 |           75 |

</div>

※ 알아둘 사실:
- Dictionary는 data의 순서가 없기 때문에 입력 순서대로 column이 만들어지는 것은 아니다.
- 반면, lists, arrays, series는 순서가 있는 자료구조이기 때문에, 입력한 순서대로 column을 만들어 준다.


## DataFrame 복사본 만들기
- a = b 방식: 원본 데이터가 변하면 똑같이 변하는 '깊은 복사'
- `df.copy()` 방식: 복사 당시의 상태만 복사되는 '얕은 복사'

```python
a = pd.DataFrame([['Kim', 23], ['Lee', 12],  ['Chung', 28]], columns = ['Name', 'Age'])
a
```

<div class="code-example" markdown="1">

|    | Name   |   Age |
|---:|:-------|------:|
|  0 | Kim    |    23 |
|  1 | Lee    |    12 |
|  2 | Chung  |    28 |

</div>

\# 두 가지 방식으로 각각 복사하기
{: .fs-3  .text-grey-dk-000}

```python
just_copy = a   ## a = b 방식의 '깊은 복사'
pandas_copy = a.copy()   ## 복사 당시의 상태만 복사하는 '얕은 복사'
```

\# 원본 데이터 변경해보기
{: .fs-3  .text-grey-dk-000}

```python
print(a, '\n')
print(just_copy, '\n')  # 단순히 a = b 방식으로 한 '깊은 복사'의 경우, 원데이터가 변하면 함께 따라 변한다
print(pandas_copy)  # a를 바꾸기 전 상태를 보존하려면, 이렇게 df.copy() 방식을 써야 한다
```
```
   Name  Age
0  변경됨!   23
1  변경됨!   12
2  변경됨!   28 

   Name  Age
0  변경됨!   23
1  변경됨!   12
2  변경됨!   28 

    Name  Age
0    Kim   23
1    Lee   12
2  Chung   28
```

## CSV, Excel 데이터 취급

### CSV 파일 읽고 쓰기
1. CSV 파일을 DataFrame으로 읽어오기
    ```python
    df = pd.read_csv('경로/파일명.csv', header=1, index_col=0, encoding='utf-8')  
                    # 같은 폴더 안에 있는 파일은 '파일명.csv'라고만 써줘도 됨
    ```
    - `header=1`이라고 하면 파일의 두번째 행(index_num=1)이 header로 설정됨 (첫번째 행은 읽어오지 않음)
        - `header=None`이라고 하면 첫 행이 header가 되는 대신 0부터 시작하는 숫자로 header가 채워짐 (default: csv 파일의 첫 행이 header로 들어감)
    - `index_col=0`이라고 하면 첫 번째 열이 index로 설정됨 (default: None)
        - `index_col='열이름'` 이런 식으로 열의 이름을 직접 str으로 넣어줘도 된다
    - 한글 등 글자가 깨질 때는 `encoding='utf-8'`과 같이 인코딩 방식을 설정해줄 수도 있다

1. DataFrame을 CSV 파일로 내보내기
    ```python
    df.to_csv('경로/파일명.cvs', index=False)   # 같은 폴더 안으로 저장하려면 '파일명.csv'라고만 써줘도 됨
    ```
    - 읽어올 때와 마찬가지로, `encoding='utf-8'`과 같은 옵션도 지정 가능
    - `index=False`라고 하면 index는 저장되지 않음 (보통 index는 0부터 시작하는 숫자로 채워지기에, 굳이 저장하지 않아도 되는 경우가 많음)


### Excel 파일 읽고 쓰기
1. Excel 파일을 DataFrame으로 읽어오기
    ```python
    df = pd.read_excel('파일명.xlsx', sheet_name='Sheet2', usecols="C:X", header=2, encoding='utf-8')
    # index_col=1이나 header=2 같은 방식으로 어느 열을 index로 가져갈건지, 어느 행을 header로 삼을건지 지정 가능.
    # usecols='C:X' 혹은 usecols='A,C,D' 이런 식으로해서 사용할 열 지정 가능
    ```
    - `sheet_name='Sheet2'`는 파일에서 Sheet2만 가져오겠다는 의미. 
        - sheet_name에는 int/str/list를 넣어줄 수 있다 (ex. `sheet_name=0`, `sheet_name=[0, 1, "Sheet5"]`)
    - `usecols='C:X`는 C~X까지의 열을 가져오겠다는 의미. (C, X도 포함)
        - `usecols='A,C,D'`이렇게 지정하는 것도 가능. (A, C, D 열만 가져오겠다는 의미)
    - `index_col`이나 `header`, `encoding`은 read_csv에서와 같은 기능

1. DataFrame을 Excel 파일로 내보내기
    ```python
    df.to_excel('파일명.xlsx')
    ```
    - `encoding='utf-8'`이나 `index=False`와 같은 옵션 추가 가능

1. 여러 개의 DataFrame을 서로 다른 시트로 해서 Excel 파일로 저장
    ```python
    writer = pd.ExcelWriter('파일명.xlsx', engine='xlsxwriter')

    # Write each dataframe to a different worksheet.
    df1.to_excel(writer, sheet_name='df1', encoding='utf-8')  # encoding='utf-8'은 옵션.
    df2.to_excel(writer, sheet_name='df2', encoding='utf-8')
    df3.to_excel(writer, sheet_name='df3', encoding='utf-8')


    # Close the Pandas Excel writer and output the Excel file.
    writer.save()
    ```

## Indexing & Slicing

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

### Indexing
: `.loc[행 방향, 열 방향]`

1. 행 방향 & 열 방향
    ```python
    ## iPhone X의 메모리 가져오기
    iphone_df.loc['iPhone X', '메모리']
    ```
    ```
    '3GB'
    ```

1. 행 방향 
    ```python
    ## iPhone X에 대한 모든 정보 가져오기
    iphone_df.loc['iPhone X', :]      ## iphone_df.loc['iPhone X'] 이렇게 써도 동일한 결과.
    ```
    ```
    출시일        2017-11-03
    디스플레이             5.8
    메모리               3GB
    출시 버전        iOS 11.1
    Face ID           Yes
    Name: iPhone X, dtype: object
    ```
    ※ 하나의 행/열만 추출하면 'Pandas Series' 형태로 가져와짐
    {: .fs-3}
    ```python
    type(iphone_df.loc['iPhone X'])
    ```
    ```
    pandas.core.series.Series
    ```
1. 행 방향 - 2개 이상
    ```python
    # iPhone X와 iPhone 8에 대한 정보 가져오기
    iphone_df.loc[['iPhone X', 'iPhone 8']]
    ```

    <div class="code-example" markdown="1">

    |          | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:---------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone X | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone 8 | 2017-09-22 |          4.7 | 2GB      | iOS 11.0    | No        |

    </div>

    ※ 두 개 이상의 행/열을 추출하면 'Pandas DataFrame' 형태로 가져와짐
    {: .fs-3}
    ```python
    type(iphone_df.loc[['iPhone X', 'iPhone 8']])
    ```
    ```
    pandas.core.frame.DataFrame
    ```

1. 열 방향
    ```python
    # 각 iPhone 모델에 대한 출시일 정보만 다 가져오기
    iphone_df.loc[:,'출시일']     ## iphone_df['출시일'] 이렇게 써도 동일한 결과. (loc 사용X)
    ```
    ```
    iPhone 7         2016-09-16
    iPhone 7 Plus    2016-09-16
    iPhone 8         2017-09-22
    iPhone 8 Plus    2017-09-22
    iPhone X         2017-11-03
    iPhone XS        2018-09-21
    iPhone XS Max    2018-09-21
    Name: 출시일, dtype: object
    ```
    *사실 열 방향 indexing은 `iphone_df['출시일']`과 같이 loc을 사용하지 않는 게 더 간단하다

1. 열 방향 - 2개 이상
    ```python
    # 출시일과 디스플레이 정보 가져오기
    iphone_df[['출시일', '디스플레이']]   ## iphone_df.loc[:,['출시일', '디스플레이']] 이렇게 써도 동일한 결과.
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 |
    |:--------------|:-----------|-------------:|
    | iPhone 7      | 2016-09-16 |          4.7 |
    | iPhone 7 Plus | 2016-09-16 |          5.5 |
    | iPhone 8      | 2017-09-22 |          4.7 |
    | iPhone 8 Plus | 2017-09-22 |          5.5 |
    | iPhone X      | 2017-11-03 |          5.8 |
    | iPhone XS     | 2018-09-21 |          5.8 |
    | iPhone XS Max | 2018-09-21 |          6.5 |

    </div>


### Slicing
```python
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

1. 행 방향
    ```python
    # iPhone 8부터 iPhone XS까지의 모든 row 반환
    iphone_df.loc['iPhone 8':'iPhone XS']
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone 8      | 2017-09-22 |          4.7 | 2GB      | iOS 11.0    | No        |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |

    </div>

1. 열 방향
    ```python
    # 메모리부터 Face ID까지의 column 반환
    iphone_df.loc[:, '메모리':'Face ID']
    ```

    <div class="code-example" markdown="1">

    |               | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:---------|:------------|:----------|
    | iPhone 7      | 2GB      | iOS 10.0    | No        |
    | iPhone 7 Plus | 3GB      | iOS 10.0    | No        |
    | iPhone 8      | 2GB      | iOS 11.0    | No        |
    | iPhone 8 Plus | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 4GB      | iOS 12.0    | Yes       |

    </div>

    ※ 주의! indexing과 다르게, slicing은 loc을 안쓰고 직접할 수는 없음!
    ```python
    # loc 안쓰고 column을 slicing하려고 하면 아래와 같이 이상하게 뜬다
    iphone_df['메모리':'Face ID'] 
    ```

    <div class="code-example" markdown="1">

    | 출시일   | 디스플레이   | 메모리   | 출시 버전   | Face ID   |

    </div>

1. 행 방향 & 열 방향
    ```python
    ## row, column 모두 slicing하기
    iphone_df.loc['iPhone 7':'iPhone X', '메모리':'Face ID']  #순서는 무조건 row 다음 column
    ```
    <div class="code-example" markdown="1">

    |               | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:---------|:------------|:----------|
    | iPhone 7      | 2GB      | iOS 10.0    | No        |
    | iPhone 7 Plus | 3GB      | iOS 10.0    | No        |
    | iPhone 8      | 2GB      | iOS 11.0    | No        |
    | iPhone 8 Plus | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 3GB      | iOS 11.1    | Yes       |

    </div>

### 조건으로 Indexing
```python
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

1. 디스플레이가 5인치 이상인 스마트폰 정보만 추출하기
    ```python
    iphone_df.loc[iphone_df['디스플레이'] > 5]
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

    </div>

1. FaceID가 가능한 스마트폰 정보만 추출
    ```python
    iphone_df.loc[iphone_df['Face ID'] == 'Yes']
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

    </div>

1. 디스플레이가 5인치 이상 "AND" FaceID가 가능한 스마트폰 정보 추출
    ```python
    condition = (iphone_df['디스플레이'] > 5) & (iphone_df['Face ID'] == 'Yes')
    iphone_df[condition]
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

    </div>

1. 디스플레이가 5인치 이상 "OR" FaceID가 가능한 스마트폰 정보 추출
    ```python
    condition = (iphone_df['디스플레이'] > 5) | (iphone_df['Face ID'] == 'Yes')
    iphone_df[condition]
    ```

    <div class="code-example" markdown="1">

    |               | 출시일     |   디스플레이 | 메모리   | 출시 버전   | Face ID   |
    |:--------------|:-----------|-------------:|:---------|:------------|:----------|
    | iPhone 7 Plus | 2016-09-16 |          5.5 | 3GB      | iOS 10.0    | No        |
    | iPhone 8 Plus | 2017-09-22 |          5.5 | 3GB      | iOS 11.0    | No        |
    | iPhone X      | 2017-11-03 |          5.8 | 3GB      | iOS 11.1    | Yes       |
    | iPhone XS     | 2018-09-21 |          5.8 | 4GB      | iOS 12.0    | Yes       |
    | iPhone XS Max | 2018-09-21 |          6.5 | 4GB      | iOS 12.0    | Yes       |

    </div>

### 숫자 위치 기반 Indexing & Slicing
: `.iloc[행 방향, 열 방향]`
  
※ iloc: integer + location. 숫자 기반으로 위치에 접근해 indexing할 때는 iloc 사용
{: .fs-3}

```python
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

1. 3번째 행 & 4번째 열의 값 추출
    ```python
    iphone_df.iloc[2, 4]
    ```
    ```
    'No'
    ```

1. 2번째,4번째 행 & 2번째,5번째 열 추출
    ```python
    iphone_df.iloc[[1,3], [1,4]]
    ```

    <div class="code-example" markdown="1">

    |               |   디스플레이 | Face ID   |
    |:--------------|-------------:|:----------|
    | iPhone 7 Plus |          5.5 | No        |
    | iPhone 8 Plus |          5.5 | No        |

    </div>

1. iloc으로 slicing
    ```python
    iphone_df.iloc[3:, 1:4]
    ```

    <div class="code-example" markdown="1">

    |               |   디스플레이 | 메모리   | 출시 버전   |
    |:--------------|-------------:|:---------|:------------|
    | iPhone 8 Plus |          5.5 | 3GB      | iOS 11.0    |
    | iPhone X      |          5.8 | 3GB      | iOS 11.1    |
    | iPhone XS     |          5.8 | 4GB      | iOS 12.0    |
    | iPhone XS Max |          6.5 | 4GB      | iOS 12.0    |

    </div>

    - 행: 4번째 행(index_num=3)부터 끝까지 
    - 열: 2번째 열(index_num=1)부터 4번째 열(index_num=3)까지 
        - 1:4로 slicing했으니 5번째 열(index_num=4)는 포함되지 않는다 (list에서의 slicing과 동일)
        - cf) `iphone_df.loc['iPhone 8':'iPhone XS']`에서는 iPhone8에서 iPhone XS까지 양 끝 모두 포함