---
layout: default
title: Plotly
parent: 데이터 시각화
nav_order: 4
---

# Plotly
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


[***Plotly**](https://plotly.com/python/){: target="_blank"}  
\: 세련된 opensource interactive graphing library. 

[***Plotly Express**](https://medium.com/plotly/introducing-plotly-express-808df010143d){: target="_blank"}  
\: easy-to-use, high-level visualization library    
(Plotly와 Plotly Express의 관계는 Matplotlib과 Seaborn의 관계와 유사)
- `pip install plotly`로 설치해서 사용

[***Chart Studio**](https://chart-studio.plotly.com/feed/#/){: target="_blank"}  
\: plotly로 시각화한 차트를 chart studio에 upload하면 쉽게 web 상에 embed할 수 있다  
- `pip install chart_studio`로 설치
- username과 api_key를 활용해 연결: 
    ```python
    import chart_studio

    username = ''  # 자신의 username (plotly account를 만들어야 함)
    api_key = ''    # 자신의 api key (settings > regenerate key)

    chart_studio.tools.set_credentials_file(username=username, api_key=api_key)
    ```
- 작성한 차트를 upload하는 코드:
    ```python
    chart_studio.plotly.plot(fig, filename = '파일이름', auto_open=True)  # fig: 작성한 차트를 저장한 변수
    ## 위 코드를 실행하면 새로운 window로 해당 차트의 링크가 열리고, notebook에도 link를 아래에 return해줌
    ```


## Bar graph
- [https://plotly.com/python/bar-charts/](https://plotly.com/python/bar-charts/){: target="_blank"}  


```python
# 필요한 library를 import
import plotly.express as px
import plotly.io as pio

import pandas as pd
import numpy as np

pio.templates.default = "plotly_white"  # default template을 지정 (지정하지 않으면 기본은 'plotly')
```

```python
bills_df = px.data.tips()  # plotly express에서 제공되는 기본 data 사용
bills_df.head()
```

<div class="code-example" markdown="1">

|    |   total_bill |   tip | sex    | smoker   | day   | time   |   size |
|---:|-------------:|------:|:-------|:---------|:------|:-------|-------:|
|  0 |        16.99 |  1.01 | Female | No       | Sun   | Dinner |      2 |
|  1 |        10.34 |  1.66 | Male   | No       | Sun   | Dinner |      3 |
|  2 |        21.01 |  3.5  | Male   | No       | Sun   | Dinner |      3 |
|  3 |        23.68 |  3.31 | Male   | No       | Sun   | Dinner |      2 |
|  4 |        24.59 |  3.61 | Female | No       | Sun   | Dinner |      4 |

</div>

1. daily average bills per sex

    ```python
    fig = px.bar(bills_df.groupby(['day', 'sex'])[['total_bill']].mean().reset_index(),
                x='day', y='total_bill', color='sex',
                title='Average Bills per Day', category_orders={'day':['Thur', 'Fri', 'Sat', 'Sun']},
                color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="600" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/1.embed"></iframe>
    - 연속적이지 않은 color palette는 `color_discrete_sequence` 옵션으로 지정
    - color로 구분을 넣으면 default로 누적 그래프 형태로 시각화됨

1. daily average bills per sex (2)

    ```python
    fig = px.bar(bills_df.groupby(['day', 'sex'])[['total_bill']].mean().reset_index(),
                x='day', y='total_bill', color='sex', height=400, 
                title='Average Bills per Day', category_orders={'day':['Thur', 'Fri', 'Sat', 'Sun']},
                color_discrete_sequence=px.colors.qualitative.Pastel,
                barmode='group')  # 양 옆으로 놓이는 구조  # barmode='stack'라고 하면 누적 (dafault setting)
    fig.show()
    ```
    <iframe width="600" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/3.embed"></iframe>
    - `barmode='group'` 옵션을 추가해주면 그룹별 값이 위아래 대신 양옆으로 배치됨
    - `height` 옵션으로 크기를 지정해주는 것도 가능 (+. `width` 옵션도 가능)

1. daily total bills per sex

    ```python
    fig = px.bar(bills_df, x='day', y='total_bill', color='sex',
                title='Total Bills per Day', category_orders={'day':['Thur', 'Fri', 'Sat', 'Sun']},
                color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="600" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/5.embed"></iframe>
    - seaborn에서는 전체 data를 넣어서 bar graph를 그리면 default로 평균값을 시각화해주는 반면, plotly express는 전체 data의 그대로 합산해서 시각화해준다

1. category별로 영역을 나누어 그리기

    ```python
    fig = px.bar(bills_df, x='day', y='total_bill', color='smoker', barmode='group', facet_col='sex',
                category_orders={'day':['Thur', 'Fri', 'Sat', 'Sun']},
                color_discrete_sequence=px.colors.qualitative.Safe, template='plotly')
    fig.show()
    ```
    <iframe width="700" height="450" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/7.embed"></iframe>
    - `facet_col` 옵션을 통해 특정 그룹별로 영역을 나눠서 시각화할 수 있다
    - `template` 옵션을 통해 각 그래프별 template을 별도로 지정할 수도 있다   
    (원래 default는 'plotly'지만, 위에서 default template을 'plotly_white'로 지정해두었으므로, 해당 그래프에서는 'plotly' template을 사용하기 위해 옵션을 별도로 지정)

## Scatter Plot
- [https://plotly.com/python/line-and-scatter/](https://plotly.com/python/line-and-scatter/){: target="_blank"} 

```python
iris_df = px.data.iris()
iris_df.head()
```

<div class="code-example" markdown="1">

|    |   sepal_length |   sepal_width |   petal_length |   petal_width | species   |   species_id |
|---:|---------------:|--------------:|---------------:|--------------:|:----------|-------------:|
|  0 |            5.1 |           3.5 |            1.4 |           0.2 | setosa    |            1 |
|  1 |            4.9 |           3   |            1.4 |           0.2 | setosa    |            1 |
|  2 |            4.7 |           3.2 |            1.3 |           0.2 | setosa    |            1 |
|  3 |            4.6 |           3.1 |            1.5 |           0.2 | setosa    |            1 |
|  4 |            5   |           3.6 |            1.4 |           0.2 | setosa    |            1 |

</div>

1. 기본 scatter plot

    ```python
    fig = px.scatter(iris_df, x='sepal_width', y='sepal_length', color='species',
                    color_discrete_sequence=px.colors.qualitative.Safe)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/9.embed"></iframe>

1. trendline을 포함한 scatter plot

    ```python
    fig = px.scatter(bills_df, x='total_bill', y='tip', trendline='ols',
                    color_discrete_sequence=px.colors.qualitative.Pastel1,
                    trendline_color_override='gold')
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/11.embed"></iframe>
    - `trendline='ols'` 옵션을 통해 선형회귀선을 함께 시각화

### Dot Plot

```python
gapminder_df = px.data.gapminder()
gapminder_df.head()
```

<div class="code-example" markdown="1">

|    | country     | continent   |   year |   lifeExp |      pop |   gdpPercap | iso_alpha   |   iso_num |
|---:|:------------|:------------|-------:|----------:|---------:|------------:|:------------|----------:|
|  0 | Afghanistan | Asia        |   1952 |    28.801 |  8425333 |     779.445 | AFG         |         4 |
|  1 | Afghanistan | Asia        |   1957 |    30.332 |  9240934 |     820.853 | AFG         |         4 |
|  2 | Afghanistan | Asia        |   1962 |    31.997 | 10267083 |     853.101 | AFG         |         4 |
|  3 | Afghanistan | Asia        |   1967 |    34.02  | 11537966 |     836.197 | AFG         |         4 |
|  4 | Afghanistan | Asia        |   1972 |    36.088 | 13079460 |     739.981 | AFG         |         4 |

</div>

→ scatter plot에 categorical axis를 넣어서 시각화 (dot plot)

```python
fig = px.scatter(gapminder_df.query("continent=='Americas'"), x='lifeExp', y='country',
                color='year',
                color_continuous_scale='Burgyl')
fig.show()
```
<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/13.embed"></iframe>
- 연속적인 color palette는 `color_continuous_scale` 옵션으로 지정


### Bubble Chart
: scatter plot에서 size option을 지정해 시각화하면 된다

1. 2007년의 gdpPercap과 liefExp의 관계를 시각화 (bubble size: population)

    ```python
    fig = px.scatter(gapminder_df.query("year == 2007"), 
                    x='gdpPercap', y='lifeExp', size='pop', color='continent',
                    hover_name='country')
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/15.embed"></iframe>
    - `hover_name` 옵션을 통해 마우스를 hover할 때 우선적으로 보여지는 정보를 지정

1. x값을 log 변환해 시각화

    ```python
    fig = px.scatter(gapminder_df.query("year == 2007"), 
                    x='gdpPercap', y='lifeExp', size='pop', color='continent',
                    hover_name='country', log_x=True, size_max=60)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/17.embed"></iframe>
    - x값과 y값의 더 명확한 관계를 시각화하기 위해 `log_x=True` 옵션을 지정
    - 더 bubble의 size를 구분하기 쉽도록 `size_max=60` 옵션으로 bubble size를 조정


## Line Chart
- [https://plotly.com/python/line-charts/](https://plotly.com/python/line-charts/){: target="_blank"}

```python
fig = px.line(gapminder_df.query("continent == 'Oceania'"), 
              x='year', y='lifeExp', color='country', symbol='country',
              color_discrete_sequence=px.colors.qualitative.Pastel1)
fig.show()
```
<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/19.embed"></iframe>

- `symbol='country'` 옵션을 통해 각 국가별 marker의 모양을 다르게 지정
- 각 그룹별 marker 모양을 구분하지 않고 모두 동일한 marker로 표시하고 싶으면 `symbol` 옵션 대신 `markers=True` 옵션을 추가해주면 된다

## Pie Graph
- [https://plotly.com/python/pie-charts/](https://plotly.com/python/pie-charts/){: target="_blank"}

```python
# 2007년 gapminder 수치 중, Asia 지역의 국가별 population 비중을 시각화

temp_df = gapminder_df.query("year == 2007").query("continent == 'Asia'")
# population이 가장 많은 top 15 국가를 제외하고는 다 'Other countries'로 처리
temp_df.sort_values(by='pop', ascending=False, inplace=True)
temp_df.iloc[15:, 0] = 'Other countries'

fig = px.pie(temp_df, values='pop', names='country',
             color_discrete_sequence=px.colors.qualitative.Antique)
fig.show()
```
<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/21.embed"></iframe>


### Treemap
- [https://plotly.com/python/treemaps/](https://plotly.com/python/treemaps/){: target="_blank"}

```python
temp_df = gapminder_df.query("year == 2007")
fig = px.treemap(temp_df, path=[px.Constant('world'), 'continent', 'country'], 
                values='pop', color='lifeExp', color_continuous_scale='RdBu',
                color_continuous_midpoint=np.average(temp_df['lifeExp'], weights=temp_df['pop']))

fig.update_layout(margin = dict(t=50, l=25, r=25, b=25))
fig.show()
```
<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/23.embed"></iframe>



### Sunburst Chart
- [https://plotly.com/python/sunburst-charts/](https://plotly.com/python/sunburst-charts/){: target="_blank"}


```python
fig = px.sunburst(bills_df, path=['day', 'time', 'sex'], values='tip',
                 color='time', color_discrete_sequence=px.colors.qualitative.Pastel)
fig.show()
```
<iframe width="500" height="450" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/25.embed"></iframe>


## Statistical Charts

### Box Plot
- [https://plotly.com/python/box-plots/](https://plotly.com/python/box-plots/){: target="_blank"}


1. 기본 box plot

    ```python
    fig = px.box(bills_df, x='sex', y='total_bill', color='smoker', 
                color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/29.embed"></iframe>

1. 각 point를 함께 시각화

    ```python
    fig = px.box(bills_df, x='sex', y='total_bill', color='smoker', points='all',
                color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/32.embed"></iframe>
    - `points='all'` 옵션을 지정하면 box plot 옆에 모든 point를 함께 시각화해줌 (default: `points='outliers'`)


### Strip Plot
- [https://plotly.com/python/strip-charts/](https://plotly.com/python/strip-charts/){: target="_blank"}

1. 기본 strip plot

    ```python
    fig = px.strip(bills_df, x='sex', y='total_bill', color='smoker', 
                color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/34.embed"></iframe>

1. category별로 세분화해 시각화

    ```python
    fig = px.strip(bills_df, x='total_bill', y='time', color='sex', facet_col='day', 
                category_orders={'day':['Thur', 'Fri', 'Sat', 'Sun']},
                color_discrete_sequence=px.colors.qualitative.Safe, template='plotly')
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/36.embed"></iframe>


### Violin Plot
- [https://plotly.com/python/violin/](https://plotly.com/python/violin/){: target="_blank"}

1. 기본 violin plot

    ```python
    fig = px.violin(bills_df, x='sex', y='total_bill', color='smoker',
                    color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/38.embed"></iframe>

1. box, points 옵션 지정

    ```python
    fig = px.violin(bills_df, y='total_bill', color='sex', box=True, points='all',
                    color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/40.embed"></iframe>
    - `points='all'` 옵션을 지정하면 violin plot 옆에 모든 point를 함께 시각화해줌
    - `box=True` 옵션을 지정하면 violin plot 안에 box plot를 함께 시각화해줌

### Histogram
- [https://plotly.com/python/histograms/](https://plotly.com/python/histograms/){: target="_blank"}

1. 기본 histogram

    ```python
    fig = px.histogram(bills_df, x='total_bill', nbins=10)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/42.embed"></iframe>
    - `nbins` 옵션으로 number of bins를 조절

1. categorical data를 넣으면 count plot처럼 작용

    ```python
    fig = px.histogram(bills_df, x='day', category_orders={'day':['Thur', 'Fri', 'Sat', 'Sun']})
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/44.embed"></iframe>
    - x값에 categorical data를 넣으면 각 카테고리별 수를 세어서 시각화해줌 (count plot)

1. histogram과 각 값의 분포를 함께 시각화

    ```python
    fig = px.histogram(bills_df, x='total_bill', y='tip', color='sex', marginal='box',
                    color_discrete_sequence=px.colors.qualitative.Pastel)
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/46.embed"></iframe>
    - `marginal='box'` 옵션을 추가하면 각 값의 분포를 box plot으로 함께 시각화해줌   
    (marginal: box, violin, rug 중에 선택 가능)
    - 참고: [Combined statistical representations](https://plotly.com/python/distplot/){: target="_blank"}




<br/>


+) 참고용 링크들
- [color_discrete_sequence](https://plotly.com/python/discrete-color/){: target="_blank"}
- [color_continuous_scale](https://plotly.com/python/builtin-colorscales/#builtin-sequential-color-scales){: target="_blank"}
- [adjusting size](https://plotly.com/python/setting-graph-size/#adjusting-height-width--margins-with-plotly-express){: target="_blank"}
- [templates](https://plotly.com/python/templates/){: target="_blank"}
- [image export](https://plotly.com/python/static-image-export/){: target="_blank"}   /   [html export](https://plotly.com/python/interactive-html-export/){: target="_blank"}
- [plotly.express python api reference](https://plotly.com/python-api-reference/plotly.express.html#px){: target="_blank"}