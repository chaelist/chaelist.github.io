---
layout: default
title: Regularization
parent: Machine Learning 심화
nav_order: 1
--- 

# Regularization
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

## 편향과 분산
1. 편향(Bias)
- 모델이 너무 간단해서 데이터의 관계를 잘 학습하지 못하는 경우, 모델의 편향(bias)이 높다고 함
- 높은 차항의 회귀로 training 데이터에 거의 완벽히 맞춘 모델은 편향이 낮은 모델이라고 할 수 있음

1. 분산(Variance)
- 모델이 얼마나 일관된 성능을 보여주는지를 분산(variance)라고 함
- 다양한 데이터셋 간에 성능 차이가 많이 나면 분산이 높다고 하고, 성능 차이가 별로 없으면 분산이 낮다고 함
- 직선 모델은 어떤 데이터셋에 적용해도 성능이 비슷하게 나옴 (= 분산이 작음) vs 복잡한 곡선 모델은 데이터셋에 따라 성능의 편차가 큼 (= 분산이 큼)


### 과적합과 과소적합
1. 과소적합(Underfit)
- 편향이 높고 분산이 낮은 모델을 underfit되었다고 함
- 너무 단순한 모델이라 관계를 제대로 학습하지 못하는 경우. 
- 대신 모델이 간단하기에 어떤 데이터에 적용해도 일관된 성능을 보임

1. 과적합(Overfit)
- 편향이 낮고 분산이 높은 모델을 overfit되었다고 함
- training 데이터의 패턴을 학습하는 게 아니라 거의 데이터 자체를 외워버리는 수준으로 모델을 training 데이터에 거의 완벽히 맞추는 경우.
- training 데이터에 대한 성능은 아주 높지만, 처음 보는 test 데이터에 대한 성능은 많이 떨어진다


***편향-분산 트레이드오프(Bias-Variance Tradeoff)**   
\: 일반적으로, 편향과 분산은 하나가 줄어들면 다른 하나는 늘어나는 tradeoff 관계   
※ 과소적합과 과적합 사이의 적당한 밸런스를 찾아내는 게 중요하다!


### Overfitting 방지하기
- 독립변수 추가/제거: 관계가 있는 애들은 추가, 관계가 없는 애들은 제거
  - 보통, 학습데이터가 아주 많을수록 모델의 설명력이 좋아짐. 하지만 y와 상관없는 독립변수가 많이 들어있으면 새로운 data를 잘 설명하는 데 도움이 안됨
- 독립변수와 종속변수의 관계를 잘 파악 (비선형관계 등을 잘 고려해서 반영)
- parameter에 penalty 주기 = Regularization


## Overfitting 문제 체험
: 6차항의 복잡한 모델을 활용해 Overfitting 문제를 체험해본 후, 아래에서 같은 입력변수에 Regularization을 적용해 결과를 비교해볼 예정.

```python
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error
from sklearn.preprocessing import PolynomialFeatures
from sklearn.datasets import load_boston  # boston 집값 데이터

from math import sqrt

import numpy as np
import pandas as pd
```

### 6차항 모델 준비
```python
boston_dataset = load_boston()

X = pd.DataFrame(boston_dataset.data, columns=boston_dataset.feature_names)

# 입력변수를 6차항으로 바꾸기
polynomial_transformer = PolynomialFeatures(6)   # 6차항 변환기 준비
polynomial_features = polynomial_transformer.fit_transform(X.values)
features = polynomial_transformer.get_feature_names(X.columns)

# 바꾼 값들을 다시 X로 저장
X = pd.DataFrame(polynomial_features, columns=features)
X.head()
```

<div class="code-example" markdown="1">

|    |   1 |    CRIM |   ZN |   INDUS |   CHAS |   NOX |    RM |   AGE |    DIS |   RAD |   ...   |   B^4 LSTAT^2 |   B^3 LSTAT^3 |   B^2 LSTAT^4 |        B LSTAT^5 |    LSTAT^6 |
|---:|----:|--------:|-----:|--------:|-------:|------:|------:|------:|-------:|------:|---|--------------:|--------------:|--------------:|-----------------:|-----------:|
|  0 |   1 | 0.00632 |   18 |    2.31 |      0 | 0.538 | 6.575 |  65.2 | 4.09   |     1 |  ... |   6.15436e+11 |   7.72203e+09 |   9.68901e+07 |      1.2157e+06  |  15253.7   |
|  1 |   1 | 0.02731 |    0 |    7.07 |      0 | 0.469 | 6.421 |  78.9 | 4.9671 |     2 |  ... |   2.07308e+12 |   4.77399e+10 |   1.09938e+09 |      2.5317e+07  | 583012     |
|  2 |   1 | 0.02729 |    0 |    7.07 |      0 | 0.469 | 7.185 |  61.1 | 4.9671 |     2 |  ... |   3.86749e+11 |   3.96761e+09 |   4.07033e+07 | 417571           |   4283.81  |
|  3 |   1 | 0.03237 |    0 |    2.18 |      0 | 0.458 | 6.998 |  45.8 | 6.0622 |     3 |  ... |   2.09631e+11 |   1.56175e+09 |   1.16351e+07 |  86681.6         |    645.779 |
|  4 |   1 | 0.06905 |    0 |    2.18 |      0 | 0.458 | 7.147 |  54.2 | 6.0622 |     3 |  ... |   7.04983e+11 |   9.46727e+09 |   1.27137e+08 |      1.70733e+06 |  22927.8   |

5 rows × 27132 columns
{: .fs-2  .text-grey-dk-000}
</div>


&nbsp;
{: .fs-1 .lh-0}

```python
# 목표변수도 dataframe으로 정리
y = pd.DataFrame(boston_dataset.target, columns=['MEDV'])
y.head()
```

<div class="code-example" markdown="1">

|    |   MEDV |
|---:|-------:|
|  0 |   24   |
|  1 |   21.6 |
|  2 |   34.7 |
|  3 |   33.4 |
|  4 |   36.2 |

</div>


### 학습 & 결과 확인

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=5)

model = LinearRegression()
model.fit(X_train, y_train)
```
```
LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)
```


→ training data와 test data에서의 예측 성능 비교:

```python
rsquare = model.score(X_train, y_train)
print(f'training set에서의 r스퀘어: {rsquare :.3f}')

rsquare = model.score(X_test, y_test)
print(f'test set에서의 r스퀘어: {rsquare :.3f}')
```
```
training set에서의 r스퀘어: 1.000
test set에서의 r스퀘어: -28657.989
```
- training set에서는 100%의 설명력을 갖는 반면, test set은 하나도 설명을 하지 못한다. 과적합된 것!

```python
y_train_predict = model.predict(X_train)
y_test_predict = model.predict(X_test)

mse = mean_squared_error(y_train, y_train_predict)
print(f'training set에서의 rmse: {sqrt(mse) :.3f}')

mse = mean_squared_error(y_test, y_test_predict)
print(f'test set에서의 rmse: {sqrt(mse) :.3f}')
```
```
training set에서의 rmse: 0.000
test set에서의 rmse: 1650.789
```
- training set에서는 평균제곱근오차가 0인 반면, test set에서는 1650이 넘는 큰 값이 나온다


## Regularization (가중치 규제)
: 모델을 학습시킬 때 θ값(coefficient; 변수별 가중치)들이 너무 커지는 것을 방지해주는 방법   
(penalty를 부여하는 개념)
- 데이터에 대한 오차와 θ값 중 어느 걸 줄이는 것이 더 중요할지를 상수 λ(lambda)에 따라 결정   
(λ가 클수록 θ값에 대한 제한이 커짐)

### L1 Regularization
- L1: Lasso. 각 parameter의 절대값을 사용
- **손실 함수 J = 평균제곱오차 +  λ\|θ1\| + λ\|θ2\| + ... + λ\|θn\|**
- L1 Regularization을 사용하는 회귀 모델을 Lasso 모델이라고 함
- ※ L1 Regularization은 여러 θ값을 아예 0으로 만들어준다. (모델에 중요하지 않다고 생각되는 속성들을 아예 0으로 만들어 없애주는 효과)
    - L1 Regularization은 어떤 모델에 쓰이는 속성(변수)의 개수를 줄이고 싶을 때 사용된다.
    - ex) 속성 20개로 2차 다중 다항 회귀 모델을 만들면 속성은 총 230개가 됨 → 속성이 이렇게 많으면 과적합 가능성이 높을 뿐만이 아니라 모델을 학습시킬 때 많은 자원(RAM, 시간 등)을 소모됨 → L1 Regularization을 사용하면 학습에 사용되는 속성의 수를 많이 줄일 수 있다

### L2 Regularization
- L2: Ridge. 각 parameter의 제곱값을 사용
- **손실 함수 J = 평균제곱오차 + λθ1<sup>2</sup> + λθ2<sup>2</sup> + ... + λθn<sup>2</sup>**
- L2 Regularization을 사용하는 회귀 모델을 Ridge 모델이라고 함
- ※ L1과 달리 L2 Regularization은 θ값을 아예 0으로 만들지는 않고, 조금씩 가중치를 줄여주기만 한다.
    - 속성의 개수를 줄일 필요는 없다고 생각되면 L2 Regularization을 쓰면 됨

&nbsp;
{: .fs-1 .lh-0}

***L1, L2 Regularization 사용하기:**  
(Regularization은 손실함수를 최소화하는 모든 알고리즘에 적용할 수 있음)
- 다중회귀/다항회귀 모델: Linear Regression 대신 Lasso 또는 Ridge 모델을 사용하면 됨
- Logistic Regression 모델: L2 Regularization를 적용하는 것이 default. 
    - 'penalty'라는 옵셔널 파라미터를 통해 어떤 Regularization 기법을 사용할지 정해줄 수 있음
```python
LogisticRegression(penalty='none')  # Regularization 사용 안함
LogisticRegression(penalty='l1')  # L1 Regularization 사용
LogisticRegression(penalty='l2')  # L2 Regularization 사용
LogisticRegression()  # 위와 똑같음: L2 Regularization 사용
```

+) Deep Learning에서도 regularization이 중요.


### Lasso Regression 구현

```python
from sklearn.linear_model import Lasso  # 아예 Lasso Regression 모델이 따로 제공된다
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

from math import sqrt

import numpy as np
import pandas as pd
```

*위의 과적합 문제 체험에서 사용한 X, y 그대로 사용

```python
X.head()
```

<div class="code-example" markdown="1">

|    |   1 |    CRIM |   ZN |   INDUS |   CHAS |   NOX |    RM |   AGE |    DIS |   RAD |   ...   |   B^4 LSTAT^2 |   B^3 LSTAT^3 |   B^2 LSTAT^4 |        B LSTAT^5 |    LSTAT^6 |
|---:|----:|--------:|-----:|--------:|-------:|------:|------:|------:|-------:|------:|---|--------------:|--------------:|--------------:|-----------------:|-----------:|
|  0 |   1 | 0.00632 |   18 |    2.31 |      0 | 0.538 | 6.575 |  65.2 | 4.09   |     1 |  ... |   6.15436e+11 |   7.72203e+09 |   9.68901e+07 |      1.2157e+06  |  15253.7   |
|  1 |   1 | 0.02731 |    0 |    7.07 |      0 | 0.469 | 6.421 |  78.9 | 4.9671 |     2 |  ... |   2.07308e+12 |   4.77399e+10 |   1.09938e+09 |      2.5317e+07  | 583012     |
|  2 |   1 | 0.02729 |    0 |    7.07 |      0 | 0.469 | 7.185 |  61.1 | 4.9671 |     2 |  ... |   3.86749e+11 |   3.96761e+09 |   4.07033e+07 | 417571           |   4283.81  |
|  3 |   1 | 0.03237 |    0 |    2.18 |      0 | 0.458 | 6.998 |  45.8 | 6.0622 |     3 |  ... |   2.09631e+11 |   1.56175e+09 |   1.16351e+07 |  86681.6         |    645.779 |
|  4 |   1 | 0.06905 |    0 |    2.18 |      0 | 0.458 | 7.147 |  54.2 | 6.0622 |     3 |  ... |   7.04983e+11 |   9.46727e+09 |   1.27137e+08 |      1.70733e+06 |  22927.8   |

5 rows × 27132 columns
{: .fs-2  .text-grey-dk-000}
</div>

&nbsp;
{: .fs-1 .lh-0}

```python
y.head()
```

<div class="code-example" markdown="1">

|    |   MEDV |
|---:|-------:|
|  0 |   24   |
|  1 |   21.6 |
|  2 |   34.7 |
|  3 |   33.4 |
|  4 |   36.2 |

</div>


&nbsp;
{: .fs-1 .lh-0}


```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=5)

model = Lasso(alpha=0.001, max_iter=1000, normalize=True)   
model.fit(X_train, y_train)
```
```
Lasso(alpha=0.001, copy_X=True, fit_intercept=True, max_iter=1000,
      normalize=True, positive=False, precompute=False, random_state=None,
      selection='cyclic', tol=0.0001, warm_start=False)
```
- `alpha`가 λ(lambda)값을 결정해주는 파라미터
- Lasso 모델은 경사하강법을 사용 → `max_iter`로 경사하강법을 최대 몇 번 할 지 설정
- `normalize=True`로 설정하면 데이터를 0~1 사이의 값으로 normalize해준다
- +) Lasso 말고 Ridge 모델을 쓰고 싶다면, Lasso 대신 Ridge를 import해서 사용해주면 됨

&nbsp;
{: .fs-1 .lh-0}

→ training data와 test data에서의 예측 성능 비교:
```python
rsquare = model.score(X_train, y_train)
print(f'training set에서의 r스퀘어: {rsquare :.3f}')

rsquare = model.score(X_test, y_test)
print(f'test set에서의 r스퀘어: {rsquare :.3f}')
```
```
training set에서의 r스퀘어: 0.945
test set에서의 r스퀘어: 0.902
```

```python
y_train_predict = model.predict(X_train)
y_test_predict = model.predict(X_test)

mse = mean_squared_error(y_train, y_train_predict)
print(f'training set에서의 rmse: {sqrt(mse) :.3f}')

mse = mean_squared_error(y_test, y_test_predict)
print(f'test set에서의 rmse: {sqrt(mse) :.3f}')
```
```
training set에서의 rmse: 2.099
test set에서의 rmse: 3.058
```
- Regularization으로 overfitting을 방지해주니, training set과 test set에서 r스퀘어값과 평균제곱근오차가 비슷하게 맞춰짐!