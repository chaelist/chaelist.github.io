---
layout: default
title: 기초 & Linear Regression
parent: Machine Learning 기초
nav_order: 1
--- 

# 기초 & Linear Regression
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

## Machine Learning이란?
: 데이터셋으로 모델을 학습시켜 이를 바탕으로 새로운 데이터셋을 예측할 수 있게 하는 방식. 
- 인공지능(AI)의 한 분야이며, 명시적으로 프로그래밍하지 않아도 기계 스스로의 학습을 통한 예측이 가능.

### 관련 용어들
1. **종속변수와 독립변수**
- `y = f(x)`에서의 y가 종속변수, x가 독립변수.
- Independent variable (독립변수): ML에서 학습의 단서가 되는 변수들. (ex. 귀의 모양, 털의 색, 수염의 길이)
  - feature 또는 입력변수라고도 함.
- Dependent variable (종속변수): ML에서 예측해야 하는 정답 (ex. 개 / 고양이)
  - 목표변수라고도 함.
- IV와 DV 간의 관계는 '파라미터'의 값에 따라 달라진다
  - y = a + bx에서의 a, b처럼 모델의 모양을 결정짓는 값들을 Parameter(파라미터)라고 한다


    &nbsp;
    {: .fs-1 .lh-0}

1. **연속변수와 이산변수**
- Continuous variable (연속변수): 무한한 값을 취할 수 있는 변수 (ex. 체중, 수입)
- Discrete variable (이산변수): 취할 수 있는 값이 유한한 변수. (ex. 성별, 자녀 수)
  - Categorical varialbe (범주형변수): 취하는 값이 숫자의 크기가 아닌 그룹의 이름을 지칭하는 변수. (ex. 성별)


### Maching Learning의 종류

1. **Supervised Learning (지도 학습)**
  - 정답(=DV)이 있는 training data로 학습 → 새로운 데이터(test data)에 적용해서 정답을 예측한다.
  - Machine Learning의 대부분은 Supervised Learning.

    *예시: 
    - Regression problems (회귀): DV가 연속 변수인 경우. (ex. 아파트 가격 예측, 연봉 예측)
    - Classifiaction problems (분류): DV가 범주형 변수인 경우. (ex. 고양이인지 강아지인지 분류)

    &nbsp;
    {: .fs-1 .lh-0}

1. **Unsupervised Learning (비지도 학습)**
  - training data가 필요 없음 (=정답이 따로 없음). 비교를 통해 비슷한 data point끼리 묶어준다.
  - 비지도 학습은 대부분 탐색적 데이터 분석의 일부로 수행된다. 

    *예시:  
    - Clustering (군집화): 거리를 계산해 유사한 data point끼리 같은 Cluster로 묶어준다.
    - Dimension reduction (차원 축소): 많은 feature를 갖고 있는 다차원 데이터셋의 차원을 축소해 새로운 데이터셋을 생성해준다.

    &nbsp;
    {: .fs-1 .lh-0}

1. **Reinforcement learning (강화학습)**
  - action & feedback 프로세스를 통해 문제를 풀어나가는 방식. 
  - 현재 상태에서 높은 reward를 받는 방법을 찾아가며 action을 취하는 것을 반복하다보면 높은 reward를 받을 수 있는 전략을 터득하게 됨


+. **Deep Learning**:    <br/>
&nbsp;&nbsp; 머신러닝 중 인공신경망 모델을 집중 연구하는 분야로, 지도학습, 비지도학습, 강화학습과 모두 연결이 가능하다

<br/>

## Linear Regression (선형회귀)

### 기본 개념 

- **가설 함수: 일차 함수** (학습 = 데이터에 가장 잘 맞는 일차 함수를 찾는 것!)
  - feature가 하나인 경우: h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub>
  - feature가 여러 개인 경우: h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + ... + θ<sub>n</sub>x<sub>n</sub>
    - feature가 여러 개인 경우도 항이 많긴 하지만 일차함수.

- **손실함수: MSE(평균제곱오차)**
  - 손실 함수(loss function): 가설 함수를 평가하기 위한 함수. 손실 함수의 결과값이 작을수록 손실이 작은 것. = 좋은 가설 함수.
  - MSE: 각 데이터포인트들이 가설 함수로부터 얼마나 떨어져 있는지, 그 오차를 모두 제곱해서 합하고 평균을 낸 것.
  - 손실 함수의 최저점, 즉 MSE가 가장 최소가 되는 지점을 찾으면 데이터에 가장 잘 맞는 가설함수를 찾을 수 있다.

- **손실함수의 최저점을 찾는 방법**:
  - Normal Equation: derivative(미분값)이 0이 되는 지점을 직접 바로 계산해서 찾는다
  - Gradient Descent (경사하강법): 특정 지점에서 시작, 점점 값을 update해가면서 최저점을 찾는다

※ sklearn의 Linear Regression 모델은 `scipy.linalg.lstsq`를 사용해서 최저점을 계산.   
(Normal Equation 방식)



### scikit-learn으로 구현하기
※ Anaconda를 사용하고 있다면 scikit-learn은 이미 내장되어 있음.   
그 외 경우에는 `pip install scikit-learn`로 설치.

- sklearn에서 기본 제공하는 학습용 dataset 중 'boston 집값 데이터'를 활용
- IV이 2개 이상인 'Multiple Linear Regression (다중선형회귀)' 문제. 

```python
from sklearn.datasets import load_boston  # boston 집값 데이터 불러오기
from sklearn.model_selection import train_test_split  # 데이터셋을 training set / test set 나누기 위한 함수
from sklearn.linear_model import LinearRegression # 선형회귀를 구현해주는 함수
from sklearn.metrics import mean_squared_error  # mean squared error(평균제곱오차)를 구해주는 함수

import pandas as pd
```


**1. 데이터 준비**

```python
boston_dataset = load_boston()  # 데이터셋을 가져와준다
```

+) `print(boston_dataset.DESCR)`를 해주면 데이터셋에 대한 정보를 살펴볼 수 있음
- Number of Instances: 506 (506개의 집값 데이터가 들어있음)
- Number of Attributes: 13  (입력변수 13개)
- 14번째 속성이 대체로 목표변수인 '집값'

&nbsp;
{: .fs-1 .lh-0}

**→ 데이터 정리**

```python
# boston dataset의 입력변수들을 dataframe으로 정리
X = pd.DataFrame(boston_dataset.data, columns=boston_dataset.feature_names)

# 목표변수도 dataframe으로 정리
y = pd.DataFrame(boston_dataset.target, columns=['MEDV'])
```

&nbsp;
{: .fs-1 .lh-0}

```python
X.head()
```

<div class="code-example" markdown="1">

|    |    CRIM |   ZN |   INDUS |   CHAS |   NOX |    RM |   AGE |    DIS |   RAD |   TAX |   PTRATIO |      B |   LSTAT |
|---:|--------:|-----:|--------:|-------:|------:|------:|------:|-------:|------:|------:|----------:|-------:|--------:|
|  0 | 0.00632 |   18 |    2.31 |      0 | 0.538 | 6.575 |  65.2 | 4.09   |     1 |   296 |      15.3 | 396.9  |    4.98 |
|  1 | 0.02731 |    0 |    7.07 |      0 | 0.469 | 6.421 |  78.9 | 4.9671 |     2 |   242 |      17.8 | 396.9  |    9.14 |
|  2 | 0.02729 |    0 |    7.07 |      0 | 0.469 | 7.185 |  61.1 | 4.9671 |     2 |   242 |      17.8 | 392.83 |    4.03 |
|  3 | 0.03237 |    0 |    2.18 |      0 | 0.458 | 6.998 |  45.8 | 6.0622 |     3 |   222 |      18.7 | 394.63 |    2.94 |
|  4 | 0.06905 |    0 |    2.18 |      0 | 0.458 | 7.147 |  54.2 | 6.0622 |     3 |   222 |      18.7 | 396.9  |    5.33 |

</div>

&nbsp;
{: .fs-1 .lh-0}

```python
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



**2. train_test_split**

```python
# training set / test set 분리
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=5)
```

```python
# 약 4:1로 잘 나뉘었음을 확인할 수 있음
print(X_train.shape)
print(X_test.shape)
print(y_train.shape)
print(y_test.shape)
```
```
(404, 13)
(102, 13)
(404, 1)
(102, 1)
```


**3. 모델 학습시키기**

```python
model = LinearRegression()
model.fit(X_train, y_train)   # 학습
```
```
LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)
```

*theta값 확인 (변수별 가중치를 확인할 수 있다)
```python
# theta_1부터의 theta값들. (각 변수별 가중치)
model.coef_
```
```
array([[ -0.13,   0.05,   0.  ,   2.71, -15.96,   3.41,   0.  ,  -1.49,
          0.36,  -0.01,  -0.95,   0.01,  -0.59]])
```

```python
# theta_0. (절편)
model.intercept_
```
```
array([37.91248701])
```


**4. test data로 성능 체크**

1. 평균제곱근오차 확인

    ```python
    # X_test로 예측
    y_test_prediction = model.predict(X_test)

    ## 평균제곱근오차 (RMSE: Root Mean Square Error)
    mean_squared_error(y_test, y_test_prediction) ** 0.5
    ```
    ```
    4.568292042303217
    ```

1.  R 스퀘어 값 확인
- `model.score()`: R square (R<sup>2</sup>) 값을 계산해주는 함수
- R square: 0 ~ 1 사이의 값을 가지며, 1에 가까울수록 모델의 성능이 좋은 것. (설명력이 좋은 것)

    ```python
    print(model.score(X_train, y_train))
    print(model.score(X_test, y_test))
    ```
    ```
    0.738339392059052
    0.7334492147453064
    ```
    training data는 약 74%, test data는 약 73%를 잘 예측한다는 뜻

<br/>

## Polynomial Regression (다항회귀)
- IV와 DV 간의 관계를 가장 잘 표현하는 게 일차함수가 아니라 다항식(곡선 모양)인 경우, 다항 회귀를 사용

### 기본 개념
- **가설 함수: 다항식** (원하는 모양에 따라 2차 함수 ~ n차 함수)
   - ex) feature가 하나이고, 가설함수가 2차 함수: h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sup>2</sup>
   - ex) feature가 하나이고, 가설함수가 3차 함수: h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sup>2</sup> + + θ<sub>3</sub>x<sup>3</sup>
   - ex) feature가 2개이고, 가설함수가 2차 함수: h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + θ<sub>3</sub>x<sub>1</sub>x<sub>2</sub> + θ<sub>4</sub>x<sub>1</sub><sup>2</sup> + θ<sub>5</sub>x<sub>2</sub><sup>2</sup>
- **선형회귀와 동일한 방식으로 문제를 해결**
    - ex) h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sup>2</sup>: 입력변수가 2개인 선형회귀로 취급
    - ex) h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x<sub>1</sub> + θ<sub>2</sub>x<sub>2</sub> + θ<sub>3</sub>x<sub>1</sub>x<sub>2</sub> + θ<sub>4</sub>x<sub>1</sub><sup>2</sup> + θ<sub>5</sub>x<sub>2</sub><sup>2</sup>: 입력변수가 5개인 선형회귀로 취급
- 다항회귀의 장점: **속성 간에 있을 수 있는 복잡한 관계들이 학습에 반영된다**
    - ex) 집 값 예측 문제에서 IV가 '너비', '높이' 2개일 때, 다항회귀를 사용하면 '너비x높이'라는 새로운 변수가 입력변수로 들어가는 셈이기에, 예측력이 커짐  
    (단순선형회귀의 경우 두 변수가 독립적이라, '높이와 너비가 모두 커야 집 값이 높다'는 관계는 학습 불가)
- 다항회귀의 단점: 너무 차수를 높여서 training data에 꼭 맞는 가설 함수를 만드는 경우, training data는 잘 설명하지만 새로운 데이터는 잘 설명하지 못하는 **overfitting(과적합) 문제가 발생할 수 있다**


### scikit-learn으로 구현하기
- sklearn에서 기본 제공하는 학습용 dataset 중 'boston 집값 데이터'를 활용해 2차 함수를 만들어줄 예정.
- IV이 2개 이상인 '다중 다항 회귀' 문제. 
- LinearRegression 모델을 사용하되, 다항 속성을 만들어서 IV로 넣어준다


```python
from sklearn.datasets import load_boston
from sklearn.preprocessing import PolynomialFeatures # 다항속성을 만들어주는 툴
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression  # 똑같이 LinearRegression 모델을 사용
from sklearn.metrics import mean_squared_error

import pandas as pd
```


**1. 다항 속성 만들어주기**

```python
# boston dataset 가져옴
boston_dataset = load_boston()
```

```python
polynomial_transformer = PolynomialFeatures(2)   # () 안에 몇차 함수로 할 건지 써줌 - 여기선 2차함수로 지정.
polynomial_data = polynomial_transformer.fit_transform(boston_dataset.data)

polynomial_data.shape  # 열이 13개에서 105개로 늘어났음을 확인 가능.
```
```
(506, 105)
```

+) 어떤 feature들이 105개 안에 들어갔나 확인
```python
polynomial_feature_names = polynomial_transformer.get_feature_names(boston_dataset.feature_names)

print(polynomial_feature_names) # 가능한 모든 2차 조합이 다 들어있음을 확인 가능
```
```
['1', 'CRIM', 'ZN', 'INDUS', 'CHAS', 'NOX', 'RM', 'AGE', 'DIS', 'RAD', 'TAX', 'PTRATIO', 'B', 'LSTAT', 'CRIM^2', 'CRIM ZN', 'CRIM INDUS', 'CRIM CHAS', 'CRIM NOX', 'CRIM RM', 'CRIM AGE', 'CRIM DIS', 'CRIM RAD', 'CRIM TAX', 'CRIM PTRATIO', 'CRIM B', 'CRIM LSTAT', 'ZN^2', 'ZN INDUS', 'ZN CHAS', 'ZN NOX', 'ZN RM', 'ZN AGE', 'ZN DIS', 'ZN RAD', 'ZN TAX', 'ZN PTRATIO', 'ZN B', 'ZN LSTAT', 'INDUS^2', 'INDUS CHAS', 'INDUS NOX', 'INDUS RM', 'INDUS AGE', 'INDUS DIS', 'INDUS RAD', 'INDUS TAX', 'INDUS PTRATIO', 'INDUS B', 'INDUS LSTAT', 'CHAS^2', 'CHAS NOX', 'CHAS RM', 'CHAS AGE', 'CHAS DIS', 'CHAS RAD', 'CHAS TAX', 'CHAS PTRATIO', 'CHAS B', 'CHAS LSTAT', 'NOX^2', 'NOX RM', 'NOX AGE', 'NOX DIS', 'NOX RAD', 'NOX TAX', 'NOX PTRATIO', 'NOX B', 'NOX LSTAT', 'RM^2', 'RM AGE', 'RM DIS', 'RM RAD', 'RM TAX', 'RM PTRATIO', 'RM B', 'RM LSTAT', 'AGE^2', 'AGE DIS', 'AGE RAD', 'AGE TAX', 'AGE PTRATIO', 'AGE B', 'AGE LSTAT', 'DIS^2', 'DIS RAD', 'DIS TAX', 'DIS PTRATIO', 'DIS B', 'DIS LSTAT', 'RAD^2', 'RAD TAX', 'RAD PTRATIO', 'RAD B', 'RAD LSTAT', 'TAX^2', 'TAX PTRATIO', 'TAX B', 'TAX LSTAT', 'PTRATIO^2', 'PTRATIO B', 'PTRATIO LSTAT', 'B^2', 'B LSTAT', 'LSTAT^2']
```


**2. X, y값 정리**

```python
X = pd.DataFrame(polynomial_data, columns=polynomial_feature_names)
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


**3. train_test_split & 학습**

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=5)
```

```python
model = LinearRegression()
model.fit(X_train, y_train)  #학습
```
```
LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)
```

*theta값 확인

```python
model.coef_  # 변수별 가중치
```
```
array([[ -0.  ,  -5.09,  -0.17,  -5.97,  24.32, 165.18,  21.99,   1.03,
         -5.67,   3.22,  -0.01,   5.35,  -0.05,   0.75,   0.  ,   0.27,
          0.59,   2.42,  -0.03,   0.09,  -0.01,  -0.06,   0.36,  -0.04,
          0.54,  -0.  ,   0.02,  -0.  ,  -0.01,  -0.11,  -1.28,   0.03,
          0.  ,  -0.01,  -0.  ,   0.  ,  -0.01,   0.  ,  -0.  ,   0.03,
         -0.21,   1.3 ,   0.31,   0.  ,   0.08,  -0.01,   0.  ,  -0.01,
          0.01,  -0.01,  24.32, -18.48,  -6.89,   0.04,   3.05,  -0.41,
          0.02,  -0.85,   0.03,  -0.47, -46.79,   3.65,  -0.6 ,  15.93,
         -0.99,   0.13, -11.92,  -0.04,   1.5 ,   0.16,  -0.06,  -0.02,
         -0.15,  -0.01,  -0.54,  -0.  ,  -0.22,   0.  ,  -0.01,   0.02,
         -0.  ,   0.  ,  -0.  ,  -0.01,   0.51,  -0.1 ,  -0.01,  -0.19,
         -0.01,   0.1 ,  -0.14,   0.01,  -0.12,  -0.  ,  -0.05,  -0.  ,
          0.01,  -0.  ,  -0.  ,  -0.01,   0.01,  -0.  ,  -0.  ,  -0.  ,
          0.01]])
```

```python
model.intercept_   # 절편
```
```
array([-141.9])
```

**4. test data로 성능 체크**

1. 평균제곱근오차 확인
    ```python
    y_test_prediction = model.predict(X_test)

    ## 평균제곱근오차 (RMSE: Root Mean Square Error)
    mean_squared_error(y_test, y_test_prediction) ** 0.5
    ```
    ```
    3.1965276513308156
    ```

1. R 스퀘어 값 확인
    ```python
    print(model.score(X_train, y_train)) 
    print(model.score(X_test, y_test))
    ```
    ```
    0.9315569004651908
    0.8694943908787136
    ```
    training data는 약 93%, test data는 약 87%를 잘 예측한다는 뜻
