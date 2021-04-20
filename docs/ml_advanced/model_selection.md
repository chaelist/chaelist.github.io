---
layout: default
title: Model Selection
parent: Machine Learning 심화
nav_order: 3
--- 

# Model Selection
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

## k-겹 교차 검증 (k-fold cross validation)
: 머신 러닝 모델의 성능을 보다 정확하게 평가할 수 있는 방법.
- test set과 training set을 임의로 나눌 때, 어떻게 나뉘는지에 따라 딱 이 test set에서만 성능이 좋게 나올 수도 있고, 안좋게 나올 수도 있다. 이런 문제를 해결해주는 것이 교차 검증!
- k값은 데이터를 몇 세트로 나눌지를 의미. 데이터 수에 따라 다르지만, 가장 일반적으로 사용하는 숫자는 5 
- +) 데이터가 많을수록 우연히 test set에서만 성능이 다르게 나올 확률이 적기 때문에 작은 k를 사용해도 된다


### k-겹 교차 검증 방법
1. 먼저 전체 데이터를 k 개의 같은 사이즈로 나눈다. 
  - ex) k=5, 데이터가 총 1000개가 있다면 이 데이터를 200개씩 5개의 셋으로 나눈다
2. 맨 처음에는 첫 번째 데이터 셋을 test test으로 사용하고, 나머지를 training set으로 사용
3. 그 다음에는 두 번째 데이터 셋을 test set으로 사용하고, 나머지 4개를 training set으로 사용해서 다시 모델을 학습시키고, 성능을 파악
4. 이 과정을 모든 데이터 셋에 반복 → 5개의 테스트 셋에 대한 각 성능 5개의 평균을 모델 성능으로 본다   
(모델의 성능을 여러 번 다른 데이터로 검증하기 때문에 평가에 대한 신뢰가 올라가는 것!)


### scikit-learn으로 구현
- sklearn.model_selection.cross_val_score를 사용

```python
from sklearn import datasets
from sklearn.model_selection import cross_val_score   # 교차 검증을 해주는 함수
from sklearn.linear_model import LogisticRegression

import numpy as np
import pandas as pd

# 경고 메시지 출력을 막는 코드 추가
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)
```

***iris dataset 준비**
```python
iris_data = datasets.load_iris()

X = pd.DataFrame(iris_data.data, columns=iris_data.feature_names)
y = pd.DataFrame(iris_data.target, columns=['Class'])

X.head()
```

<div class="code-example" markdown="1">

|    |   sepal length (cm) |   sepal width (cm) |   petal length (cm) |   petal width (cm) |
|---:|--------------------:|-------------------:|--------------------:|-------------------:|
|  0 |                 5.1 |                3.5 |                 1.4 |                0.2 |
|  1 |                 4.9 |                3   |                 1.4 |                0.2 |
|  2 |                 4.7 |                3.2 |                 1.3 |                0.2 |
|  3 |                 4.6 |                3.1 |                 1.5 |                0.2 |
|  4 |                 5   |                3.6 |                 1.4 |                0.2 |

</div>

&nbsp;
{: .fs-1 .lh-0}

```python
y.head()
```

<div class="code-example" markdown="1">

|    |   Class |
|---:|--------:|
|  0 |       0 |
|  1 |       0 |
|  2 |       0 |
|  3 |       0 |
|  4 |       0 |

</div>


***LogisticRegression 모델로 교차 검증**
```python
logistic_model = LogisticRegression(max_iter=2000)

cross_val_score(logistic_model, X, y.values.ravel(), cv=5)
# ravel(): 다차원 array를 1차원 array로 평평하게 펴주는 함수.
```
```
array([0.96666667, 1.        , 0.93333333, 0.96666667, 1.        ])
```
- cross_val_score: 알아서 모델을 학습시켜서 k겹 교차검증을 해주고, 각 k개의 성능이 return된다
    - 파라미터: k-겹 교차검증을 해줄 모델명, X값, y값, cv = k값을 결정. 
    - 여기서는 cv=5니까 5겹 교차검증을 한다는 뜻.

**→ cross_val_score의 결과로 return된 값들을 평균내준다**
```python
# 이렇게 평균낸 값을 해당 모델의 성능으로 생각
np.average(cross_val_score(logistic_model, X, y.values.ravel(), cv=5))
```
```
0.9733333333333334
```


## 그리드 서치 (Grid Search)
: 좋은 하이퍼파라미터를 고르는 방법 중 하나
1. 우선 정해줘야 하는 각 하이퍼파라미터에 넣어보고 싶은 후보 값을 몇 개씩 정해준다
2. 선택한 후보 값들의 모든 조합을 써서 학습을 시키고, 그중 성능이 가장 좋게 나오는 하이퍼파라미터 조합을 고른다

### scikit-learn으로 구현
- sklearn.model_selection.GridSearchCV를 사용

```python
from sklearn import datasets
from sklearn.linear_model import Lasso
from sklearn.preprocessing import PolynomialFeatures

from sklearn.model_selection import GridSearchCV   # Grid Search를 쉽게 해주는 함수

import numpy as np
import pandas as pd
```

**1. boston housing 데이터 준비**
```python
boston_dataset = datasets.load_boston()

X = pd.DataFrame(boston_dataset.data, columns=boston_dataset.feature_names)

# 입력변수를 6차항으로 바꾸기
polynomial_transformer = PolynomialFeatures(2)   # 2차항 변환기 준비
polynomial_features = polynomial_transformer.fit_transform(X.values)
features = polynomial_transformer.get_feature_names(X.columns)

# 바꾼 값들을 다시 X로 저장
X = pd.DataFrame(polynomial_features, columns=features)
X.head()
```

<div class="code-example" markdown="1">
 
|    |   1 |    CRIM |   ZN |   INDUS |   CHAS |   NOX |    RM |   AGE |    DIS |   RAD |   TAX |   PTRATIO |      B |   LSTAT |      CRIM^2 |   CRIM ZN |   CRIM INDUS |   CRIM CHAS |   CRIM NOX |   CRIM RM |   CRIM AGE |   CRIM DIS |   CRIM RAD |   CRIM TAX |   CRIM PTRATIO |   CRIM B |   CRIM LSTAT |   ZN^2 |   ZN INDUS |   ZN CHAS |   ZN NOX |   ZN RM |   ZN AGE |   ZN DIS |   ZN RAD |   ZN TAX |   ZN PTRATIO |   ZN B |   ZN LSTAT |   INDUS^2 |   INDUS CHAS |   INDUS NOX |   INDUS RM |   INDUS AGE |   INDUS DIS |   INDUS RAD |   INDUS TAX |   INDUS PTRATIO |   INDUS B |   INDUS LSTAT |   CHAS^2 |   CHAS NOX |   CHAS RM |   CHAS AGE |   CHAS DIS |   CHAS RAD |   CHAS TAX |   CHAS PTRATIO |   CHAS B |   CHAS LSTAT |    NOX^2 |   NOX RM |   NOX AGE |   NOX DIS |   NOX RAD |   NOX TAX |   NOX PTRATIO |   NOX B |   NOX LSTAT |    RM^2 |   RM AGE |   RM DIS |   RM RAD |   RM TAX |   RM PTRATIO |    RM B |   RM LSTAT |   AGE^2 |   AGE DIS |   AGE RAD |   AGE TAX |   AGE PTRATIO |   AGE B |   AGE LSTAT |   DIS^2 |   DIS RAD |   DIS TAX |   DIS PTRATIO |   DIS B |   DIS LSTAT |   RAD^2 |   RAD TAX |   RAD PTRATIO |   RAD B |   RAD LSTAT |   TAX^2 |   TAX PTRATIO |    TAX B |   TAX LSTAT |   PTRATIO^2 |   PTRATIO B |   PTRATIO LSTAT |    B^2 |   B LSTAT |   LSTAT^2 |
|---:|----:|--------:|-----:|--------:|-------:|------:|------:|------:|-------:|------:|------:|----------:|-------:|--------:|------------:|----------:|-------------:|------------:|-----------:|----------:|-----------:|-----------:|-----------:|-----------:|---------------:|---------:|-------------:|-------:|-----------:|----------:|---------:|--------:|---------:|---------:|---------:|---------:|-------------:|-------:|-----------:|----------:|-------------:|------------:|-----------:|------------:|------------:|------------:|------------:|----------------:|----------:|--------------:|---------:|-----------:|----------:|-----------:|-----------:|-----------:|-----------:|---------------:|---------:|-------------:|---------:|---------:|----------:|----------:|----------:|----------:|--------------:|--------:|------------:|--------:|---------:|---------:|---------:|---------:|-------------:|--------:|-----------:|--------:|----------:|----------:|----------:|--------------:|--------:|------------:|--------:|----------:|----------:|--------------:|--------:|------------:|--------:|----------:|--------------:|--------:|------------:|--------:|--------------:|---------:|------------:|------------:|------------:|----------------:|-------:|----------:|----------:|
|  0 |   1 | 0.00632 |   18 |    2.31 |      0 | 0.538 | 6.575 |  65.2 | 4.09   |     1 |   296 |      15.3 | 396.9  |    4.98 | 3.99424e-05 |   0.11376 |    0.0145992 |           0 | 0.00340016 |  0.041554 |   0.412064 |  0.0258488 |    0.00632 |    1.87072 |       0.096696 |  2.50841 |    0.0314736 |    324 |      41.58 |         0 |    9.684 |  118.35 |   1173.6 |    73.62 |       18 |     5328 |        275.4 | 7144.2 |      89.64 |    5.3361 |            0 |     1.24278 |    15.1883 |     150.612 |      9.4479 |        2.31 |      683.76 |          35.343 |   916.839 |       11.5038 |        0 |          0 |         0 |          0 |          0 |          0 |          0 |              0 |        0 |            0 | 0.289444 |  3.53735 |   35.0776 |   2.20042 |     0.538 |   159.248 |        8.2314 | 213.532 |     2.67924 | 43.2306 |  428.69  |  26.8917 |    6.575 |  1946.2  |      100.598 | 2609.62 |    32.7435 | 4251.04 |   266.668 |      65.2 |   19299.2 |        997.56 | 25877.9 |     324.696 | 16.7281 |    4.09   |   1210.64 |       62.577  | 1623.32 |     20.3682 |       1 |       296 |          15.3 |  396.9  |        4.98 |   87616 |        4528.8 | 117482   |     1474.08 |      234.09 |     6072.57 |          76.194 | 157530 |   1976.56 |   24.8004 |
|  1 |   1 | 0.02731 |    0 |    7.07 |      0 | 0.469 | 6.421 |  78.9 | 4.9671 |     2 |   242 |      17.8 | 396.9  |    9.14 | 0.000745836 |   0       |    0.193082  |           0 | 0.0128084  |  0.175358 |   2.15476  |  0.135652  |    0.05462 |    6.60902 |       0.486118 | 10.8393  |    0.249613  |      0 |       0    |         0 |    0     |    0    |      0   |     0    |        0 |        0 |          0   |    0   |       0    |   49.9849 |            0 |     3.31583 |    45.3965 |     557.823 |     35.1174 |       14.14 |     1710.94 |         125.846 |  2806.08  |       64.6198 |        0 |          0 |         0 |          0 |          0 |          0 |          0 |              0 |        0 |            0 | 0.219961 |  3.01145 |   37.0041 |   2.32957 |     0.938 |   113.498 |        8.3482 | 186.146 |     4.28666 | 41.2292 |  506.617 |  31.8937 |   12.842 |  1553.88 |      114.294 | 2548.49 |    58.6879 | 6225.21 |   391.904 |     157.8 |   19093.8 |       1404.42 | 31315.4 |     721.146 | 24.6721 |    9.9342 |   1202.04 |       88.4144 | 1971.44 |     45.3993 |       4 |       484 |          35.6 |  793.8  |       18.28 |   58564 |        4307.6 |  96049.8 |     2211.88 |      316.84 |     7064.82 |         162.692 | 157530 |   3627.67 |   83.5396 |
|  2 |   1 | 0.02729 |    0 |    7.07 |      0 | 0.469 | 7.185 |  61.1 | 4.9671 |     2 |   242 |      17.8 | 392.83 |    4.03 | 0.000744744 |   0       |    0.19294   |           0 | 0.012799   |  0.196079 |   1.66742  |  0.135552  |    0.05458 |    6.60418 |       0.485762 | 10.7203  |    0.109979  |      0 |       0    |         0 |    0     |    0    |      0   |     0    |        0 |        0 |          0   |    0   |       0    |   49.9849 |            0 |     3.31583 |    50.798  |     431.977 |     35.1174 |       14.14 |     1710.94 |         125.846 |  2777.31  |       28.4921 |        0 |          0 |         0 |          0 |          0 |          0 |          0 |              0 |        0 |            0 | 0.219961 |  3.36976 |   28.6559 |   2.32957 |     0.938 |   113.498 |        8.3482 | 184.237 |     1.89007 | 51.6242 |  439.003 |  35.6886 |   14.37  |  1738.77 |      127.893 | 2822.48 |    28.9555 | 3733.21 |   303.49  |     122.2 |   14786.2 |       1087.58 | 24001.9 |     246.233 | 24.6721 |    9.9342 |   1202.04 |       88.4144 | 1951.23 |     20.0174 |       4 |       484 |          35.6 |  785.66 |        8.06 |   58564 |        4307.6 |  95064.9 |      975.26 |      316.84 |     6992.37 |          71.734 | 154315 |   1583.1  |   16.2409 |
|  3 |   1 | 0.03237 |    0 |    2.18 |      0 | 0.458 | 6.998 |  45.8 | 6.0622 |     3 |   222 |      18.7 | 394.63 |    2.94 | 0.00104782  |   0       |    0.0705666 |           0 | 0.0148255  |  0.226525 |   1.48255  |  0.196233  |    0.09711 |    7.18614 |       0.605319 | 12.7742  |    0.0951678 |      0 |       0    |         0 |    0     |    0    |      0   |     0    |        0 |        0 |          0   |    0   |       0    |    4.7524 |            0 |     0.99844 |    15.2556 |      99.844 |     13.2156 |        6.54 |      483.96 |          40.766 |   860.293 |        6.4092 |        0 |          0 |         0 |          0 |          0 |          0 |          0 |              0 |        0 |            0 | 0.209764 |  3.20508 |   20.9764 |   2.77649 |     1.374 |   101.676 |        8.5646 | 180.741 |     1.34652 | 48.972  |  320.508 |  42.4233 |   20.994 |  1553.56 |      130.863 | 2761.62 |    20.5741 | 2097.64 |   277.649 |     137.4 |   10167.6 |        856.46 | 18074.1 |     134.652 | 36.7503 |   18.1866 |   1345.81 |      113.363  | 2392.33 |     17.8229 |       9 |       666 |          56.1 | 1183.89 |        8.82 |   49284 |        4151.4 |  87607.9 |      652.68 |      349.69 |     7379.58 |          54.978 | 155733 |   1160.21 |    8.6436 |
|  4 |   1 | 0.06905 |    0 |    2.18 |      0 | 0.458 | 7.147 |  54.2 | 6.0622 |     3 |   222 |      18.7 | 396.9  |    5.33 | 0.0047679   |   0       |    0.150529  |           0 | 0.0316249  |  0.4935   |   3.74251  |  0.418595  |    0.20715 |   15.3291  |       1.29123  | 27.4059  |    0.368036  |      0 |       0    |         0 |    0     |    0    |      0   |     0    |        0 |        0 |          0   |    0   |       0    |    4.7524 |            0 |     0.99844 |    15.5805 |     118.156 |     13.2156 |        6.54 |      483.96 |          40.766 |   865.242 |       11.6194 |        0 |          0 |         0 |          0 |          0 |          0 |          0 |              0 |        0 |            0 | 0.209764 |  3.27333 |   24.8236 |   2.77649 |     1.374 |   101.676 |        8.5646 | 181.78  |     2.44114 | 51.0796 |  387.367 |  43.3265 |   21.441 |  1586.63 |      133.649 | 2836.64 |    38.0935 | 2937.64 |   328.571 |     162.6 |   12032.4 |       1013.54 | 21512   |     288.886 | 36.7503 |   18.1866 |   1345.81 |      113.363  | 2406.09 |     32.3115 |       9 |       666 |          56.1 | 1190.7  |       15.99 |   49284 |        4151.4 |  88111.8 |     1183.26 |      349.69 |     7422.03 |          99.671 | 157530 |   2115.48 |   28.4089 |

</div>

&nbsp;
{: .fs-1 .lh-0}

```python
y = pd.DataFrame(boston_dataset.target, columns=['MEDV'])
y.head()
```

<div class="code-example" markdown="1">

|    |   MEDV |
|---:|-------:|
|  0 |   24.0 |
|  1 |   21.6 |
|  2 |   34.7 |
|  3 |   33.4 |
|  4 |   36.2 |

</div>

&nbsp;
{: .fs-1 .lh-0}

+) 데이터 셔플
```python
# boston housing 데이터는 미리 셔플하지 않으면 교차검증할 때 score값이 매우 낮게 나옴
# 이처럼, 데이터가 특정 순서에 따라 정렬되어 있는 경우 셔플해서 사용하는 게 좋다

joined_data = X.join(y)
shuffled_data = joined_data.sample(frac=1)
shuffled_X = shuffled_data.iloc[:, :-1]
shuffled_y = shuffled_data.iloc[:, -1]
```

**2. hyperparameter 후보 정리**
```python
# Grid Search를 통해 성능을 테스트해보고 싶은 hyperparameter를 dictionary 형태로 적어준다
# key: parameter명, value: 테스트해볼 값들의 리스트
hyper_parameter = {
    'alpha': [0.001, 0.001, 0.01, 0.1, 1],
    'max_iter': [500, 1000, 1500, 2000, 3000]
}
```

**3. Grid Search**
```python
lasso_model = Lasso()  ## hyperparameter를 우선 하나도 넣지 않고 모델을 만든다

grid_search = GridSearchCV(lasso_model, hyper_parameter, cv=5)
    # 검증할 모델, 검증할 hyperparameter들을 적어준 dictionary를 적어줌
    # cv는 성능을 몇 겹 교차검증으로 평가할지 정하는 파라미터. cv=5면 5겹 교차검증을 하겠다는 의미. (cv=5가 default)

grid_search.fit(shuffled_X, shuffled_y)  # 일반적인 모델을 학습시키듯이 fit()에 X, y를 써주면 된다
```
```
GridSearchCV(cv=5, error_score=nan,
             estimator=Lasso(alpha=1.0, copy_X=True, fit_intercept=True,
                             max_iter=1000, normalize=False, positive=False,
                             precompute=False, random_state=None,
                             selection='cyclic', tol=0.0001, warm_start=False),
             iid='deprecated', n_jobs=None,
             param_grid={'alpha': [0.001, 0.001, 0.01, 0.1, 1],
                         'max_iter': [500, 1000, 1500, 2000, 3000]},
             pre_dispatch='2*n_jobs', refit=True, return_train_score=False,
             scoring=None, verbose=0)
```
- `refit=True`가 default setting이라 여러 hyperparameter 조합을 모두 학습시켜본 후 모델을 최적의 조합으로 다시 학습시켜준다.


**4. 최적의 hyperparameter 조합 확인**
```python
grid_search.best_params_   ## 최적의 hyperparameter를 찾아줌
```
```
{'alpha': 0.1, 'max_iter': 3000}
```

+) best 조합의 score를 확인
```python
grid_search.best_score_
```
```
0.8425712188051439
```

+) score가 높은 Top 5 조합과 그 평균점수 확인
- `cv_results_`로 테스트된 각 조합별로, 각 교차검증 회별 점수와 평균점수 등을 모두 확인할 수 있다

```python
scores_df = pd.DataFrame(grid_search.cv_results_)
scores_df[scores_df['rank_test_score'] < 6][['params', 'mean_test_score', 'rank_test_score']]
```

<div class="code-example" markdown="1">

|    | params                           |   mean_test_score |   rank_test_score |
|---:|:---------------------------------|------------------:|------------------:|
| 16 | {'alpha': 0.1, 'max_iter': 1000} |          0.836727 |                 4 |
| 17 | {'alpha': 0.1, 'max_iter': 1500} |          0.838901 |                 3 |
| 18 | {'alpha': 0.1, 'max_iter': 2000} |          0.840577 |                 2 |
| 19 | {'alpha': 0.1, 'max_iter': 3000} |          0.842571 |                 1 |
| 24 | {'alpha': 1, 'max_iter': 3000}   |          0.834124 |                 5 |

</div>


### Randomized Search
- hyperparameter grid가 커질수록 Grid Search는 느려진다
- 그렇기에, 가능한 모든 조합을 시도하는 대신, random하게 grid 안 여러 조합을 적당히 건너뛰면서 시도하면 시간을 크게 단축할 수 있다
- Randomized Search를 활용하면 최적의 조합을 건너뛸 확률도 약간 존재하지만, 대체로 빠른 시간 내에 최적에 가까운 조합을 찾아낼 수 있다. (같은 시간 내에 더 많은 hyperparameter를 조정 가능)

```python
# Import RandomizedSearchCV
from sklearn.model_selection import RandomizedSearchCV

# GridSearch에서 활용했던 lasso_model와 hyper_parameter를 그대로 활용
lasso_model = Lasso() 

hyper_parameter = {
    'alpha': [0.001, 0.001, 0.01, 0.1, 1],
    'max_iter': [500, 1000, 1500, 2000, 3000]
}

# Randomized Search 모델을 만들고 fit
random_search = RandomizedSearchCV(lasso_model, hyper_parameter, cv=5)
random_search.fit(shuffled_X, shuffled_y)
```
```
RandomizedSearchCV(cv=5, error_score=nan,
                   estimator=Lasso(alpha=1.0, copy_X=True, fit_intercept=True,
                                   max_iter=1000, normalize=False,
                                   positive=False, precompute=False,
                                   random_state=None, selection='cyclic',
                                   tol=0.0001, warm_start=False),
                   iid='deprecated', n_iter=10, n_jobs=None,
                   param_distributions={'alpha': [0.001, 0.001, 0.01, 0.1, 1],
                                        'max_iter': [500, 1000, 1500, 2000,
                                                     3000]},
                   pre_dispatch='2*n_jobs', random_state=None, refit=True,
                   return_train_score=False, scoring=None, verbose=0)
```


***최적의 hyperparameter 조합 확인**

```python
print(random_search.best_params_)
```
```
{'max_iter': 3000, 'alpha': 0.1}
```

+) best 조합의 score를 확인

```python
random_search.best_score_
```
```
0.8425712188051439
```

+) score가 높은 Top 5 조합과 그 평균점수 확인
```python
scores_df = pd.DataFrame(random_search.cv_results_)
scores_df[scores_df['rank_test_score'] < 6][['params', 'mean_test_score', 'rank_test_score']]
```

<div class="code-example" markdown="1">

|    | params                             |   mean_test_score |   rank_test_score |
|---:|:-----------------------------------|------------------:|------------------:|
|  0 | {'max_iter': 1000, 'alpha': 0.001} |          0.820117 |                 5 |
|  1 | {'max_iter': 500, 'alpha': 0.01}   |          0.824949 |                 2 |
|  2 | {'max_iter': 3000, 'alpha': 0.1}   |          0.842571 |                 1 |
|  6 | {'max_iter': 1000, 'alpha': 0.001} |          0.820117 |                 5 |
|  7 | {'max_iter': 3000, 'alpha': 0.001} |          0.820963 |                 3 |
|  8 | {'max_iter': 3000, 'alpha': 0.001} |          0.820963 |                 3 |

</div>