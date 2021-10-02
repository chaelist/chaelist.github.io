---
layout: default
title: Pandas str, dt, 조건문
parent: Pandas
nav_order: 5
---

# Pandas str, dt, 조건문
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


## 문자열 처리 함수 'str'
- DataFrame에 직접 적용은 안되고, Series에 적용해야 한다

```python
df = pd.read_csv('data/전국_법정동코드.txt', sep='\t', encoding='cp949')  ## 데이터 출처: 행정표준코드관리시스템 
df.head()
```

<div class="code-example" markdown="1">

|    |   법정동코드 | 법정동명                 | 폐지여부   |
|---:|-------------:|:-------------------------|:-----------|
|  0 |   1100000000 | 서울특별시               | 존재       |
|  1 |   1111000000 | 서울특별시 종로구        | 존재       |
|  2 |   1111010100 | 서울특별시 종로구 청운동 | 존재       |
|  3 |   1111010200 | 서울특별시 종로구 신교동 | 존재       |
|  4 |   1111010300 | 서울특별시 종로구 궁정동 | 존재       |

</div>

### 문자열 인덱싱: str[ ]
- 기존의 string indexing 방법과 동일 [(참고)](../../python_basics/numbers_list_string/#string-기초)

```python
df['법정동명'].str[:5].head()  # 앞 5자리까지 추출
```
```
0    서울특별시
1    서울특별시
2    서울특별시
3    서울특별시
4    서울특별시
Name: 법정동명, dtype: object
```


### 문자열 분할: str.split( )
- string에 split()을 적용하는 방법과 동일 [(참고)](../../python_basics/numbers_list_string/#main-string-fuctions)

```python
df['법정동명'].str.split().head()   # 공백 기준 분리
```
```
0              [서울특별시]
1         [서울특별시, 종로구]
2    [서울특별시, 종로구, 청운동]
3    [서울특별시, 종로구, 신교동]
4    [서울특별시, 종로구, 궁정동]
Name: 법정동명, dtype: object
```

+) 분할된 개별 리스트를 바로 데이터 프레임으로 만드려면, `expand=True`옵션을 추가
```python
df['법정동명'].str.split(" ", expand=True).head()
    ## df['법정동명'].str.split(expand=True).head()라고만 해도 됨 (공백 기준으로 분리하는 경우)
```

<div class="code-example" markdown="1">

|    | 0          | 1      | 2      | 3   | 4   |
|---:|:-----------|:-------|:-------|:----|:----|
|  0 | 서울특별시 |  None  | None   | None   | None   |
|  1 | 서울특별시 | 종로구 | None   | None   | None   |
|  2 | 서울특별시 | 종로구 | 청운동 | None   |  None  |
|  3 | 서울특별시 | 종로구 | 신교동 | None   | None   |
|  4 | 서울특별시 | 종로구 | 궁정동 | None   |  None  |

</div>

### 특정 글자 기준 필터링
1. **str.startswith()**: 특정 글자로 시작하는 데이터만 필터링
    ```python
    # '대전'으로 시작하는 데이터만 필터링
    df[df['법정동명'].str.startswith("대전")].head()
    ```

    <div class="code-example" markdown="1">

    |      |   법정동코드 | 법정동명             | 폐지여부   |
    |-----:|-------------:|:---------------------|:-----------|
    | 3870 |   3000000000 | 대전광역시           | 존재       |
    | 3871 |   3011000000 | 대전광역시 동구      | 존재       |
    | 3872 |   3011010100 | 대전광역시 동구 원동 | 존재       |
    | 3873 |   3011010200 | 대전광역시 동구 인동 | 존재       |
    | 3874 |   3011010300 | 대전광역시 동구 효동 | 존재       |

    </div>

1. **str.endswith()**: 특정 글자로 끝나는 데이터만 필터링
    ```python
    # '구'으로 끝나는 데이터만 필터링
    df[df['법정동명'].str.endswith("구")].head()
    ```

    <div class="code-example" markdown="1">

    |     |   법정동코드 | 법정동명          | 폐지여부   |
    |----:|-------------:|:------------------|:-----------|
    |   1 |   1111000000 | 서울특별시 종로구 | 존재       |
    |  94 |   1114000000 | 서울특별시 중구   | 존재       |
    | 179 |   1117000000 | 서울특별시 용산구 | 존재       |
    | 229 |   1120000000 | 서울특별시 성동구 | 존재       |
    | 332 |   1121500000 | 서울특별시 광진구 | 존재       |

    </div>

1. **str.contains()**: 특정 글자를 포함하는 데이터만 필터링

    ```python
    # '서대문구'가 들어간 데이터만 필터링
    df[df['법정동명'].str.contains("서대문구")].head()
    ```

    <div class="code-example" markdown="1">

    |     |   법정동코드 | 법정동명                      | 폐지여부   |
    |----:|-------------:|:------------------------------|:-----------|
    | 597 |   1141000000 | 서울특별시 서대문구           | 존재       |
    | 635 |   1141010100 | 서울특별시 서대문구 충정로2가 | 존재       |
    | 636 |   1141010200 | 서울특별시 서대문구 충정로3가 | 존재       |
    | 637 |   1141010300 | 서울특별시 서대문구 합동      | 존재       |
    | 638 |   1141010400 | 서울특별시 서대문구 미근동    | 존재       |

    </div>

### 정규식 & findall 활용
- 특정 정규식에 매칭되는 값 추출하기  [(참고: 정규식)](../../data_handling/regular_expressions)

```python
df['법정동명'].str.findall('\w+동').head()
```
```
0       []
1       []
2    [청운동]
3    [신교동]
4    [궁정동]
Name: 법정동명, dtype: object
```

### 문자 대체: str.replace( )
- string에 replace()을 적용하는 방법과 동일 [(참고)](../../python_basics/numbers_list_string/#main-string-fuctions)

```python
df['법정동명'].str.replace(" ", "_").head()  # 공백을 "_"로 대체
```
```
0            서울특별시
1        서울특별시_종로구
2    서울특별시_종로구_청운동
3    서울특별시_종로구_신교동
4    서울특별시_종로구_궁정동
Name: 법정동명, dtype: object
```


### 문자열 패딩
- 고정된 길이로, 남는 부분 채우기

```python
# 문자열 길이를 20자로 맞추려고 함 & 남는 만큼 왼쪽을 "_"로 채우기
df['법정동명'].str.pad(width=20, side='left', fillchar='_').head()
     ## side='right'이라고 하면 오른쪽이 "-"로 채워짐
```
```
0    _______________서울특별시
1    ___________서울특별시 종로구
2    _______서울특별시 종로구 청운동
3    _______서울특별시 종로구 신교동
4    _______서울특별시 종로구 궁정동
Name: 법정동명, dtype: object
```

+) `side=`를 지정해주지 않으면 (=default) 좌우가 균일하게 채워진다. (글자가 가운데로.)
```python
# 글자를 가운데에 두고 좌우로 "_"를 채워서 문자열 길이 20자를 맞춰줌
df['법정동명'].str.center(width=20, fillchar='_').head()
```
```
0    _______서울특별시________
1    _____서울특별시 종로구______
2    ___서울특별시 종로구 청운동____
3    ___서울특별시 종로구 신교동____
4    ___서울특별시 종로구 궁정동____
Name: 법정동명, dtype: object
```


### 양 끝 공백 제거: str.strip( )
- string에 strip()을 적용하는 방법과 동일 [(참고)](../../python_basics/numbers_list_string/#main-string-fuctions)
- rstip()과 lstrip()도 적용 가능.

```python
df2 = pd.DataFrame({'col1':['abcde  ',' FFFFghij ','abCCe   '],
                    'col2':['   fgHAAij  ',' fghij ','lmnop   ']})
df2
```

<div class="code-example" markdown="1">

|    | col1     | col2    |
|---:|:---------|:--------|
|  0 | abcde    | fgHAAij |
|  1 | FFFFghij | fghij   |
|  2 | abCCe    | lmnop   |

</div>

&nbsp;
{: .fs-1 .lh-0}

```python
df2['col1'].str.strip()  # 앞 뒤 공백 제거
    ## 마찬가지로, rstip()과 lstrip()도 적용 가능!
```
```
0       abcde
1    FFFFghij
2       abCCe
Name: col1, dtype: object
```
→ list로 변환해서 살펴보면 확실히 앞 뒤 공백이 제거되었음을 확인 가능
```python
df2['col1'].str.strip().to_list()
```
```
['abcde', 'FFFFghij', 'abCCe']
```

### 대소문자 변경

1. **str.lower()**: 소문자로 변경
```python
df2['col1'].str.lower()
```
```
0       abcde  
1     ffffghij 
2      abcce   
Name: col1, dtype: object
```

1. **str.upper()**: 대문자로 변경
```python
df2['col1'].str.upper()
```
```
0       ABCDE  
1     FFFFGHIJ 
2      ABCCE   
Name: col1, dtype: object
```

1. **str.swapcase()**: 소문자 ↔ 대문자 서로 바꿔줌
```python
df2['col1'].str.swapcase()  # 소문자는 대문자로, 대문자는 소문자로
```
```
0       ABCDE  
1     ffffGHIJ 
2      ABccE   
Name: col1, dtype: object
```



## 날짜형 데이터 변환/가공

### 'Datetime' 타입으로 변환/가공

```python
transaction = pd.read_csv('data/transaction_1.csv')  ## 데이터 출처: https://github.com/wikibook/pyda100
transaction.head()
```

<div class="code-example" markdown="1">

|    | transaction_id   |   price | payment_date        | customer_id   |
|---:|:-----------------|--------:|:--------------------|:--------------|
|  0 | T0000000113      |  210000 | 2019-02-01 01:36:57 | PL563502      |
|  1 | T0000000114      |   50000 | 2019-02-01 01:37:23 | HD678019      |
|  2 | T0000000115      |  120000 | 2019-02-01 02:34:19 | HD298120      |
|  3 | T0000000116      |  210000 | 2019-02-01 02:47:23 | IK452215      |
|  4 | T0000000117      |  170000 | 2019-02-01 04:33:46 | PL542865      |

</div>

***pd.to_datetime()**: 데이터를 datetime type으로 변환해주는 함수

1. datetime 형태로 생긴 데이터: 그냥 pd.to_datetime() 안에 넣어주면 알아서 연/월/일 등이 식별된다
    ```python
    print(transaction['payment_date'].dtypes)

    # pd.to_datetime 함수를 활용해 datetime type으로 변환
    transaction['payment_date'] = pd.to_datetime(transaction['payment_date'])

    print(transaction['payment_date'].dtypes)
    ```
    ```
    object
    datetime64[ns]
    ```

1. str, int 등 타입의 데이터 변환하기 (연/월/일의 구조가 모호한 경우)
- `format=`을 통해 어떤 형태로 데이터가 담겨 있는지 명시해주면 알맞게 datetime으로 변환 가능
- ex) 데이터가 '12/08/2021'의 형태라면, `format='%d/%m/%Y`이라고 명시

    ```python
    df = pd.DataFrame({'date_example': [201901, 201902, 201903, 201904, 201905]})

    df['datetime'] = pd.to_datetime(df['date_example'], format='%Y%m')
    df
    ```

    <div class="code-example" markdown="1">

    |    |   date_example | datetime            |
    |---:|---------------:|:--------------------|
    |  0 |         201901 | 2019-01-01 00:00:00 |
    |  1 |         201902 | 2019-02-01 00:00:00 |
    |  2 |         201903 | 2019-03-01 00:00:00 |
    |  3 |         201904 | 2019-04-01 00:00:00 |
    |  4 |         201905 | 2019-05-01 00:00:00 |

    </div>

    +) 데이터 타입 확인:
    ```python
    df.dtypes
    ```
    ```
    date_example             int64
    datetime        datetime64[ns]
    dtype: object
    ```


### Datetime 데이터 가공하기
1. **.dt.strftime()**: 날짜와 시간 정보를 지정한 특정 문자열 형태로 바꿔준다
    - %Y: 앞의 빈자리를 0으로 채우는 4자리 연도 숫자
    - %m: 앞의 빈자리를 0으로 채우는 2자리 월 숫자
    - %d: 앞의 빈자리를 0으로 채우는 2자리 일 숫자
    - `.dt.strftime("%Y년 %m월 %d일")` 이런 식으로 (= '2020년 12월 25일'의 형식) 정리하는 것도 가능.

    ```python
    # '201902'와 같은 형태로 날짜를 연월 단위로 정리
    transaction['payment_month'] = transaction['payment_date'].dt.strftime('%Y%m')
    transaction[['payment_date', 'payment_month']].head()
    ```

    <div class="code-example" markdown="1">

    |    | payment_date        |   payment_month |
    |---:|:--------------------|----------------:|
    |  0 | 2019-02-01 01:36:57 |          201902 |
    |  1 | 2019-02-01 01:37:23 |          201902 |
    |  2 | 2019-02-01 02:34:19 |          201902 |
    |  3 | 2019-02-01 02:47:23 |          201902 |
    |  4 | 2019-02-01 04:33:46 |          201902 |

    </div>

    → 이렇게 월 단위로 정리해놓으면, 월별 금액 비교 등이 용이하다.
    ```python
    # month 기준으로 price의 합 집계
    transaction.groupby('payment_month').sum()['price']   
        ## transaction.groupby('payment_month')['price'].sum()라고 해도 됨.
    ```
    ```
    payment_month
    201902    160185000
    201903    160370000
    201904    160510000
    201905    155420000
    201906     74755000
    Name: price, dtype: int64
    ```

    cf) strftime() 메소드로 정리한 데이터는 str 타입 (= object 타입)
    ```python
    transaction[['payment_date', 'payment_month']].dtypes
    ```
    ```
    payment_date     datetime64[ns]
    payment_month            object
    dtype: object
    ```

1. **.dt.weekday**: 해당 날짜의 요일을 계산해준다
    - 요일은 숫자로 표시 (월:0, 화:1, ...., 토:5, 일:6)

    ```python
    transaction['weekday'] = transaction['payment_date'].dt.weekday 
    transaction.head()
    ```

    <div class="code-example" markdown="1">

    |    | transaction_id   |   price | payment_date        | customer_id   |   payment_month |   weekday |
    |---:|:-----------------|--------:|:--------------------|:--------------|----------------:|----------:|
    |  0 | T0000000113      |  210000 | 2019-02-01 01:36:57 | PL563502      |          201902 |         4 |
    |  1 | T0000000114      |   50000 | 2019-02-01 01:37:23 | HD678019      |          201902 |         4 |
    |  2 | T0000000115      |  120000 | 2019-02-01 02:34:19 | HD298120      |          201902 |         4 |
    |  3 | T0000000116      |  210000 | 2019-02-01 02:47:23 | IK452215      |          201902 |         4 |
    |  4 | T0000000117      |  170000 | 2019-02-01 04:33:46 | PL542865      |          201902 |         4 |

    </div>

    +) **dt.year, dt.month, dt.day** 등의 속성도 있음
    ```python
    transaction['payment_date'].dt.year.head()   # 연도별로 정리
    ```
    ```
    0    2019
    1    2019
    2    2019
    3    2019
    4    2019
    Name: payment_date, dtype: int64
    ```

### 숫자로 읽혀온 날짜형 데이터 수정
: excel 날짜형 데이터가 숫자로 잘못 읽혀오는 경우, pd.to_timedelta()를 활용해 수정해줄 수 있다

```python
kokyaku_data = pd.read_excel('data/kokyaku_daicho.xlsx')  ## 데이터 출처: https://github.com/wikibook/pyda10
kokyaku_data.head()
```

<div class="code-example" markdown="1">

|    | 고객이름   | 지역   | 등록일              |
|---:|:-----------|:-------|:--------------------|
|  0 | 김 현성    | H시    | 2018-01-04 00:00:00 |
|  1 | 김 도윤    | E시    | 42782               |
|  2 | 김 지한    | A시    | 2018-01-07 00:00:00 |
|  3 | 김 하윤    | F시    | 42872               |
|  4 | 김 시온    | E시    | 43127               |

</div>
- '등록일' 칼럼의 몇몇 데이터가 날짜가 아닌 숫자형으로 읽혀온 것을 볼 수 있다

```python 
# 숫자로 읽혀온 데이터를 판별해, flg_is_serial에 저장 (True/False로)

flg_is_serial = kokyaku_data['등록일'].astype('str').str.isdigit()  ## isdigit()을 통해 숫자인지를 판별.
flg_is_serial.sum()
    ## 숫자로 된 날짜 정보가 22개임을 알 수 있음.
```
```
22
```

→ **to_timedelta()**를 활용해 숫자 데이터를 '~일' 데이터로 바꿔주고, 1900/01/01에 더하기
- 엑셀에서, 42782를 날짜 형태로 나타내면 '1900/01/01'을 기준으로 42782일을 더한, '2017/02/16'이 된다. (엑셀의 날짜 기억법)
- pd.to_timedelta(이름.astype('float'), unit='D')는 '42782 days'와 같은 형태로 숫자 데이터를 '~일' 데이터로 바꿔준다
    - unit='D'는 'days'를 의미. '~일'로 바꾸라는 것.

```python
# to_timedelta 활용
from_serial = pd.to_timedelta(kokyaku_data.loc[flg_is_serial, '등록일'].astype('float'), unit='D') + pd.to_datetime('1900/01/01')

from_serial.head() ## 날짜형으로 잘 변환된 것을 볼 수 있다
```
```
1    2017-02-18
3    2017-05-19
4    2018-01-29
21   2017-07-06
27   2017-06-17
Name: 등록일, dtype: datetime64[ns]
```

+) 예시로, to_timedelta, unit='D'가 어떤 방식으로 데이터를 변환하는지 보여주기 위해 이렇게 출력해봄:
```python
pd.to_timedelta(kokyaku_data.loc[flg_is_serial, '등록일'].astype('float'), unit='D').iloc[0]
```
```
Timedelta('42782 days 00:00:00')
```

+) 추가: 사실, '42782'가 엑셀에서는 '2017-02-16'라고 나온다. (위에서는 2017-02-18로 변환.)
- 1) 엑셀은 숫자가 0이 아닌 1부터 시작하고, 2) 1900년은 평년인데 1900/02/29일을 유효한 날짜로 계산하기 때문. (엑셀의 버그)
- 그래서 엑셀의 날짜 형식 숫자를 단순히 파이썬으로 계산하면 이틀이 어긋나는 것..
- 그래서 원래는 다음과 같이 -2를 해줘서 계산해야 엑셀 날짜에 맞출 수 있다.

```python
from_serial = pd.to_timedelta(kokyaku_data.loc[flg_is_serial, '등록일'].astype('float') - 2, unit='D') + pd.to_datetime('1900/01/01')
from_serial.head()
```
```
1    2017-02-16
3    2017-05-17
4    2018-01-27
21   2017-07-04
27   2017-06-15
Name: 등록일, dtype: datetime64[ns]
```


### relativedelta로 기간 계산
(relative delta: 날짜 비교 함수)

```python
customer = pd.read_csv('data/customer_master.csv')  ## 데이터 출처: https://github.com/wikibook/pyda10
customer.head() 
 
# 스포츠센터 회원 데이터. start_date는 가입일, end_date는 탈퇴일.
## end_date가 NaN인 것은 아직 탈퇴하지 않은 회원
```

<div class="code-example" markdown="1">

|    | customer_id   | name   | class   | gender   | start_date          |   end_date | campaign_id   |   is_deleted |
|---:|:--------------|:-------|:--------|:---------|:--------------------|-----------:|:--------------|-------------:|
|  0 | OA832399      | XXXX   | C01     | F        | 2015-05-01 00:00:00 |        NaN | CA1           |            0 |
|  1 | PL270116      | XXXXX  | C01     | M        | 2015-05-01 00:00:00 |        NaN | CA1           |            0 |
|  2 | OA974876      | XXXXX  | C01     | M        | 2015-05-01 00:00:00 |        NaN | CA1           |            0 |
|  3 | HD024127      | XXXXX  | C01     | F        | 2015-05-01 00:00:00 |        NaN | CA1           |            0 |
|  4 | HD661448      | XXXXX  | C03     | F        | 2015-05-01 00:00:00 |        NaN | CA1           |            0 |

</div>

→ start_date와 end_date 칼럼을 datetime 타입으로 변경

```python
customer['start_date'] = pd.to_datetime(customer['start_date'])
customer['end_date'] = pd.to_datetime(customer['end_date'])
customer.head()
    ## NaT는 datetime type의 NaN
```

<div class="code-example" markdown="1">

|    | customer_id   | name   | class   | gender   | start_date | end_date   | campaign_id   |   is_deleted |
|---:|:--------------|:-------|:--------|:---------|:--------------------|:-----------|:--------------|-------------:|
|  0 | OA832399      | XXXX   | C01     | F        | 2015-05-01 | NaT        | CA1           |            0 |
|  1 | PL270116      | XXXXX  | C01     | M        | 2015-05-01 | NaT        | CA1           |            0 |
|  2 | OA974876      | XXXXX  | C01     | M        | 2015-05-01 | NaT        | CA1           |            0 |
|  3 | HD024127      | XXXXX  | C01     | F        | 2015-05-01 | NaT        | CA1           |            0 |
|  4 | HD661448      | XXXXX  | C03     | F        | 2015-05-01 | NaT        | CA1           |            0 |

</div>

→ 가입~탈퇴까지의 회원 기간 계산하기

```python
from dateutil.relativedelta import relativedelta  # 날짜 비교 함수 relativedelta를 사용하기 위해 라이브러리 import

customer['calc_date'] = customer['end_date']  # 날짜 계산용 칼럼을 따로 복제
customer['calc_date'] = customer['calc_date'].fillna(pd.to_datetime('20190430')) # 결측치에는 일괄적으로 2019.04.30을 대입

customer['membership_period'] = 0
for i in range(len(customer)):
    delta = relativedelta(customer['calc_date'].iloc[i], customer['start_date'].iloc[i])
    customer['membership_period'].iloc[i] = delta.years*12 + delta.months  # 회원 기간을 월 단위로 계산 (몇 달 후에 탈퇴했나)

customer.head()
```

<div class="code-example" markdown="1">

|    | customer_id   | name   | class   | gender   | start_date  | end_date   | campaign_id   |   is_deleted | calc_date   |   membership_period |
|---:|:--------------|:-------|:--------|:---------|:------------|:-----------|:--------------|-------------:|:------------|---------:|
|  0 | OA832399      | XXXX   | C01     | F        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30|       47 |
|  1 | PL270116      | XXXXX  | C01     | M        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30|       47 |
|  2 | OA974876      | XXXXX  | C01     | M        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30 |      47 |
|  3 | HD024127      | XXXXX  | C01     | F        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30 |      47 |
|  4 | HD661448      | XXXXX  | C03     | F        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30 |      47 |

</div>

+) relativedelta 사용법 살펴보기
```python
delta = relativedelta(pd.to_datetime('20190430'), pd.to_datetime('20170301'))
print(delta)   ## 몇년 몇개월 며칠 차이 나는지 저장되어 있음
print(delta.years)
print(delta.months)
print(delta.days)
```
```
relativedelta(years=+2, months=+1, days=+29)
2
1
29
```

### +) 더 간단한 기간 계산
- 두 column이 동일한 datetime 타입을 가지고 있다면, 간단히 빼기(-)로 기간을 계산할 수 있다.
- 그 결과는 000 days와 같이 일수 기준으로 나오며, `.dt.days`를 붙여주면 int 형태로 일수만 추출된다


```python
# 위에서 relativedelta로 계산한 것과 달리, 'days' 기준의 회원 기간 계산
customer['membership_period'] = (customer['calc_date'] - customer['start_date']).dt.days
customer.head()
```

<div class="code-example" markdown="1">

|    | customer_id   | name   | class   | gender   | start_date  | end_date   | campaign_id   |   is_deleted | calc_date   |   membership_period |
|---:|:--------------|:-------|:--------|:---------|:------------|:-----------|:--------------|-------------:|:------------|---------:|
|  0 | OA832399      | XXXX   | C01     | F        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30|       1460 |
|  1 | PL270116      | XXXXX  | C01     | M        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30|       1460 |
|  2 | OA974876      | XXXXX  | C01     | M        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30 |      1460 |
|  3 | HD024127      | XXXXX  | C01     | F        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30 |      1460 |
|  4 | HD661448      | XXXXX  | C03     | F        | 2015-05-01  | NaT        | CA1           |            0 | 2019-04-30 |      1460 |

</div>



## 조건문으로 데이터 가공

### lambda 식 + apply()
: `series.apply(lambda x: 식)`과 같은 형식으로 사용.

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

1. 각 줄의 'model'명의 길이를 세서 model_len이라는 칼럼으로 저장하기

    ```python
    laptops_df['model_len'] = laptops_df['model'].apply(lambda x: len(x))
    laptops_df[['model', 'model_len']].head()
    ```

    <div class="code-example" markdown="1">

    |    | model            |   model_len |
    |---:|:-----------------|------------:|
    |  0 | Inspiron 15-3567 |          16 |
    |  1 | MacBook Air      |          11 |
    |  2 | MacBook Air      |          11 |
    |  3 | MacBook Pro      |          11 |
    |  4 | MacBook Pro      |          11 |

    </div>

2. lambda 식에서 if-else 절 사용: 각 줄의 가격이 비싼지 아닌지 정의하는 새로운 칼럼 생성하기

    ```python
    # price 칼럼의 평균 -- 아래에서 Expensive 여부 판단 기준으로 사용
    laptops_df['price'].mean() 
    ```
    ```
    64132.89820359281
    ```

    → 각 행의 'price'를 보고 64133 이상이면 Expensive, 아니면 Affordable이라고 정의하는 새로운 열 생성

    ```python
    laptops_df['Expensive_Affordable'] = laptops_df['price'].apply(lambda x: 'Expensive' if x > 64133 else 'Affordable')
    laptops_df[['price','Expensive_Affordable']].head()
    ```

    <div class="code-example" markdown="1">

    |    |   price | Expensive_Affordable   |
    |---:|--------:|:-----------------------|
    |  0 |   40000 | Affordable             |
    |  1 |   55499 | Affordable             |
    |  2 |   71500 | Expensive              |
    |  3 |   96890 | Expensive              |
    |  4 |  112666 | Expensive              |

    </div>

1. lambda 식에 함수를 받아서 사용: 각 줄의 가격을 세분화해 분류하는 새로운 칼럼 생성하기

    ```python
    # price 칼럼의 분포 -- 아래에서 카테고리 판단 기준으로 사용
    laptops_df['price'].describe()  
    ```
    ```
    count       167.000000
    mean      64132.898204
    std       42797.674010
    min       13872.000000
    25%       35457.500000
    50%       47990.000000
    75%       77494.500000
    max      226000.000000
    Name: price, dtype: float64
    ```

    → 각 행의 'price'를 보고 아래 get_category() 함수에 따른 분류를 적어주는 새로운 열 생성

    ```python
    def get_category(price):
    if price <= 35457: cat = 'cheap'
    elif price <= 47990: cat = 'affordable'
    elif price <= 77494: cat = 'expensive'
    else: cat = 'above budget'
    return cat

    laptops_df['price_cat'] = laptops_df['price'].apply(lambda x: get_category(x))
    laptops_df[['price', 'price_cat']].head(10)
    ```

    <div class="code-example" markdown="1">

    |    |   price | price_cat    |
    |---:|--------:|:-------------|
    |  0 |   40000 | affordable   |
    |  1 |   55499 | expensive    |
    |  2 |   71500 | expensive    |
    |  3 |   96890 | above budget |
    |  4 |  112666 | above budget |
    |  5 |  226000 | above budget |
    |  6 |  158000 | above budget |
    |  7 |   96990 | above budget |
    |  8 |   33225 | cheap        |
    |  9 |   21990 | cheap        |

    </div>

### lambda 식 + applymap()
: `series.applymap(lambda x: 식)`과 같은 형식으로 사용.
- df.apply()의 경우 row / column basis로 작용하고, df.applymap()의 경우 element-wise로 작용한다는 차이가 있기 때문에, 각 element를 하나 하나 조건식으로 검사해서 변경하고자 하는 경우에는 applymap()을 사용해야 한다

```python
df = pd.DataFrame({
    'A': [0, 1, 2, 3],
    'B': [1, 0, 0, 0],
    'C': [3, 0, 2, 0],
    'D': [0, 0, 1, 4]})

df
```

<div class="code-example" markdown="1">

|    |   A |   B |   C |   D |
|---:|----:|----:|----:|----:|
|  0 |   0 |   1 |   3 |   0 |
|  1 |   1 |   0 |   0 |   0 |
|  2 |   2 |   0 |   2 |   1 |
|  3 |   3 |   0 |   0 |   4 |

</div>

→ 유무만을 나타내기 위해, 0보다 큰 숫자는 모두 'O'로, 그 외는 공백으로 변경:
```python
df.applymap(lambda x: 'O' if x > 0 else '')
```

<div class="code-example" markdown="1">

|    | A   | B   | C   | D   |
|---:|:----|:----|:----|:----|
|  0 |     | O   | O   |     |
|  1 | O   |     |     |     |
|  2 | O   |     | O   | O   |
|  3 | O   |     |     | O   |

</div>


### pd.where()
: `series.where(series객체에 대한 조건문, 거짓 값에 대한 대체 값)`의 형태로 사용

```python
df = pd.DataFrame({'a': [1, 2, 3, 4, 5], 'b': [10, 20, 30, 40, 50]})
df
```

<div class="code-example" markdown="1">

|    |   a |   b |
|---:|----:|----:|
|  0 |   1 |  10 |
|  1 |   2 |  20 |
|  2 |   3 |  30 |
|  3 |   4 |  40 |
|  4 |   5 |  50 |

</div>

1. seriesA.where(sereiesA에 대한 조건, 값)

    ```python
    df['c'] = df['a'].where(df['a'] < 3, 10)
    df
    ```

    <div class="code-example" markdown="1">

    |    |   a |   b |   c |
    |---:|----:|----:|----:|
    |  0 |   1 |  10 |   1 |
    |  1 |   2 |  20 |   2 |
    |  2 |   3 |  30 |  10 |
    |  3 |   4 |  40 |  10 |
    |  4 |   5 |  50 |  10 |

    </div>

1. seriesB.where(sereiesA에 대한 조건, 값)

    ```python
    # a열 중 3보다 작은 값(1,2)에는 b열의 값, 3이상인 값에 대해서는 100을 넣는다
    df['d'] = df['b'].where(df['a'] < 3, 100)  
    df
    ```

    <div class="code-example" markdown="1">

    |    |   a |   b |   c |   d |
    |---:|----:|----:|----:|----:|
    |  0 |   1 |  10 |   1 |  10 |
    |  1 |   2 |  20 |   2 |  20 |
    |  2 |   3 |  30 |  10 | 100 |
    |  3 |   4 |  40 |  10 | 100 |
    |  4 |   5 |  50 |  10 | 100 |

    </div>


### np.where() 활용
: `np.where(조건, 조건이 부합할 때의 값, 아닐 때의 값)`의 형태로 사용

```python
import numpy as np

df = pd.DataFrame({'a':[1, 2, 3, 4, 5], 'b':[5, 4, 3, 2, 1]})

df['flag'] = np.where(df['a'] < df['b'], 'b is bigger', 'a is bigger')
df
```

<div class="code-example" markdown="1">

|    |   a |   b | flag        |
|---:|----:|----:|:------------|
|  0 |   1 |   5 | b is bigger |
|  1 |   2 |   4 | b is bigger |
|  2 |   3 |   3 | a is bigger |
|  3 |   4 |   2 | a is bigger |
|  4 |   5 |   1 | a is bigger |

</div>
