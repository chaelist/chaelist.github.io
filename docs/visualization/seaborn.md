---
layout: default
title: Seaborn
parent: 데이터 시각화
nav_order: 2
---

# Seaborn
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


*Seaborn: Python data visualization library based on matplotlib
{: .text-purple-200}

## KDE Plot
: 대체로 세상에서 일어나는 대부분의 일들은 비슷한 확률밀도함수(PDF)의 생김새를 갖는다.  
그런데, 우리는 무한개의 데이터를 구할 수 없기에,  
우리가 구한 데이터로 그래프를 그려보면 매끄러운 확률밀도함수의 모양이 나오지 않는다.  
→ 이를 해결하는 것이 KDE(Kernel Density Estimation).  
KDE를 사용하면 우리가 구한 데이터를 기반으로 어느 정도 추측을 해서, 실제 분포에 가깝게 매끄러운 그래프를 그려준다.

<br/>
**+) PDF: Probability Density Function (확률밀도함수)**
- PDF 아래의 모든 면적을 더하면 1이다. (100%)
- PDF를 활용해서 값들이 어떻게 분포되어 있는지 나타낼 수 있다
- 특정 구간의 면적이 곧 값이 그 구간에 속할 확률이다
- 특정 '점'의 확률을 무조건 0이다. (면적이 0이므로)


### 기본 KDE Plot
```python
import pandas as pd
import seaborn as sns   # import해야 사용 가능; 보통 sns로 줄여서 import

body_df = pd.read_csv('data/body.csv')   ## 데이터 출처: codeit
body_df.head()
```

<div class="code-example" markdown="1">

|    |   Number |   Height |   Weight |
|---:|---------:|---------:|---------:|
|  0 |        1 |    176   |     85.2 |
|  1 |        2 |    175.3 |     67.7 |
|  2 |        3 |    168.6 |     75.2 |
|  3 |        4 |    168.1 |     67.1 |
|  4 |        5 |    175.3 |     63   |

</div>

→ 키 데이터를 순서대로 정렬
- value_counts()로 각 항목의 개수 파악
- sort_index()로 순서대로 정렬

```python
body_df['Height'].value_counts().sort_index()
```
```
154.4    1
155.5    1
157.4    1
157.8    1
158.0    1
        ..
190.3    1
191.2    1
191.8    1
192.4    1
193.1    1
Name: Height, Length: 262, dtype: int64
```

1. 'Height' 데이터 분포를 그대로 그래프로 그려보기
```python
body_df['Height'].value_counts().sort_index().plot() 
## 이렇게 우리가 가진 데이터로만 그래프를 그리면 보기 불편하게 나온다
```
![KDE_Plot_raw](../../../assets/images/seaborn/kdeplot_raw.png)

2. Seaborn의 KED Plot 기능으로 매끄러운 확률밀도함수 그리기
```python
sns.kdeplot(body_df['Height'])
```
![KDE_Plot1](../../../assets/images/seaborn/kdeplot1.png)

3. bandwidth 조절하기
```python
sns.kdeplot(body_df['Height'], bw=0.5);   # bw는 bandwidth의 약자
```
![KDE_Plot2](../../../assets/images/seaborn/kdeplot2.png)
- bw로 얼마나 매끄럽게 확률밀도함수를 조절할 것인지 선택. bw값이 클수록 매끄럽다.
- 하지만 bw가 무조건 클수록 좋은 것만은 아니다. 적절히 인사이트를 얻을 수 있는 만큼으로 조절.
- **bw를 설정하지 않으면, Seaborn이 적당한 값으로 알아서 골라준다.

### distplot()
: Seaborn의 distplot을 활용하면 히스토그램과 KDE Plot을 함께 표현 가능
```python
sns.distplot(body_df['Height'], bins=15); 
## df.plot(kind='hist', y='Height', bins=15)와 sns.kdeplot(body_df['Height'])를 합친 결과가 나옴
```
![distplot](../../../assets/images/seaborn/distplot1.png)


### violinplot()
: KDE Plot을 양 옆으로 대칭으로 그려둔 모양이랑 동일. box plot과 유사하게, 값의 분포를 파악 가능.
```python
sns.violinplot(y=body_df['Height'])
```
![violinplot](../../../assets/images/seaborn/violinplot1.png)
 

### KDE Plot; 등고선 그래프
:KDE Plot 2개를 합쳐서 등고선 형태로 나타낸 그래프.
```python
sns.kdeplot(body_df['Height'], body_df['Weight'])  
```
![Double_KDE_Plot](../../../assets/images/seaborn/double_kdeplot1.png)
- body_df 자료에서의 Height와 Weight의 연관성과 함께, 각각의 분포 모양도 볼 수 있는 그래프.
- Height의 KDE Plot과 Weight의 KEP Plot을 함쳐서, 3D 형태의 산으로 나타낸 것을 위에서 바라본 모양. → 등고선처럼 표현됨
- 아래에 각각 그려둔 KDE Plot을 보고 머릿속으로 합쳐보면, 위와 같은 등고선이 이해가 될 것이다

```python
sns.kdeplot(body_df['Height'])
```
![KDE_Plot3](../../../assets/images/seaborn/kdeplot3.png)

```python
sns.kdeplot(body_df['Weight'])
```
![KDE_Plot4](../../../assets/images/seaborn/kdeplot4.png)


## LM Plot
: LM은 Linear Model의 약자. 산점도와 Regression Line(회귀선)을 함께 그려준다.
```python
body_df = pd.read_csv('data/body.csv')   ## 데이터 출처: codeit
body_df.head()
```

<div class="code-example" markdown="1">

|    |   Number |   Height |   Weight |
|---:|---------:|---------:|---------:|
|  0 |        1 |    176   |     85.2 |
|  1 |        2 |    175.3 |     67.7 |
|  2 |        3 |    168.6 |     75.2 |
|  3 |        4 |    168.1 |     67.1 |
|  4 |        5 |    175.3 |     63   |

</div>

1. 기본 lmplot 그리기
```python
sns.set_style('darkgrid')  ## 이렇게 grid의 style도 지정 가능
sns.lmplot(data=body_df, x='Height', y='Weight')
```
![LMPlot1](../../../assets/images/seaborn/lmplot1.png)

1. ci (Confidence Interval, 신뢰구간) 조절
- lmplot의 default setting은 ci=95 (95% 신뢰구간 표시) → `ci=None`으로 꺼버리거나, `ci=80` 이런 식으로 변경할 수 있다

    ```python
    sns.set_style('ticks')  ## style: dict, None, or one of {darkgrid, whitegrid, dark, white, ticks}
    sns.lmplot(data=body_df, x='Height', y='Weight', ci=None)
    ```
    ![LMPlot2](../../../assets/images/seaborn/lmplot2.png)

1. hue 옵션 지정

    ```python
    exam_df = pd.read_csv('data/exam.csv')   ## 데이터 출처: codeit
    exam_df.head()
    ```

    <div class="code-example" markdown="1">

    |    | gender   | race/ethnicity   | parental level of education   | lunch        | test preparation course   |   math score |   reading score |   writing score |
    |---:|:---------|:-----------------|:------------------------------|:-------------|:--------------------------|-------------:|----------------:|----------------:|
    |  0 | female   | group B          | bachelor's degree             | standard     | none                      |           72 |              72 |              74 |
    |  1 | female   | group C          | some college                  | standard     | completed                 |           69 |              90 |              88 |
    |  2 | female   | group B          | master's degree               | standard     | none                      |           90 |              95 |              93 |
    |  3 | male     | group A          | associate's degree            | free/reduced | none                      |           47 |              57 |              44 |
    |  4 | male     | group C          | some college                  | standard     | none                      |           76 |              78 |              75 |

    </div>

    → hue 옵션을 사용해 'gender' 별로 데이터 비교해보기
    ```python
    sns.lmplot(data=exam_df, x='math score', y='writing score', hue='gender', palette='Set2', height=4);  
            ## palette(그래프가 그려지는 컬러 팔레트를 결정), height(그래프 크기를 결정)도 지정 가능.
    ```
    ![LMPlot3](../../../assets/images/seaborn/lmplot3.png)


cf) scatterplot은 그냥 점만 흩뿌려주는 반면, lmplot은 데이터로부터 linear model을 유추해 선을 함께 그려줌 

```python
sns.scatterplot(data=exam_df, x='math score', y='writing score', hue='gender')
```
![ScatterPlot1](../../../assets/images/seaborn/scatterplot1.png)

## Joint Plot
- 2차원 실수형 데이터는 jointplot을 활용하면 다각도로 살피기 용이하다
- jointplot을 사용하면 scatterplot + 각 변수의 히스토그램을 함께 그려준다
- kind='kde'이면 커널 밀도 히스토그램을 그린다 (등고선 모양 그래프 + 각 변수의 KDE Plot)

1. 기본 jointplot: scatterplot + 변수별 히스토그램
```python
sns.jointplot(data=body_df, x='Height', y='Weight')
```
![JointPlot1](../../../assets/images/seaborn/jointplot1.png)

1. KDE jointplot: 등고선 그래프 + 변수별 KDE Plot
```python
sns.jointplot(data=body_df, x='Height', y='Weight', kind='kde')
```
![JointPlot2](../../../assets/images/seaborn/jointplot2.png)


## Box Plot

