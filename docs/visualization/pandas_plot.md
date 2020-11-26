---
layout: default
title: Pandas plot() 함수
parent: 데이터 시각화
nav_order: 1
---

# Pandas plot() 함수
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


*Pandas 내장 기능인 .plot() 함수를 사용하면 쉽게 그래프를 그릴 수 있다.  
(당연히 element가 '숫자형'일때만 그래프 그려짐)

## 선그래프

```python
import pandas as pd
```
```python
%matplotlib inline  # 그래프의 결과를 출력 세션에 나타나게 하는 설정
```

```python
df = pd.read_csv('data/broadcast.csv', index_col=0)   ## 데이터 출처: codeit
df
```

<div class="code-example" markdown="1">

|      |    KBS |    MBC |    SBS |   TV CHOSUN |   JTBC |   Channel A |   MBN |
|-----:|-------:|-------:|-------:|------------:|-------:|------------:|------:|
| 2011 | 35.951 | 18.374 | 11.173 |       9.102 |  7.38  |       3.771 | 2.809 |
| 2012 | 36.163 | 16.022 | 11.408 |       8.785 |  7.878 |       5.874 | 3.31  |
| 2013 | 31.989 | 16.778 |  9.673 |       9.026 |  7.81  |       5.35  | 3.825 |
| 2014 | 31.21  | 15.663 |  9.108 |       9.44  |  7.49  |       5.776 | 4.572 |
| 2015 | 27.777 | 16.573 |  9.099 |       9.94  |  7.267 |       6.678 | 5.52  |
| 2016 | 27.583 | 14.982 |  8.669 |       9.829 |  7.727 |       6.624 | 5.477 |
| 2017 | 26.89  | 12.465 |  8.661 |       8.886 |  9.453 |       6.056 | 5.215 |

</div>

1. 기본 그래프 그려보기
```python
df.plot()   # df.plot(kind='line') 이렇게 써도 동일. 선(line) 그래프가 default이기 때문.
```
![Linegraph1](../../../assets/images/pandas_plot/linegraph1.png)

1. 1개 값에 대해서만 그래프 그리기
```python
df.plot(y='KBS')  # KBS에 대해서만 그래프를 그림
     # df['KBS'].plot() 이렇게 써도 거의 동일.
```
![Linegraph2](../../../assets/images/pandas_plot/linegraph2.png)

1. 2개 값에 대해 그래프 그리기
```python
df.plot(y=['KBS', 'MBC']);  # df[['KBS', 'MBC']].plot() 이렇게 써도 동일
```
![Linegraph3](../../../assets/images/pandas_plot/linegraph3.png)


## 막대그래프
