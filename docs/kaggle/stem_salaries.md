---
layout: default
title: STEM Salaries 1
parent: Kaggle Dataset EDA
nav_order: 6
---

# STEM Salaries 1
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


***분석 대상 데이터셋: Data Science and STEM Salaries**
- [데이터셋 출처](https://www.kaggle.com/jackogozaly/data-science-and-stem-salaries?select=Levels_Fyi_Salary_Data.csv){: target="_blank"}
- levels.fyi에서 가져온 62,642개의 salary data (Data Science & STEM 직종)
- 2019.01.01 ~ 2020.09.09의 기간에 기록된 약 1060개의 회사, 15개 job title에 대한 데이터

## 데이터 파악 및 전처리

```python
# 필요한 라이브러리 import
import pandas as pd
import numpy as np

import plotly.express as px
import plotly.io as pio

pio.templates.default = "plotly_white"  # default template을 지정
```

```python
salary_df = pd.read_csv('data/Levels_Fyi_Salary_Data.csv')
salary_df.head(3)
```

<div class="code-example" markdown="1">

|    | timestamp          | company   | level   | title             |   totalyearlycompensation | location          |   yearsofexperience |   yearsatcompany |   tag |   basesalary |   stockgrantvalue |   bonus |   gender |   otherdetails |   cityid |   dmaid |   rowNumber |   Masters_Degree |   Bachelors_Degree |   Doctorate_Degree |   Highschool |   Some_College |   Race_Asian |   Race_White |   Race_Two_Or_More |   Race_Black |   Race_Hispanic |   Race |   Education |
|---:|:-------------------|:----------|:--------|:------------------|--------------------------:|:------------------|--------------------:|-----------------:|------:|-------------:|------------------:|--------:|---------:|---------------:|---------:|--------:|------------:|-----------------:|-------------------:|-------------------:|-------------:|---------------:|-------------:|-------------:|-------------------:|-------------:|----------------:|-------:|------------:|
|  0 | 6/7/2017 11:33:27  | Oracle    | L3      | Product Manager   |                    127000 | Redwood City, CA  |                 1.5 |              1.5 |   nan |       107000 |             20000 |   10000 |      nan |            nan |     7392 |     807 |           1 |                0 |                  0 |                  0 |            0 |              0 |            0 |            0 |                  0 |            0 |               0 |    nan |         nan |
|  1 | 6/10/2017 17:11:29 | eBay      | SE 2    | Software Engineer |                    100000 | San Francisco, CA |                 5   |              3   |   nan |            0 |                 0 |       0 |      nan |            nan |     7419 |     807 |           2 |                0 |                  0 |                  0 |            0 |              0 |            0 |            0 |                  0 |            0 |               0 |    nan |         nan |
|  2 | 6/11/2017 14:53:57 | Amazon    | L7      | Product Manager   |                    310000 | Seattle, WA       |                 8   |              0   |   nan |       155000 |                 0 |       0 |      nan |            nan |    11527 |     819 |           3 |                0 |                  0 |                  0 |            0 |              0 |            0 |            0 |                  0 |            0 |               0 |    nan |         nan |

</div>

→ 칼럼 정보 파악
```python
salary_df.info()
```
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 62642 entries, 0 to 62641
Data columns (total 29 columns):
 #   Column                   Non-Null Count  Dtype  
---  ------                   --------------  -----  
 0   timestamp                62642 non-null  object 
 1   company                  62637 non-null  object 
 2   level                    62523 non-null  object 
 3   title                    62642 non-null  object 
 4   totalyearlycompensation  62642 non-null  int64  
 5   location                 62642 non-null  object 
 6   yearsofexperience        62642 non-null  float64
 7   yearsatcompany           62642 non-null  float64
 8   tag                      61788 non-null  object 
 9   basesalary               62642 non-null  float64
 10  stockgrantvalue          62642 non-null  float64
 11  bonus                    62642 non-null  float64
 12  gender                   43102 non-null  object 
 13  otherdetails             40137 non-null  object 
 14  cityid                   62642 non-null  int64  
 15  dmaid                    62640 non-null  float64
 16  rowNumber                62642 non-null  int64  
 17  Masters_Degree           62642 non-null  int64  
 18  Bachelors_Degree         62642 non-null  int64  
 19  Doctorate_Degree         62642 non-null  int64  
 20  Highschool               62642 non-null  int64  
 21  Some_College             62642 non-null  int64  
 22  Race_Asian               62642 non-null  int64  
 23  Race_White               62642 non-null  int64  
 24  Race_Two_Or_More         62642 non-null  int64  
 25  Race_Black               62642 non-null  int64  
 26  Race_Hispanic            62642 non-null  int64  
 27  Race                     22427 non-null  object 
 28  Education                30370 non-null  object 
dtypes: float64(6), int64(13), object(10)
memory usage: 13.9+ MB
```

### 불필요한 칼럼 삭제
- cityid, dmaid, rowNumber 칼럼은 불필요하다고 판단
- Masters_Degree ~ Race_Hispanic 칼럼은 'Race'와 'Education'의 정보를 더미변수화해놓은 칼럼이므로 우선 삭제

```python
drop_columns = salary_df.loc[:, 'cityid':'Race_Hispanic'].columns
salary_df.drop(drop_columns, axis='columns', inplace=True)
salary_df.head(3)
```

<div class="code-example" markdown="1">

|    | timestamp          | company   | level   | title             |   totalyearlycompensation | location          |   yearsofexperience |   yearsatcompany |   tag |   basesalary |   stockgrantvalue |   bonus |   gender |   otherdetails |   Race |   Education |
|---:|:-------------------|:----------|:--------|:------------------|--------------------------:|:------------------|--------------------:|-----------------:|------:|-------------:|------------------:|--------:|---------:|---------------:|-------:|------------:|
|  0 | 6/7/2017 11:33:27  | Oracle    | L3      | Product Manager   |                    127000 | Redwood City, CA  |                 1.5 |              1.5 |   nan |       107000 |             20000 |   10000 |      nan |            nan |    nan |         nan |
|  1 | 6/10/2017 17:11:29 | eBay      | SE 2    | Software Engineer |                    100000 | San Francisco, CA |                 5   |              3   |   nan |            0 |                 0 |       0 |      nan |            nan |    nan |         nan |
|  2 | 6/11/2017 14:53:57 | Amazon    | L7      | Product Manager   |                    310000 | Seattle, WA       |                 8   |              0   |   nan |       155000 |                 0 |       0 |      nan |            nan |    nan |         nan |

</div>

### 중복값 제거

```python
salary_df.duplicated().sum()  # 중복값이 포함되어 있나 확인 (모든 열의 데이터가 같은 경우)
```
```
44
```

→ 모든 컬럼이 중복되는 경우는 실수로 중복 기입된 것이라 간주, 하나의 row만 남기고 삭제

```python
print('중복 제거 이전: ', len(salary_df))

salary_df.drop_duplicates(inplace=True, ignore_index=True)
print('중복 제거 이후: ', len(salary_df))
```
```
중복 제거 이전:  62642
중복 제거 이후:  62598
```

### 결측치 채우기

```python
salary_df.isna().sum()
```
```
timestamp                      0
company                        5
level                        119
title                          0
totalyearlycompensation        0
location                       0
yearsofexperience              0
yearsatcompany                 0
tag                          854
basesalary                     0
stockgrantvalue                0
bonus                          0
gender                     19529
otherdetails               22467
Race                       40171
Education                  32232
dtype: int64
```

→ gender, Race, Education의 null값을 모두 'Unknown'으로 채워줌 (분석에 사용하기 위함)

```python
salary_df[['gender', 'Race', 'Education']] = salary_df[['gender', 'Race', 'Education']].fillna('Unknown')
salary_df.head(3)
```

<div class="code-example" markdown="1">

|    | timestamp          | company   | level   | title             |   totalyearlycompensation | location          |   yearsofexperience |   yearsatcompany |   tag |   basesalary |   stockgrantvalue |   bonus | gender   |   otherdetails | Race    | Education   |
|---:|:-------------------|:----------|:--------|:------------------|--------------------------:|:------------------|--------------------:|-----------------:|------:|-------------:|------------------:|--------:|:---------|---------------:|:--------|:------------|
|  0 | 6/7/2017 11:33:27  | Oracle    | L3      | Product Manager   |                    127000 | Redwood City, CA  |                 1.5 |              1.5 |   nan |       107000 |             20000 |   10000 | Unknown  |            nan | Unknown | Unknown     |
|  1 | 6/10/2017 17:11:29 | eBay      | SE 2    | Software Engineer |                    100000 | San Francisco, CA |                 5   |              3   |   nan |            0 |                 0 |       0 | Unknown  |            nan | Unknown | Unknown     |
|  2 | 6/11/2017 14:53:57 | Amazon    | L7      | Product Manager   |                    310000 | Seattle, WA       |                 8   |              0   |   nan |       155000 |                 0 |       0 | Unknown  |            nan | Unknown | Unknown     |

</div>

→ 잘 채워졌는지 확인

```python
salary_df.isna().sum()
```
```
timestamp                      0
company                        5
level                        119
title                          0
totalyearlycompensation        0
location                       0
yearsofexperience              0
yearsatcompany                 0
tag                          854
basesalary                     0
stockgrantvalue                0
bonus                          0
gender                         0
otherdetails               22467
Race                           0
Education                      0
dtype: int64
```

### 이상한 값이 있는지 확인

1. 숫자형 칼럼 점검

    ```python
    salary_df.describe()
    ```

    <div class="code-example" markdown="1">

    |       |   totalyearlycompensation |      yearsofexperience |         yearsatcompany |          basesalary |     stockgrantvalue |                bonus |
    |:------|--------------------------:|-----------------------:|-----------------------:|--------------------:|--------------------:|---------------------:|
    | count |         62642.0           | 62642.0                | 62642.0                |   62642.0           |   62642.0           |   62642.0            |
    | mean  |        216300.37364707384 |     7.2041350850866825 |     2.7020929408384147 |  136687.28129689346 |   51486.08073259315 |   19334.746587752627 |
    | std   |        138033.7463773671  |     5.84037534823308   |     3.263655591673307  |   61369.27805673717 |   81874.56939076641 |   26781.292039968368 |
    | min   |         10000.0           |     0.0                |     0.0                |       0.0           |       0.0           |       0.0            |
    | 25%   |        135000.0           |     3.0                |     0.0                |  108000.0           |       0.0           |    1000.0            |
    | 50%   |        188000.0           |     6.0                |     2.0                |  140000.0           |   25000.0           |   14000.0            |
    | 75%   |        264000.0           |    10.0                |     4.0                |  170000.0           |   65000.0           |   26000.0            |
    | max   |       4980000.0           |    69.0                |    69.0                | 1659870.0           | 2800000.0           | 1000000.0            |

    </div>
    - 전반적으로 특별히 이상한 값은 없다고 생각됨

    ```python
    fig = px.box(salary_df, x='yearsatcompany', height=300)
    fig.show()
    ```
    <iframe width="600" height="300" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/51.embed"></iframe>

    ```python
    salary_df.query('yearsatcompany == 69')
    ```

    <div class="code-example" markdown="1">

    |       | timestamp         | company   |   level | title            |   totalyearlycompensation | location   |   yearsofexperience |   yearsatcompany | tag                |   basesalary |   stockgrantvalue |   bonus | gender   | otherdetails    | Race    | Education   |
    |------:|:------------------|:----------|--------:|:-----------------|--------------------------:|:-----------|--------------------:|-----------------:|:-------------------|-------------:|------------------:|--------:|:---------|:----------------|:--------|:------------|
    | 46988 | 4/3/2021 11:04:46 | Disney    |       5 | Product Designer |                    102000 | Crapo, MD  |                  69 |               69 | Interaction Design |       100000 |              2000 |       0 | Unknown  | Title: Joiooooo | Unknown | Unknown     |

    </div>

    - yeartsatcompany == 69라는 값이 다른 값과 너무 크기가 다르긴 하지만, 그럴 수 있다고 생각됨.

1. gender 칼럼 점검

    ```python
    salary_df['gender'].unique()
    ```
    ```
    array(['Unknown', 'Male', 'Female', 'Other', 'Title: Senior Software Engineer'], dtype=object)
    ```
    - gender에 'Title: Senior Software Engineer'라는 값이 들어가 있는 것은 잘못 기입된 것이라 생각됨

    ```python
    salary_df.query('gender == "Title: Senior Software Engineer"')
    ```

    <div class="code-example" markdown="1">

    |       | timestamp         | company   | level   | title             |   totalyearlycompensation | location   |   yearsofexperience |   yearsatcompany | tag                            |   basesalary |   stockgrantvalue |   bonus | gender                          |   otherdetails | Race    | Education   |
    |------:|:------------------|:----------|:--------|:------------------|--------------------------:|:-----------|--------------------:|-----------------:|:-------------------------------|-------------:|------------------:|--------:|:--------------------------------|---------------:|:--------|:------------|
    | 11010 | 9/17/2019 6:23:02 | GitHub    | E4      | Software Engineer |                    205000 | Buda, TX   |                  15 |                4 | Distributed Systems (Back-End) |       177000 |             31000 |    1000 | Title: Senior Software Engineer |            nan | Unknown | Unknown     |

    </div>

    → gender == "Title: Senior Software Engineer"라고 기입되어 있던 index=10985 행의 gender 값을 'Unknown'으로 바꿔줌
    ```python
    salary_df.loc[10985, 'gender'] = 'Unknown'
    ```

1. Race, Education, title 칼럼 점검
- 특별히 이상한 값은 없음

    ```python
    salary_df['Race'].unique()
    ```
    ```
    array(['Unknown', 'White', 'Asian', 'Black', 'Two Or More', 'Hispanic'], dtype=object)
    ```

    ```python
    salary_df['Education'].unique()
    ```
    ```
    array(['Unknown', 'PhD', "Master's Degree", "Bachelor's Degree", 'Some College', 'Highschool'], dtype=object)
    ```

    ```python
    list(salary_df['title'].unique())
    ```
    ```
    ['Product Manager', 'Software Engineer', 'Software Engineering Manager', 'Data Scientist', 'Solution Architect',
    'Technical Program Manager', 'Human Resources', 'Product Designer', 'Marketing', 'Business Analyst', 
    'Hardware Engineer', 'Sales', 'Recruiter', 'Mechanical Engineer', 'Management Consultant']
    ```

### company 칼럼의 표기 통일

```python
salary_df['company'].nunique()
```
```
1631
```

```python
list(salary_df['company'].unique())
```
```
['Oracle', 'eBay', 'Amazon', 'Apple', 'Microsoft', 'Salesforce', 'Facebook', 'Uber', 'Oath', 'Google', 'Netflix',
'Pinterest', 'Linkedin',  'Adobe', 'LinkedIn', 'amazon', 'Symantec', 'Intel Corporation', 'Intel', (생략)]
```
- 'Apple' = 'apple', 'Intel' = 'Intel Corporation' 등 통일되지 않은 이름들이 꽤 있음

1. 공백을 제거하고 uppercase로 맞춰줌
    ```python
    salary_df['company'] = salary_df['company'].str.strip().str.upper()
    ```

2. llc, org, ltd 등 뒤에 붙는 다양한 단어들을 지워줌
    ```python
    salary_df['company'] = salary_df['company'].str.replace(' LLC', '').str.replace('.ORG', '').str.replace(' LTD', '').str.replace(' CORPORATION', '').str.replace(' INC', '')
    salary_df['company'] = salary_df['company'].str.replace(' MEDIA', '').str.replace(' GROUP', '').str.replace(' TECHNOLOGY', '').str.replace(' TECHNOLOGIES', '').str.strip()
    ```

    ```python
    salary_df['company'].nunique()
    ```
    ```
    1081
    ```

3. 같은 회사인데 이름이 다른 경우가 더 있는지 확인
- 앞에서부터 6글자를 떼어서 groupby로 묶어서 확인

    ```python
    temp = salary_df[['company']]
    temp['6letters'] = temp['company'].str[:6]
    temp_groupby = temp.groupby('6letters')['company'].agg([pd.Series.nunique, set])
    temp_groupby.query('nunique > 1')
    ```

    <div class="code-example" markdown="1">

    | 6letters   |   nunique | set                                                                    |
    |:-----------|----------:|:-----------------------------------------------------------------------|
    | AMAZON     |         3 | {'AMAZON.COM', 'AMAZON', 'AMAZON WEB SERVICES'}                        |
    | AMERIC     |         3 | {'AMERICAN FAMILY INSURANCE', 'AMERICAN EXPRESS', 'AMERICAN AIRLINES'} |
    | ARISTA     |         2 | {'ARISTA NETWORKS', 'ARISTA'}                                          |
    | BANK O     |         2 | {'BANK OF AMERICA MERRILL LYNCH', 'BANK OF AMERICA'}                   |
    | BETTER     |         3 | {'BETTER.COM', 'BETTERMENT', 'BETTER MORTGAGE'}                        |
    | (생략) | ... | ... |

    </div>

4. 같은 회사인데 표기가 다른 경우를 함수로 만들어서 정리해줌
- 'amazon.com'과 'amazon web services'처럼 계열사지만 다른 기업이라고 판단되는 경우는 그냥 둠

    ```python
    def clean_company_names(name):
        try: 
            if name.startswith('AMAZON.COM'): final_name = 'AMAZON'
            elif name.startswith('ARISTA'): final_name = 'ARISTA'
            elif name.startswith('BLOOMBERG'): final_name = 'BLOOMBERG'
            elif name.startswith('BOOKING'): final_name = 'BOOKING.COM'    
            elif name.startswith('DELOITTE CONSULTING'): final_name = 'DELOITTE CONSULTING'    
            elif name.startswith('FORD'): final_name = 'FORD'
            elif name.startswith('CADENCE'): final_name = 'CADENCE'
            elif name.startswith('MCKINSEY'): final_name = 'MCKINSEY & COMPANY                
            elif name.startswith('MOTOROLA'): final_name = 'MOTOROLA'
            elif name.startswith('NUANCE'): final_name = 'NUANCE          
            elif name.startswith('COSTCO'): final_name = 'COSTCO'  
            elif name.startswith('TOYOTA'): final_name = 'TOYOTA'      
            elif name.startswith('WALMART'): final_name = 'WALMART' 
            elif name.startswith('MOODY'): final_name = "MOODY'S"     
            elif name.startswith('MACY'): final_name = "MACY'S"
            elif name.startswith('ERNST'): final_name = "ERNST & YOUNG"
            elif name.startswith('JANE STREET'): final_name = 'JANE STREET CAPITAL'
            elif name.startswith('LIBERTY MUTUAL'): final_name = 'LIBERTY MUTUAL'
            elif name.startswith('SAMSUNG'): final_name = 'SAMSUNG'
            else: final_name = name
        except:
            final_name = name
        return final_name

    salary_df['company'] = salary_df['company'].apply(lambda name: clean_company_names(name))
    ```

→ 최종적으로 정리 완료된 기업 수:
```python
salary_df['company'].nunique()
```
```
1060
```

## 기업별, 직업별 salary

### 기업별 평균 salary

1. 평균 salary가 많은 기업 확인
- 100명 이상의 기록이 있는 기업에 한해 분석 (수가 적으면 1명의 값에 영향을 너무 크게 받으므로)

    ```python
    sal_comp = salary_df.groupby(['company'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_comp = sal_comp['totalyearlycompensation'].reset_index()
    sal_comp = sal_comp.query('count >= 100')

    fig = px.bar(sal_comp.sort_values('mean', ascending=False).head(10),
                x='company', y='mean', color='mean', 
                hover_name='company', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                labels={'mean':'average yearly compensation', 'count':'data count'},
                color_continuous_scale = 'Brwnyl')

    fig.update(layout_coloraxis_showscale=False)  # 원래 오른쪽에 나오게 되는 colorbar를 숨김
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/53.embed"></iframe>
    - 100개 이상의 기록이 있는 기업 중 가장 평균 yearly compensation이 높은 기업은 'Netflix'


1. 평균 salary가 많은 기업 top 10: gender별로 나눠서 확인

    ```python
    sal_comp2 = salary_df.groupby(['company', 'gender'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_comp2 = sal_comp2['totalyearlycompensation'].reset_index()

    top_10 = sal_comp.sort_values('mean', ascending=False).head(10)['company']
    sal_comp2 = sal_comp2[sal_comp2['company'].isin(top_10)]

    fig = px.scatter(sal_comp2, x='company', y='mean', color='gender',
                    category_orders={'company':top_10},   # 가장 평균 salary가 많은 기업부터 순서대로 시각화
                    hover_name='company', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(marker={'size': 10})  # marker의 size를 키워줌
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/55.embed"></iframe>
    - 10개 기업 모두 Male이 Female보다 평균 yearly compensation이 높음

1. 평균 salary가 많은 기업 top 10: job title별로 나눠서 확인

    ```python
    sal_comp3 = salary_df.groupby(['company', 'title'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_comp3 = sal_comp3['totalyearlycompensation'].reset_index()

    top_10 = sal_comp.sort_values('mean', ascending=False).head(10)['company']
    sal_comp3 = sal_comp3[sal_comp3['company'].isin(top_10)]

    fig = px.scatter(sal_comp3, x='company', y='mean', color='title',
                    category_orders={'company':top_10},   # 가장 평균 salary가 많은 기업부터 순서대로 시각화
                    hover_name='title', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(marker={'size': 10})  # marker의 size를 키워줌
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/57.embed"></iframe>
    - 10개 기업 모두 'Software Engineering Manager' title이 평균 yearly compensation이 가장 높음


### title별 평균 salary

1. job title별 평균 salary 확인

    ```python
    sal_title = salary_df.groupby(['title'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_title = sal_title['totalyearlycompensation'].reset_index()

    fig = px.bar(sal_title.sort_values('mean', ascending=False),
                x='title', y='mean', color='mean',
                hover_name='title', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                labels={'mean':'average yearly compensation', 'count':'data count'},
                color_continuous_scale = 'Brwnyl')

    fig.update(layout_coloraxis_showscale=False)  # 원래 오른쪽에 나오게 되는 colorbar를 숨김
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/59.embed"></iframe>
    - 'Software Engineering Manager' title이 가장 평균 yearly compensation이 높음
    - 그 다음으로 평균 yearly compensation이 높은 건 manager 직급인 'Product Manager', 'Technical Program Manager' title


1. job title별 평균 salary: gender별로 나눠서 확인

    ```python
    sal_title2 = salary_df.groupby(['title', 'gender'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_title2 = sal_title2['totalyearlycompensation'].reset_index()

    top_order = sal_title.sort_values('mean', ascending=False)['title']

    fig = px.scatter(sal_title2, x='title', y='mean', color='gender',
                    category_orders={'title':top_order},  # 가장 평균 salary가 많은 title부터 순서대로 시각화
                    hover_name='title', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(marker={'size': 10})  # marker의 size를 키워줌
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/63.embed"></iframe>
    - 대체로 모든 직군에서 Male이 Female보다 평균 yearly compensation이 높음
    - Mechanical Engineer, Recruiter, Business Analyst 직군은 남녀 차이가 거의 없음


1. job title별 평균 salary: education level별로 나눠서 확인

    ```python
    sal_title3 = salary_df.groupby(['title', 'Education'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_title3 = sal_title3['totalyearlycompensation'].reset_index()

    top_order = sal_title.sort_values('mean', ascending=False)['title']

    fig = px.scatter(sal_title3, x='title', y='mean', color='Education',
                    category_orders={'title':top_order},  # 가장 평균 salary가 많은 title부터 순서대로 시각화
                    hover_name='title', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(marker={'size': 10})  # marker의 size를 키워줌
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/77.embed"></iframe>
    - 대부분의 직종에서 PhD를 취득한 사람이 평균 yearly compensation이 가장 높음


### 기업별 title별 treemap
- 기록된 직원 정보가 많은 기업 top 20에 대해서 treemap을 그려봄  
  (회사별 규모 정보가 따로 있으면 좋겠지만, 없으므로 기록된 정보 수를 규모라고 간주하고 분석)

```python
top_20 = salary_df.groupby('company')[['title']].count().sort_values(by='title', ascending=False).head(20).index

fig = px.treemap(salary_df[(salary_df['company'].notnull()) & (salary_df['company'].isin(top_20))], 
                 path=[px.Constant('top 20'), 'company', 'title'],
                 color='totalyearlycompensation', color_continuous_scale='Brwnyl')

fig.update_layout(margin = dict(t=50, l=25, r=25, b=25))
fig.show()
```
<iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/87.embed"></iframe>
- 대부분의 기업에서 'Software Engineering Manager' title이 가장 평균 yearly compensation이 높음




## 성별, 인종, 교육수준별 salary

### gender별 평균 salary

```python
sal_gend = salary_df.groupby('gender')[['totalyearlycompensation']].agg(['mean', 'count'])
sal_gend = sal_gend['totalyearlycompensation'].reset_index()

fig = px.bar(sal_gend, x='gender', y='mean', color='gender',
             hover_name='gender', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
             labels={'mean':'average yearly compensation', 'count':'data count'},
             color_discrete_sequence=px.colors.qualitative.Antique)

fig.update_traces(showlegend=False)  # 원래 오른쪽에 나타나게 되는 legend를 숨김
fig.show()
```
<iframe width="500" height="300" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/67.embed"></iframe>
- Male이 Female보다 평균 yearly compensation이 높음


### race별 평균 salary

1. race별 평균 yearly compensation

    ```python
    sal_race = salary_df.groupby('Race')[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_race = sal_race['totalyearlycompensation'].reset_index()

    fig = px.bar(sal_race, x='Race', y='mean', color='Race',
                category_orders={'Race':['White', 'Asian', 'Black', 'Hispanic']},
                hover_name='Race', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                labels={'mean':'average yearly compensation', 'count':'data count'},
                color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(showlegend=False)  # 원래 오른쪽에 나타나게 되는 legend를 숨김
    fig.show()
    ```
    <iframe width="600" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/69.embed"></iframe>
    - Two Or More, Unknown을 제외하면 백인이 가장 평균 yearly compensation이 많고, 흑인이 가장 적음

1. gender * race 교차해서 비교

    ```python
    sal_race_gend = salary_df.groupby(['gender', 'Race'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_race_gend = sal_race_gend['totalyearlycompensation'].reset_index()

    fig = px.scatter(sal_race_gend, x='Race', y='mean', color='gender',
                    category_orders={'Race':['White', 'Asian', 'Black', 'Hispanic']},
                    hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(marker={'size': 10})  # marker의 size를 키워줌
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/71.embed"></iframe>
    - Two Or More, Unknown을 제외하면 백인 남성이 가장 평균 yearly compensation이 많음
    - 혼혈(Two Or More)은 특이하게도 여성이 남성보다 평균 yearly compensation이 많음

1. race별 gender별 분포 비교

    ```python
    fig = px.violin(salary_df.query('gender == "Male" or gender == "Female"'), 
                x='Race', y='totalyearlycompensation', color='gender',
                category_orders={'Race':['White', 'Asian', 'Black', 'Hispanic'], 'gender':['Female', 'Male']},
                hover_name='company', hover_data=['title'],
                labels={'totalyearlycompensation':'average yearly compensation'},
                color_discrete_sequence=px.colors.qualitative.Antique)

    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="../../../assets/images/kaggle/stem_salaries_1.html"></iframe>
    - 최상위권에는 white / asian male이 가장 많음 (Unknown 제외)
    - white는 female도 최상위권에 꽤 분포
    - Black은 1M 이상이 1명, Hispanic은 없음


### education별 평균 salary

1. education별 평균 yearly compensation

    ```python
    sal_edu = salary_df.groupby('Education')[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_edu = sal_edu['totalyearlycompensation'].reset_index()

    fig = px.bar(sal_edu, x='Education', y='mean', color='Education',
                category_orders={'Education':['PhD', "Master's Degree", "Bachelor's Degree", 'Some College', 'Highschool']},
                hover_name='Education', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                labels={'mean':'average yearly compensation', 'count':'data count'},
                color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(showlegend=False)  # 원래 오른쪽에 나타나게 되는 legend를 숨김
    fig.show()
    ```
    <iframe width="600" height="400" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/73.embed"></iframe>
    - PhD (박사 학위)를 취득한 사람이 평균 yearly compensation이 가장 높음

1. gender * education 교차해서 비교

    ```python
    sal_edu_gend = salary_df.groupby(['gender', 'Education'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_edu_gend = sal_edu_gend['totalyearlycompensation'].reset_index()

    fig = px.scatter(sal_edu_gend, x='Education', y='mean', color='gender',
                    category_orders={'Education':['PhD', "Master's Degree", "Bachelor's Degree", 'Some College', 'Highschool']},
                    hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_discrete_sequence=px.colors.qualitative.Antique)

    fig.update_traces(marker={'size': 10})  # marker의 size를 키워줌
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/75.embed"></iframe>
    - PhD까지 취득한 남성이 평균 yearly compensation이 가장 높음
    - Bachelor's Degree (학사 학위)를 취득한 여성이 평균 yearly compensation이 가장 낮음 (Highschool이 최종학력인 여성의 경우 데이터 양이 적어서 유의미한 비교라고 하기는 애매)


1. educaiton별 gender별 분포 비교

    ```python
    orders_dict = {'Education':['PhD', "Master's Degree", "Bachelor's Degree", 'Some College', 'Highschool'],
                'gender':['Female', 'Male']}

    fig = px.violin(salary_df.query('gender == "Male" or gender == "Female"'), 
                x='Education', y='totalyearlycompensation', color='gender',
                category_orders = orders_dict,
                hover_name='company', hover_data=['title'],
                labels={'totalyearlycompensation':'average yearly compensation'},
                color_discrete_sequence=px.colors.qualitative.Antique)

    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="../../../assets/images/kaggle/stem_salaries_2.html"></iframe>
    - 오히려 최상위권(1M 이상)에는 PhD보다 Masters's & Bacher's Degree 소지자가 더 많음


## location별 salary

### 가장 많은 회사가 위치한 location

1. 가장 많은 회사가 위치한 location top 10

    ```python
    # 데이터에 기록된 회사만을 대상으로 계산
    comp_loc = salary_df.groupby(['location'])[['company']].nunique().reset_index()

    fig = px.bar(comp_loc.sort_values('company', ascending=False).head(10),
                x='location', y='company', color='company', 
                hover_name='location',  labels={'company':'# of companies'},
                color_continuous_scale = 'Brwnyl')

    fig.update(layout_coloraxis_showscale=False)  # 원래 오른쪽에 나오게 되는 colorbar를 숨김
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/79.embed"></iframe>
    - 약 400개의 회사가 위치한 San Francisco가 가장 인기 있는 location


1. 가장 많은 회사가 위치한 location top 10에 대해, 그 곳에 위치한 회사 & 화사별 salary 정보를 쪼개어 확인  

    ```python
    sal_loc2 = salary_df.groupby(['location', 'company'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_loc2 = sal_loc2['totalyearlycompensation'].reset_index()

    top_comp_locations = comp_loc.sort_values('company', ascending=False).head(10)['location']

    fig = px.treemap(sal_loc2[sal_loc2['location'].isin(top_comp_locations)],
                    path = [px.Constant('top locations'), 'location', 'company'],
                    values='count', color = 'mean', hover_data=['count'],
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_continuous_scale = 'Brwnyl')

    fig.update_layout(margin = dict(t=50, l=25, r=25, b=25))
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/81.embed"></iframe>
    - 가장 많은 회사가 위치한 San Francisco의 경우, Google, Uber, Salesforce, Amazon, Facebook 등이 대표적인 회사
    - San Francisco의 평균 yearly compensation이 Seattle이나 New York보다 다소 높은 편


### 가장 평균 salary가 높은 location

1. 평균 salary가 높은 location top 10
- 100명 이상의 기록이 있는 위치에 한해 분석 (수가 적으면 1명의 값에 영향을 너무 크게 받으므로)

    ```python
    sal_loc = salary_df.groupby(['location'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_loc = sal_loc['totalyearlycompensation'].reset_index()
    sal_loc = sal_loc.query('count >= 100')

    fig = px.bar(sal_loc.sort_values('mean', ascending=False).head(10),
                x='location', y='mean', color='mean', 
                hover_name='location', hover_data=['count'], # hover했을 때의 제목 & 정보 추가
                labels={'mean':'average yearly compensation', 'count':'data count'},
                color_continuous_scale = 'Brwnyl')

    fig.update(layout_coloraxis_showscale=False)  # 원래 오른쪽에 나오게 되는 colorbar를 숨김
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/83.embed"></iframe>
    - 100명 이상의 기록이 있는 위치 중, 평균 yearly compensation이 가장 높은 location은 Los Gatos


1. 평균 salary가 가장 많은 위치 top 10에 대해, 그 곳에 위치한 회사 & 화사별 salary 정보를 쪼개어 확인  

    ```python
    sal_loc2 = salary_df.groupby(['location', 'company'])[['totalyearlycompensation']].agg(['mean', 'count'])
    sal_loc2 = sal_loc2['totalyearlycompensation'].reset_index()

    top_locations = sal_loc.sort_values('mean', ascending=False).head(10)['location']

    fig = px.treemap(sal_loc2[sal_loc2['location'].isin(top_locations)],
                    path = [px.Constant('top locations'), 'location', 'company'],
                    values='count', color = 'mean', hover_data=['count'],
                    labels={'mean':'average yearly compensation', 'count':'data count'},
                    color_continuous_scale = 'Brwnyl')

    fig.update_layout(margin = dict(t=50, l=25, r=25, b=25))
    fig.show()
    ```
    <iframe width="700" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chaeyun1248/85.embed"></iframe>
    - Los Gatos는 Netflix가 있어서 평균 salary가 높게 나온 듯
    - Los Gatos는 Netflix, Menlo Park은 Facebook, Cupertino는 Apple의 평균 salary가 큰 영향

