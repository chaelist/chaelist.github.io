---
layout: default
title: UK Ecommerce Data 2
parent: Kaggle Dataset EDA
nav_order: 9
---

# UK Ecommerce Data
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


***분석 대상 데이터셋: UK E-Commerce Data**
- [데이터셋 출처](https://www.kaggle.com/carrie1/ecommerce-data){: target="_blank"}
- UK-based retailer의 2020-12-01에서 2011-12-09 사이의 모든 거래를 기록한 데이터
- gift를 주로 판매하며, 대다수의 고객은 wholesaler임


## 데이터 정리

```python
# 필요한 라이브러리 import
import pandas as pd
import numpy as np

import scipy.stats as stats

from matplotlib import pyplot as plt
import seaborn as sns

import plotly.express as px
import plotly.io as pio

pio.templates.default = "plotly_white"  # default template을 지정
```

```python
ecom_df = pd.read_csv('data/uk_ecommerce_data.csv')
ecom_df.head()
```

<div class="code-example" markdown="1">

|    |   InvoiceNo | StockCode   | Description                         |   Quantity | InvoiceDate    |   UnitPrice |   CustomerID | Country        |
|---:|------------:|:------------|:------------------------------------|-----------:|:---------------|------------:|-------------:|:---------------|
|  0 |      536365 | 85123A      | WHITE HANGING HEART T-LIGHT HOLDER  |          6 | 12/1/2010 8:26 |        2.55 |        17850 | United Kingdom |
|  1 |      536365 | 71053       | WHITE METAL LANTERN                 |          6 | 12/1/2010 8:26 |        3.39 |        17850 | United Kingdom |
|  2 |      536365 | 84406B      | CREAM CUPID HEARTS COAT HANGER      |          8 | 12/1/2010 8:26 |        2.75 |        17850 | United Kingdom |
|  3 |      536365 | 84029G      | KNITTED UNION FLAG HOT WATER BOTTLE |          6 | 12/1/2010 8:26 |        3.39 |        17850 | United Kingdom |
|  4 |      536365 | 84029E      | RED WOOLLY HOTTIE WHITE HEART.      |          6 | 12/1/2010 8:26 |        3.39 |        17850 | United Kingdom |

</div>

1. 데이터 타입 정리

    ```python
    # Customer ID를 string 형태로 바꿔줌 
    ecom_df['CustomerID'].fillna(0, inplace=True)  # 일단 N/A를 0으로 바꿔줌
    ecom_df['CustomerID'] = ecom_df['CustomerID'].astype('int').astype('str')  # 정수로 -> str로 바꿔줌
    ecom_df.replace({'CustomerID': '0'}, 'N/A', inplace=True) # 0은 'N/A'로 바꿔줌

    # InvoiceDate를 datetime 형태로 바꿔줌
    ecom_df['InvoiceDate'] = pd.to_datetime(ecom_df['InvoiceDate'])
    ```

1. 중복값 제거

    ```python
    # 전체 값이 다 중복된 경우, 가장 위에 있는 행만 남기고 drop 
    ecom_df.drop_duplicates(inplace=True)
    ecom_df.reset_index(drop=True, inplace=True)
    ```

1. 데이터 정리 - 필요 없는 데이터 삭제

    ```python
    # UnitPrice = 0인 데이터는 매출과 관계가 없으므로 삭제
    drop_index = ecom_df[ecom_df['UnitPrice'] == 0].index
    ecom_df.drop(drop_index, axis='index', inplace=True)

    # CRUK, BANK CHARGES, B, AMAZONFEE, S 삭제 (제품 판매로 인한 매출과 관계 없는 Stock Code)
    drop_index = ecom_df.query("StockCode in ['CRUK', 'BANK CHARGES', 'B', 'S', 'AMAZONFEE']").index
    ecom_df.drop(drop_index, axis='index', inplace=True)

    # reset index
    ecom_df.reset_index(drop=True, inplace=True)
    ```

1. 분석이 용이하도록 새로운 칼럼 생성

    ```python
    # InvoiceMonth, InvoiceWeekday 칼럼 생성
    ecom_df['InvoiceMonth'] = ecom_df['InvoiceDate'].dt.strftime('%Y%m')  # YYYYmm 형식으로 정리
    ecom_df['InvoiceWeekday'] = ecom_df['InvoiceDate'].dt.weekday   # 월: 0 ~ 일: 6

    # TotalSpending 칼럼 생성: Quantity * UnitPrice
    ecom_df['TotalSpending'] = ecom_df['Quantity'] * ecom_df['UnitPrice']
    ```

1. CustomerID == N/A를 제외한 df를 따로 저장
- nan인 값은 '등록되지 않은 구매자' 개념인 듯. (비회원 구매 개념)
- 신규 이용자 / 이탈자 등을 분석할 때는 제외하는 것이 맞겠지만, 인기 상품 / 기간별 매출 등을 파악할 때에는 익명 이용자의 구매도 중요하므로, ecom_df와 ecom_no_na 두 개의 dataframe으로 나눠서 저장해둠

    ```python
    ecom_no_na = ecom_df.query('CustomerID != "N/A"')
    ```


## 6개월 재구매율
- 같은 e-commerce 사업이라도 재구매율과 재구매 주기는 취급 항목에 따라 다름
- 재구매율에 따라, 충성 고객 유지에 초점을 맞춘 마케팅이 필요할 수도 있고, 신규 고객 유치에 더 노력을 기울여야 할 수도 있다. (ex. 식료품 판매: 거의 매일 접속하는 이용자가 많음 vs 대형 가전 판매: 대부분 한 번 구매하면 몇 년 동안 사용)
- 주어진 데이터의 경우 1년 간의 사업 데이터이므로, 6개월 단위의 재구매율을 통해 사업 방향성을 점검

```python
# 상반기: 2010-12-01 ~ 2011-05-31
first_half = ecom_no_na.query('InvoiceDate < "2011-06-01"')
first_half_customers = set(first_half['CustomerID'].unique())

# 하반기: 2011-06-01 ~ 2011-11-30
second_half = ecom_no_na.query('InvoiceDate >= "2011-06-01" & InvoiceDate < "2011-12-01"')
second_half_customers = set(second_half['CustomerID'].unique())

print('상반기 구매자수: ', len(first_half_customers))
print('상반기&하반기 구매자수: ', len(first_half_customers & second_half_customers))
print('6개월 재구매율: ', len(first_half_customers & second_half_customers) / len(first_half_customers))
```
```
상반기 구매자수:  2767
상반기&하반기 구매자수:  1934
6개월 재구매율:  0.6989519335019877
```
- 6개월 재구매율: 약 69.9%
- 재구매율이 높은 편이므로, 충성도 높은 고객에게 초점을 맞춘 마케팅에 신경을 쓰면 좋을 듯 (ex. 포인트 제도 등)



## 고객 특성별 구매당 반품 비율
: 고객별 이탈 위험도 (risk ratio)와 구매당 반품 비율의 관계를 확인

1. 고객별 구매 패턴 정리

    ```python
    ecom_purchase = ecom_no_na.query('TotalSpending >= 0')  # TotalSpending이 양수인 값만 반영. (-는 반품한 내역으로 추정)
    temp = ecom_purchase.query('InvoiceMonth != "201112"')  # 201112는 제외하고 계산

    customer_usage = pd.pivot_table(temp, index='CustomerID', columns='InvoiceMonth', values='InvoiceNo', aggfunc='count')
    customer_usage = customer_usage.applymap(lambda x: 'O' if x > 0 else 'X')

    for index in customer_usage.index:
        templist = customer_usage.loc[index].to_list()
        
        month_btw = []
        for i in range(12):
            if templist[i] == 'O':
                for j in range(1, 12 - i):
                    if templist[i + j] == 'O':
                        month_btw.append(j)
                        break
        
        if month_btw:
            month_btw_avg = sum(month_btw) / len(month_btw)
        else:
            if (customer_usage.loc[index, '201111'] == 'O') or (customer_usage.loc[index, '201110'] == 'O'):
                month_btw_avg = 'New Customer'
            else:
                month_btw_avg = 'Never Returned'
        customer_usage.loc[index, 'avg_month_btw_purchases'] = month_btw_avg

    last_month = ecom_purchase.query('InvoiceMonth != "201112"').groupby('CustomerID')[['InvoiceMonth']].max()
    last_month.rename(columns={'InvoiceMonth':'LastMonth'}, inplace=True)
    customer_usage = customer_usage.join(last_month)

    for index in customer_usage.index:
        lastmonth = customer_usage.loc[index, 'LastMonth']
        if lastmonth == '201012':
            lastmonth = 201100
        customer_usage.loc[index, 'months_after_latest_purchase'] = 201112 - int(lastmonth)

    customer_usage.head()
    ```

    <div class="code-example" markdown="1">

    |   CustomerID | 201012   | 201101   | 201102   | 201103   | 201104   | 201105   | 201106   | 201107   | 201108   | 201109   | 201110   | 201111   | avg_month_btw_purchases   |   LastMonth |   months_after_latest_purchase |
    |-------------:|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:--------------------------|------------:|-------------------------------:|
    |        12346 | X        | O        | X        | X        | X        | X        | X        | X        | X        | X        | X        | X        | Never Returned            |      201101 |                             11 |
    |        12347 | O        | O        | X        | X        | O        | X        | O        | X        | O        | X        | O        | X        | 2.0                       |      201110 |                              2 |
    |        12348 | O        | O        | X        | X        | O        | X        | X        | X        | X        | O        | X        | X        | 3.0                       |      201109 |                              3 |
    |        12349 | X        | X        | X        | X        | X        | X        | X        | X        | X        | X        | X        | O        | New Customer              |      201111 |                              1 |
    |        12350 | X        | X        | O        | X        | X        | X        | X        | X        | X        | X        | X        | X        | Never Returned            |      201102 |                             10 |

    </div>

1. 구매 지속 고객의 risk ratio 계산 
- risk ratio(위험 비율): 마지막 구매 이후 경과한 기간 / 평균 구매 간격

    ```python
    likely_customer = customer_usage.query('avg_month_btw_purchases not in ["Never Returned", "New Customer"]')
    likely_customer['risk_ratio'] = likely_customer['months_after_latest_purchase'] / likely_customer['avg_month_btw_purchases']
    likely_customer.head()
    ```

    <div class="code-example" markdown="1">

    |   CustomerID | 201012   | 201101   | 201102   | 201103   | 201104   | 201105   | 201106   | 201107   | 201108   | 201109   | 201110   | 201111   |   avg_month_btw_purchases |   LastMonth |   months_after_latest_purchase |   risk_ratio |
    |-------------:|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|:---------|--------------------------:|------------:|-------------------------------:|-------------:|
    |        12347 | O        | O        | X        | X        | O        | X        | O        | X        | O        | X        | O        | X        |                         2 |      201110 |                              2 |     1        |
    |        12348 | O        | O        | X        | X        | O        | X        | X        | X        | X        | O        | X        | X        |                         3 |      201109 |                              3 |     1        |
    |        12352 | X        | X        | O        | O        | X        | X        | X        | X        | X        | O        | X        | O        |                         3 |      201111 |                              1 |     0.333333 |
    |        12356 | X        | O        | X        | X        | O        | X        | X        | X        | X        | X        | X        | O        |                         5 |      201111 |                              1 |     0.2      |
    |        12359 | X        | O        | O        | X        | X        | X        | O        | X        | X        | X        | O        | X        |                         3 |      201110 |                              2 |     0.666667 |

    </div>

1. 고객별 구매 대비 반품 비율 계산

    ```python
    # 고객별 Purchase와 Refunds 계산해서 join
    ecom_purchase = ecom_no_na.query('TotalSpending >= 0')
    ecom_purchase = ecom_purchase.groupby('CustomerID')[['Quantity']].sum()
    ecom_purchase.rename(columns={'Quantity':'Purchases'}, inplace=True)

    ecom_refund = ecom_no_na.query('TotalSpending < 0')
    ecom_refund = ecom_refund.groupby('CustomerID')[['Quantity']].sum() * -1
    ecom_refund.rename(columns={'Quantity':'Refunds'}, inplace=True)

    purchase_refund = ecom_purchase.join(ecom_refund, how='left')
    purchase_refund['Refunds'].fillna(0, inplace=True)


    # Refunds per Purchases 수치 계산
    purchase_refund['Re_per_Pur'] = purchase_refund['Refunds'] / purchase_refund['Purchases'] * 100
    purchase_refund.head()
    ```

    <div class="code-example" markdown="1">

    |   CustomerID |   Purchases |   Refunds |   Re_per_Pur |
    |-------------:|------------:|----------:|-------------:|
    |        12346 |       74215 |     74215 |          100 |
    |        12347 |        2458 |         0 |            0 |
    |        12348 |        2341 |         0 |            0 |
    |        12349 |         631 |         0 |            0 |
    |        12350 |         197 |         0 |            0 |

    </div>

1. 구매당 반품수 & 고객 특성 join

    ```python
    purchase_refund = purchase_refund.join(customer_usage['avg_month_btw_purchases'], how='left')
    purchase_refund = purchase_refund.join(customer_usage['months_after_latest_purchase'], how='left')
    purchase_refund = purchase_refund.join(likely_customer['risk_ratio'], how='left')
    purchase_refund.head()
    ```

    <div class="code-example" markdown="1">

    |   CustomerID |   Purchases |   Refunds |   Re_per_Pur | avg_month_btw_purchases   |   months_after_latest_purchase |   risk_ratio |
    |-------------:|------------:|----------:|-------------:|:--------------------------|-------------------------------:|-------------:|
    |        12346 |       74215 |     74215 |          100 | Never Returned            |                             11 |          nan |
    |        12347 |        2458 |         0 |            0 | 2.0                       |                              2 |            1 |
    |        12348 |        2341 |         0 |            0 | 3.0                       |                              3 |            1 |
    |        12349 |         631 |         0 |            0 | New Customer              |                              1 |          nan |
    |        12350 |         197 |         0 |            0 | Never Returned            |                             10 |          nan |

    </div>


### 이탈자와 재구매자 비교
- 이탈자 vs 1년 이내에 2번 이상 구매한 고객

```python
# 'New Customer'는 제외하고 분석
purchase_refund1 = purchase_refund.query('avg_month_btw_purchases != "New Customer"')

# 고객을 'Never Returned'와 'Returned'로 구분해줌
purchase_refund1['flag1'] = purchase_refund1['avg_month_btw_purchases'].apply(lambda x: 'Never Returned' if x == 'Never Returned' else 'Returned')
purchase_refund1.head()
```

<div class="code-example" markdown="1">

|   CustomerID |   Purchases |   Refunds |   Re_per_Pur | avg_month_btw_purchases   |   months_after_latest_purchase |   risk_ratio | flag1          |
|-------------:|------------:|----------:|-------------:|:--------------------------|-------------------------------:|-------------:|:---------------|
|        12346 |       74215 |     74215 |     100      | Never Returned            |                             11 |   nan        | Never Returned |
|        12347 |        2458 |         0 |       0      | 2.0                       |                              2 |     1        | Returned       |
|        12348 |        2341 |         0 |       0      | 3.0                       |                              3 |     1        | Returned       |
|        12350 |         197 |         0 |       0      | Never Returned            |                             10 |   nan        | Never Returned |
|        12352 |         536 |        66 |      12.3134 | 3.0                       |                              1 |     0.333333 | Returned       |

</div>

1. 이탈자와 재구매자의 구매당 반품수 비교
    
    ```python
    purchase_refund1.groupby('flag1')[['Re_per_Pur']].agg(['mean', 'count'])
    ```

    <div class="code-example" markdown="1">

    |                | Re_per_Pur                                          ||   
    | flag1          |                   mean |                  count  |
    |:---------------|-------------------------:|--------------------------:|
    | Never Returned |                  2.6610  |                      1097 |
    | Returned       |                  1.7426  |                      2644 |

    </div>

    → 시각화:

    ```python
    temp = purchase_refund1.groupby('flag1')[['Re_per_Pur']].agg(['mean', 'count'])
    temp = temp['Re_per_Pur'].reset_index()

    fig = px.bar(temp, x='flag1', y='mean', color='flag1', 
                hover_data = ['count'],
                labels = {'mean':'Refunds per Purchase', 'flag1': 'Customer Flag', 'count':'Customer Count'},
                category_orders = {'flag1':['Never Returned', 'Returned']},
                color_discrete_sequence = [px.colors.sequential.Teal[0], px.colors.sequential.Teal[2]])

    fig.update_traces(showlegend=False)
    fig.show()
    ```
    <iframe width="500" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/140.embed"></iframe>
    - 이탈 고객이 재구매 고객 대비 구매당 반품 비율이 높은 편


1. t-test로 평균 차이가 유의미한지 검정

    ```python
    never_returned = purchase_refund1.query('flag1 == "Never Returned"')
    returned = purchase_refund1.query('flag1 == "Returned"')

    # Levene의 등분산 검정 
    lev_result = stats.levene(never_returned['Re_per_Pur'], returned['Re_per_Pur'])
    print('LeveneResult(F) : %.2f \np-value : %.3f' % (lev_result))
    ```
    ```
    LeveneResult(F) : 5.76 
    p-value : 0.016
    ```

    → p-value가 0.05 미만이므로, 이분산인 독립표본 t-test로 진행:

    ```python
    t_result = stats.ttest_ind(never_returned['Re_per_Pur'], returned['Re_per_Pur'], equal_var=False) 
    print('t statistic : %.2f \np-value : %.3f' % (t_result))
    ```
    ```
    t statistic : 1.80 
    p-value : 0.072
    ```
    - t-test 결과 p-value는 0.05 이상이지만, 수치 차이를 볼 때, 이탈한 고객과 재방문 고객 간에 구매당 반품 비율 차이가 약간은 의미가 있다고 생각해도 될 듯. (데이터 수가 더 많으면 p값이 더 낮게 나올 수 있음)


### 이탈 고위험군과 저위험군 비교

```python
# 'risk ratio'가 null인 행은 제외하고 분석 (Never Returned와 New Customer 제외)
purchase_refund2 = purchase_refund[purchase_refund['risk_ratio'].notna()]

# risk ratio가 1.5보다 높으면 'High Risk', 그 외에는 'Low Risk'로 분류해줌
# risk ratio가 1.5라는 건 평소 구매 주기의 1.5배의 기간 동안 돌아오지 않았다는 의미
purchase_refund2['flag2'] = purchase_refund2['risk_ratio'].apply(lambda x: 'High Risk' if x > 1.5 else 'Low Risk')
purchase_refund2.head()
```

<div class="code-example" markdown="1">

|   CustomerID |   Purchases |   Refunds |   Re_per_Pur |   avg_month_btw_purchases |   months_after_latest_purchase |   risk_ratio | flag2    |
|-------------:|------------:|----------:|-------------:|--------------------------:|-------------------------------:|-------------:|:---------|
|        12347 |        2458 |         0 |     0        |                         2 |                              2 |     1        | Low Risk |
|        12348 |        2341 |         0 |     0        |                         3 |                              3 |     1        | Low Risk |
|        12352 |         536 |        66 |    12.3134   |                         3 |                              1 |     0.333333 | Low Risk |
|        12356 |        1591 |         0 |     0        |                         5 |                              1 |     0.2      | Low Risk |
|        12359 |        1609 |        10 |     0.621504 |                         3 |                              2 |     0.666667 | Low Risk |

</div>

1. 이탈 고위험군과 저위험군의 구매당 반품수 비교

    ```python
    purchase_refund2.groupby('flag2')[['Re_per_Pur']].agg(['mean', 'count'])
    ```

    <div class="code-example" markdown="1">

    |              | Re_per_Pur                                          ||   
    | flag2        |                   mean |                  count  |
    |:-------------|-------------------------:|--------------------------:|
    | High Risk |                  2.45521 |                       515 |
    | Low Risk  |                  1.55297 |                      2088 |

    </div>

    → 시각화:

    ```python
    temp = purchase_refund2.groupby('flag2')[['Re_per_Pur']].agg(['mean', 'count'])
    temp = temp['Re_per_Pur'].reset_index()

    fig = px.bar(temp, x='flag2', y='mean', color='flag2', 
                hover_data = ['count'],
                labels = {'mean':'Refunds per Purchase', 'flag2': 'Customer Flag', 'count':'Customer Count'},
                color_discrete_sequence = [px.colors.sequential.Teal[0], px.colors.sequential.Teal[2]])

    fig.update_traces(showlegend=False)
    fig.show()
    ```
    <iframe width="500" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/142.embed"></iframe>
    - 이탈 고위험군인 고객들이 구매당 반품 비율이 더 높은 편으로 파악됨


1. t-test로 평균 차이가 유의미한지 검정
    
    ```python
    high_risk = purchase_refund2.query('flag2 == "High Risk"')
    low_risk = purchase_refund2.query('flag2 == "Low Risk"')

    # Levene의 등분산 검정 
    lev_result = stats.levene(high_risk['Re_per_Pur'], low_risk['Re_per_Pur'])
    print('LeveneResult(F) : %.2f \np-value : %.3f' % (lev_result))
    ```
    ```
    LeveneResult(F) : 7.13 
    p-value : 0.008
    ```

    → p-value가 0.05 미만이므로, 이분산인 독립표본 t-test로 진행:
    ```python
    t_result = stats.ttest_ind(high_risk['Re_per_Pur'], low_risk['Re_per_Pur'], equal_var=False) 
    print('t statistic : %.2f \np-value : %.3f' % (t_result))
    ```
    ```
    t statistic : 1.87 
    p-value : 0.062
    ```
    - p값이 0.05 이상이긴 하지만, 수치 차이를 볼 때, 이탈한 고객과 재방문 고객 간에 구매당 반품 비율 차이가 약간은 의미가 있다고 생각해도 될 듯. (데이터 수가 더 많으면 p값이 더 낮게 나올 수 있음)
    - 반품을 방지하는 것이 이탈 고위험군 고객의 만족도를 높여 이탈을 방지하는 데에 도움이 될 거라고 가설을 세울 수 있을 듯




## 상품별 분석

```python
# Description을 기준으로 정리하기 전에, 혹시 모를 사소한 차이를 대비해 모두 upper로 맞춰주고, 양 옆 공백을 없애줌
print(ecom_df['Description'].nunique()) # 정리 이전의 Description 수

ecom_df['Description'] = ecom_df['Description'].str.upper().str.strip()
print(ecom_df['Description'].nunique()) # 정리 이후의 Description 수
```
```
4037
4026
```

### 상품별 구매 대비 반품
- 반품을 최소화하기 위해, 구매 대비 반품이 많은 제품을 살펴보기로 함

```python
# 상품별로 구매수와 반품수를 집계한 다음 join
product_purchase = ecom_df.query('TotalSpending >= 0')
product_purchase = product_purchase.groupby('Description')[['Quantity']].sum()
product_purchase.rename(columns={'Quantity':'Purchases'}, inplace=True)

product_refund = ecom_df.query('TotalSpending < 0')
product_refund = product_refund.groupby('Description')[['Quantity']].sum() * -1
product_refund.rename(columns={'Quantity':'Refunds'}, inplace=True)

products = product_purchase.join(product_refund, how='left')
products['Refunds'].fillna(0, inplace=True)

# Refunds per Purchases 수치 계산
products['Re_per_Pur'] = products['Refunds'] / products['Purchases'] * 100
products.head()
```

<div class="code-example" markdown="1">

| Description                |   Purchases |   Refunds |   Re_per_Pur |
|:---------------------------|------------:|----------:|-------------:|
| *BOOMBOX IPOD CLASSIC      |           1 |         0 |     0        |
| *USB OFFICE MIRROR BALL    |           2 |         0 |     0        |
| 10 COLOUR SPACEBOY PEN     |        6564 |       172 |     2.62035  |
| 12 COLOURED PARTY BALLOONS |        2134 |        20 |     0.93721 |
| 12 DAISY PEGS IN WOOD BOX  |         344 |         0 |     0        |

</div>

→ 구매 대비 반품비율이 가장 높은 제품 Top 10을 시각화:

```python
# 구매가 너무 적을 경우 한두명의 변심에 따라 반품비율이 너무 높게 나타날 수 있으므로, 구매가 10번 이상인 제품만 대상으로 비교
temp = products.reset_index().query('Purchases >= 10')
temp = temp.sort_values(by='Re_per_Pur', ascending=False).head(10)

fig = px.bar(temp, x='Re_per_Pur', y='Description', color='Re_per_Pur',
             hover_data=['Purchases', 'Refunds'],
             labels = {'Re_per_Pur':'Refunds per Purchase'},
             category_orders = {'Description': temp['Description'].to_list()},
             color_continuous_scale = 'Teal')

fig.update(layout_coloraxis_showscale=False) 
fig.show()
```
<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/144.embed"></iframe>
- 반품 비율이 100% 이상인 제품들: 2010.12 이전 데이터는 기록되어 있지 않기 때문에, 그 이전에 구매한 후 기록 기간 내에 반품된 건이 많아서 그런 듯. 
- 구매당 반품 비율이 50% 이상인 제품들은 집중 품질 관리가 필요하다고 생각됨.


&nbsp;
{: .fs-1 .lh-0}

+) 반품 비율이 가장 높은 제품의 구매월 & 반품월 확인:

```python
# WOODEN BOX ADVENT CALENDAR
fig = px.bar(ecom_df.query('Description == "WOODEN BOX ADVENT CALENDAR"'), 
            x='InvoiceMonth', y='Quantity', color='Quantity',
            color_continuous_scale = 'Teal')

fig.update(layout_coloraxis_showscale=False) 
fig.show()
```
<iframe width="700" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/150.embed"></iframe>
- WOODEN BOX ADVENT CALENDAR 제품의 경우, 주로 2010.12~2011.01 기간에 반품이 몰려 있고, 그 이후에는 반품이 거의 없음. -- 2011년 초에 제품 품질 개선이 이미 이루어졌는지 확인해보면 좋을 듯


### 상품별 구매 증가폭

1. 상반기 대비 하반기 구매량이 가장 많이 증가한 제품

    ```python
    first_half = ecom_df.query('InvoiceDate < "2011-06-01"')
    temp1 = first_half.groupby('Description')[['Quantity']].sum()
    temp1.rename(columns={'Quantity':'first_half'}, inplace=True)

    second_half = ecom_no_na.query('InvoiceDate >= "2011-06-01" & InvoiceDate < "2011-12-01"')
    temp2 = second_half.groupby('Description')[['Quantity']].sum()
    temp2.rename(columns={'Quantity':'second_half'}, inplace=True)

    prod_purchase_change = temp1.join(temp2, how='outer')
    prod_purchase_change.fillna(0, inplace=True)
    prod_purchase_change['change'] = prod_purchase_change['second_half'] - prod_purchase_change['first_half']
    prod_purchase_change.sort_values(by='change', ascending=False).head(10)
    ```

    <div class="code-example" markdown="1">

    | Description                         |   first_half |   second_half |   change |
    |:------------------------------------|-------------:|--------------:|---------:|
    | POPCORN HOLDER                      |            0 |         25149 |    25149 |
    | RABBIT NIGHT LIGHT                  |         1130 |         22278 |    21148 |
    | MINI PAINT SET VINTAGE              |            0 |         14033 |    14033 |
    | RED  HARMONICA IN BOX               |            0 |         12787 |    12787 |
    | ROTATING SILVER ANGELS T-LIGHT HLDR |        -6887 |          5284 |    12171 |
    | PAPER CHAIN KIT 50'S CHRISTMAS      |            0 |         11993 |    11993 |
    | BROCADE RING PURSE                  |            0 |         11842 |    11842 |
    | PACK OF 12 LONDON TISSUES           |            0 |         11763 |    11763 |
    | BUBBLEGUM RING ASSORTED             |            0 |         11419 |    11419 |
    | 60 CAKE CASES VINTAGE CHRISTMAS     |         1336 |         12642 |    11306 |

    </div>
    - Popcorn Holder가 가장 구매량이 많이 증가함. 
    - 주로 상반기에는 없었던 신상품들이 구매 증가량이 가장 많은 것을 알 수 있음

    cf) 전체 기간 구매량 Top 10:

    ```python
    ecom_df.groupby('Description')[['Quantity']].sum().sort_values(by='Quantity', ascending=False).head(10)
    ```

    <div class="code-example" markdown="1">

    | Description                        |   Quantity |
    |:-----------------------------------|-----------:|
    | WORLD WAR 2 GLIDERS ASSTD DESIGNS  |      53751 |
    | JUMBO BAG RED RETROSPOT            |      47256 |
    | POPCORN HOLDER                     |      36322 |
    | ASSORTED COLOUR BIRD ORNAMENT      |      36282 |
    | PACK OF 72 RETROSPOT CAKE CASES    |      36016 |
    | WHITE HANGING HEART T-LIGHT HOLDER |      35294 |
    | RABBIT NIGHT LIGHT                 |      30631 |
    | MINI PAINT SET VINTAGE             |      26437 |
    | PACK OF 12 LONDON TISSUES          |      26095 |
    | PACK OF 60 PINK PAISLEY CAKE CASES |      24719 |

    </div>

1. 상반기 대비 하반기 구매 금액이 가장 많이 증가한 제품

    ```python
    first_half = ecom_df.query('InvoiceDate < "2011-06-01"')
    temp1 = first_half.groupby('Description')[['TotalSpending']].sum()
    temp1.rename(columns={'TotalSpending':'first_half'}, inplace=True)

    second_half = ecom_no_na.query('InvoiceDate >= "2011-06-01" & InvoiceDate < "2011-12-01"')
    temp2 = second_half.groupby('Description')[['TotalSpending']].sum()
    temp2.rename(columns={'TotalSpending':'second_half'}, inplace=True)

    prod_purchase_change2 = temp1.join(temp2, how='outer')
    prod_purchase_change2.fillna(0, inplace=True)
    prod_purchase_change2['change'] = prod_purchase_change2['second_half'] - prod_purchase_change2['first_half']

    # 'MANUAL'은 하나의 제품은 아니므로 제외
    prod_purchase_change2.query('Description != "MANUAL"').sort_values(by='change', ascending=False).head(10)
    ```

    <div class="code-example" markdown="1">

    | Description                         |   first_half |   second_half |   change |
    |:------------------------------------|-------------:|--------------:|---------:|
    | RABBIT NIGHT LIGHT                  |      2277.5 |       42033.0   |  39755.5 |
    | PICNIC BASKET WICKER 60 PIECES      |         0    |       39619.5 |  39619.5 |
    | PAPER CHAIN KIT 50'S CHRISTMAS      |         0    |       32806.9 |  32806.9 |
    | SET OF 3 REGENCY CAKE TINS          |         0    |       25219.8 |  25219.8 |
    | HOT WATER BOTTLE KEEP CALM          |         0    |       21902.0   |  21902.0 |
    | SET OF TEA COFFEE SUGAR TINS PANTRY |         0    |       21305.5 |  21305.5 |
    | DOORMAT KEEP CALM AND COME IN       |      6175.5 |       27088.7 |  20913.2 |
    | SPOTTY BUNTING                      |      7513.9 |       27418.4 |  19904.5 |
    | SET OF 3 CAKE TINS PANTRY DESIGN    |         0    |       19277.6 |  19277.6 |
    | POPCORN HOLDER                      |         0    |       19109.8 |  19109.8 |

    </div>
    - Rabbit Night Light가 구매 금액이 가장 많이 증가함. 
    - 주로 상반기에는 없었던 신상품들이 구매 금액 증가폭이 가장 크다

    cf) 전체 기간 구매 금액 Top 10:

    ```python
    # 'DOTCOM POSTAGE', 'POSTAGE'는 판매 제품이라고 보기는 애매하므로 제외
    products_spending = ecom_df.groupby('Description')[['TotalSpending']].sum().sort_values(by='TotalSpending', ascending=False)
    products_spending.query('Description not in ["DOTCOM POSTAGE", "POSTAGE"]').head(10)
    ```

    <div class="code-example" markdown="1">

    | Description                        |   TotalSpending |
    |:-----------------------------------|----------------:|
    | REGENCY CAKESTAND 3 TIER           |        164459.5 |
    | WHITE HANGING HEART T-LIGHT HOLDER |         99612.4 |
    | PARTY BUNTING                      |         98243.9 |
    | JUMBO BAG RED RETROSPOT            |         92175.8 |
    | RABBIT NIGHT LIGHT                 |         66661.6 |
    | PAPER CHAIN KIT 50'S CHRISTMAS     |         63715.2 |
    | ASSORTED COLOUR BIRD ORNAMENT      |         58792.4 |
    | CHILLI LIGHTS                      |         53746.7 |
    | SPOTTY BUNTING                     |         42030.7 |
    | JUMBO BAG PINK POLKADOT            |         41584.4 |

    </div>



## 국가별 분석

### 국가별 매출

1. 국가별 총 매출

    ```python
    # 전체 기간 매출 Top 10 국가 시각화
    country_spending = ecom_df.groupby('Country')[['TotalSpending']].sum().sort_values(by='TotalSpending', ascending=False)

    fig = px.bar(country_spending.head(10).reset_index(),
                x='Country', y='TotalSpending', color='TotalSpending',
                color_continuous_scale = 'Teal')

    fig.update(layout_coloraxis_showscale=False) 
    fig.show()
    ```
    <iframe width="700" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/155.embed"></iframe>
    - UK 기반 업체이므로, UK가 압도적으로 많고, 주로 인접한 유럽 국가들에서 구매액이 높음

1. 국가별 고객 1인당 매출

    ```python
    # 국가별로, 고객 1인당 매출 계산
    spc_country = ecom_df.groupby('Country').agg({'TotalSpending':'sum', 'CustomerID':pd.Series.nunique})
    spc_country['SpendingPerCustomer'] = spc_country['TotalSpending'] / spc_country['CustomerID']


    # 고객 1인당 매출 Top 10 국가 시각화
    # 고객 수가 5명 이상인 국가로 한정
    spc_country_over3 = spc_country.query('CustomerID >= 5').sort_values(by='SpendingPerCustomer', ascending=False)

    fig = px.bar(spc_country_over3.head(10).reset_index(),
                x='Country', y='SpendingPerCustomer', color='SpendingPerCustomer',
                hover_data = ['CustomerID'], labels = {'CustomerID': 'Customer Count'},
                color_continuous_scale = 'Teal')

    fig.update(layout_coloraxis_showscale=False)
    fig.show()
    ```
    <iframe width="700" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/157.embed"></iframe>
    - Netherlands가 가장 고객 당 매출이 높은 것으로 확인됨.
    - UK의 경우, 고객수가 압도적으로 많아서 고객 당 매출액이 보다 평균으로 회귀한 결과가 나온 것일 수도 있고, 배송비가 적게 붙어서 그런 영향도 조금 있을 수 있을 듯.       


### 상반기 대비 하반기 비교

1. 국가별 상반기 대비 하반기 구매금액 

    ```python
    first_half = ecom_no_na.query('InvoiceDate < "2011-06-01"')
    temp1 = first_half.groupby('Country')[['TotalSpending']].sum()
    temp1.rename(columns={'TotalSpending':'first_half'}, inplace=True)

    second_half = ecom_no_na.query('InvoiceDate >= "2011-06-01" & InvoiceDate < "2011-12-01"')
    temp2 = second_half.groupby('Country')[['TotalSpending']].sum()
    temp2.rename(columns={'TotalSpending':'second_half'}, inplace=True)

    spending_change = temp1.join(temp2, how='outer')
    spending_change.fillna(0, inplace=True)
    spending_change['change'] = spending_change['second_half'] - spending_change['first_half']
    spending_change.sort_values(by='change', ascending=False).head(10)
    ```

    <div class="code-example" markdown="1">

    | Country        |       first_half |      second_half |          change |
    |:---------------|-----------------:|-----------------:|----------------:|
    | United Kingdom |   2535957.8      | 3920927.9	       |  1384970.1      |
    | EIRE           |  78764.5         | 164260.9           | 85496.3         |
    | Netherlands    | 112906.7         | 160026.8           | 47120.2         |
    | France         |  71743.5         | 117833.7           | 46090.2         |
    | Germany        |  91606.4         | 122098.4           | 30492.1         |
    | Australia      |  55600.0           |  81409.8         | 25809.8         |
    | Switzerland    |  15438.4         |  40301.0           | 24862.5         |
    | Norway         |   4722.7         |  27655.1         | 22932.4         |
    | Belgium        |  13140.0           |  26361.5         | 13221.5         |
    | Spain          |  21485.9         |  32998.7         | 11512.9         |

    </div>

    → 구매액 증가폭이 높은 국가 Top 10 시각화:

    ```python
    country_spending_change = spending_change.sort_values(by='change', ascending=False).head(10)

    fig = px.bar(country_spending_change[['first_half', 'second_half']].unstack().reset_index(),
                x='Country', y=0, color='level_0', barmode='group', 
                labels={'0':'Total Spending', 'level_0':'Time Period'},
                category_orders = {'Country': country_spending_change.index}, # 구매액 증가폭이 높은 순서대로 정렬
                color_discrete_sequence = [px.colors.sequential.Teal[0], px.colors.sequential.Teal[2]])

    fig.update(layout_coloraxis_showscale=False)
    fig.show()
    ```
    <iframe width="700" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/161.embed"></iframe>
    - 구매액 증가폭 역시 UK가 가장 크고, 그 다음이 아일랜드와 네덜란드. 


1. 국가별 상반기 대비 하반기 고객수

    ```python
    temp1 = first_half.groupby('Country')[['CustomerID']].nunique()
    temp1.rename(columns={'CustomerID':'first_half'}, inplace=True)

    temp2 = second_half.groupby('Country')[['CustomerID']].nunique()
    temp2.rename(columns={'CustomerID':'second_half'}, inplace=True)

    customer_change = temp1.join(temp2, how='outer')
    customer_change.fillna(0, inplace=True)
    customer_change['change'] = customer_change['second_half'] - customer_change['first_half']
    customer_change.sort_values(by='change', ascending=False).head(10)
    ```

    <div class="code-example" markdown="1">

    | Country        |   first_half |   second_half |   change |
    |:---------------|-------------:|--------------:|---------:|
    | United Kingdom |         2508 |          3165 |      657 |
    | Germany        |           57 |            80 |       23 |
    | France         |           56 |            70 |       14 |
    | Spain          |           19 |            25 |        6 |
    | Norway         |            3 |             9 |        6 |
    | Switzerland    |           11 |            17 |        6 |
    | Finland        |            4 |            10 |        6 |
    | Denmark        |            3 |             8 |        5 |
    | Portugal       |           11 |            14 |        3 |
    | Poland         |            3 |             6 |        3 |

    </div>

    → 구매 고객수 증가폭이 높은 국가 Top 10 시각화:

    ```python
    country_customer_change = customer_change.sort_values(by='change', ascending=False).head(10)

    fig = px.bar(country_customer_change[['first_half', 'second_half']].unstack().reset_index(),
                x='Country', y=0, color='level_0', barmode='group', 
                labels={'0':'Customer Count', 'level_0':'Time Period'},
                category_orders = {'Country': country_customer_change.index}, # 고객수 증가폭이 높은 순서대로 정렬
                color_discrete_sequence = [px.colors.sequential.Teal[0], px.colors.sequential.Teal[2]])


    fig.update(layout_coloraxis_showscale=False)
    fig.show()
    ```
    <iframe width="700" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/164.embed"></iframe>
    - 구매고객 증가폭 역시 UK가 가장 크고, 그 다음은 독일과 프랑스. 

