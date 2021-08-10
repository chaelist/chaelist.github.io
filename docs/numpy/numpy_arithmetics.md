---
layout: default
title: Numpy 연산과 통계
parent: Numpy
nav_order: 2
---

# Numpy 연산과 통계
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


## Numpy 기본 연산과 통계

### Numpy 기본 연산
: Numpy는 수학적 연산이 매우 간단하다는 점에서 list와 차별화된다

1. 각 원소에 동일한 사칙연산 적용:
    ```python
    import numpy as np   # import해야 사용 가능

    array1 = np.arange(10)
    print(array1)
    ```
    ```
    [0 1 2 3 4 5 6 7 8 9]
    ```
    # 각 원소에 2씩 더하기
    {: .fs-3  .text-grey-dk-000}
    ```python
    array1 + 2  
    ```
    ```
    array([ 2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
    ```

    # 각 원소를 2씩 나누기
    {: .fs-3  .text-grey-dk-000}
    ```python
    array1 / 2
    ```
    ```
    array([0. , 0.5, 1. , 1.5, 2. , 2.5, 3. , 3.5, 4. , 4.5])
    ```

    # 각 원소에 2씩 곱하기
    {: .fs-3  .text-grey-dk-000}    
    ```python
    array1 * 2 
    ```
    ```
    array([ 0,  2,  4,  6,  8, 10, 12, 14, 16, 18])
    ```

    # 각 원소 제곱하기
    {: .fs-3  .text-grey-dk-000} 
    ```python
    array1 ** 2
    ```
    ```
    array([ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81])
    ```
    
    cf) list로 동일한 사칙연산을 해주려면 for문을 사용해서 요소별로 연산을 해줘야 한다
    ```python
    # 아래와 같이 계산해야 array1 + 2와 같은 결과가 나온다
    list1 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    new_list = []

    for a in list1:
        new_list.append(a + 2)
    
    new_list
    ```
    ```
    [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
    ```

1. Numpy Array 간 사칙연산:
    ```python
    array1 = np.arange(10)
    array2 = np.arange(10, 20)

    print(array1)
    print(array2)
    ```
    ```
    [0 1 2 3 4 5 6 7 8 9]
    [10 11 12 13 14 15 16 17 18 19]
    ```

    # numpy array 간 덧셈
    {: .fs-3  .text-grey-dk-000}
    ```python
    array1 + array2   # 각 요소별로 차례대로 더해진다: 0+10, 1+11, 2+12, ... , 9+19 
    ```
    ```
    array([10, 12, 14, 16, 18, 20, 22, 24, 26, 28])
    ```

    # numpy array 간 나눗셈
    {: .fs-3  .text-grey-dk-000}
    ```python
    array1 / array2  # 각 요소별로 차례대로 나눠진다: 0/10, 1/11, 2/12, ... , 9/19 
    ```
    ```
    array([0.        , 0.09090909, 0.16666667, 0.23076923, 0.28571429, 0.33333333, 0.375     , 0.41176471, 0.44444444, 0.47368421])
    ```

    # numpy array 간 곱셈
    {: .fs-3  .text-grey-dk-000}
    ```python
    array1 * array2  # 각 요소별로 차례대로 곱해진다: 0*10, 1*11, 2*12, ... , 9*19 
    ```
    ```
    array([  0,  11,  24,  39,  56,  75,  96, 119, 144, 171])
    ```

    cf) list 두 개를 numpy array 간 덧셈과 동일하게 요소별로 더해주려면 for문을 사용해서 계산해야 한다
    ```python
    ## 아래와 같이 계산해야 array1 + array2와 같은 결과가 나온다
    list1 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    ist2 = [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
    added_list = []

    for a in list1:
        added_list.append(a + list2[list1.index(a)])

    added_list
    ```
    ```
    [10, 12, 14, 16, 18, 20, 22, 24, 26, 28]
    ```
    +) 단순히 list1 + list2 해버리면?
    ```python
    list1 + list2  # 아래와 같이 list1.extend(list2)의 결과가 나온다
    ```
    ```
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
    ```

### Numpy Boolean 연산
: 이 원리가 boolean indexing에 사용되는 것.

```python
array1 = np.array([2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31])

array1 > 4      # 각 원소별로 4보다 큰지 여부를 판단해 Boolean 값을 반환
                # 결과값도 array 형태. Boolean 값들을 element로 하는 numpy array
```
```
array([False, False,  True,  True,  True,  True,  True,  True,  True,  True,  True])
```

\>, <, >=, <=, == 등 사용 가능
{: .fs-3  .text-grey-dk-000}
```python
array1 % 2 == 0
```
```
array([ True, False, False, False, False, False, False, False, False,  False, False])
```

<div class="code-example" markdown="1">

\***np.where()** 함수: 해당 Boolean 조건을 만족하는 값들의 index값을 반환
- np.where( ) 안에는 boolean값들로 이루어진 numpy array가 들어간다
- 결과로 True인 element의 index값만 반환 (array형태로 반환)
{: .fs-3}

```python
np.where(array1 > 4)    # '4보다 크다'는 조건을 만족하는 값들의 index값 반환
```
```
(array([ 2,  3,  4,  5,  6,  7,  8,  9, 10]),)
```
</div>

\>> Boolean 연산을 활용한 Boolean Indexing: 2가지 방법
```python
# 1번째 방법: 직접 indexing 
print(array1[array1 > 4])

# 2번째 방법: np.where() 이용
filter = np.where(array1 > 4)
print(array1[filter])
```
```
[ 5  7 11 13 17 19 23 29 31]
[ 5  7 11 13 17 19 23 29 31]
```

### Numpy 기본 통계
1. 최댓값, 최솟값, 평균값
    ```python
    array1 = np.array([14, 6, 13, 21, 23, 31, 9, 5])

    print(array1.max()) # 최댓값
    print(array1.min()) # 최솟값
    print(array1.mean()) # 평균값
    ```
    ```
    31
    5
    15.25
    ```
    cf) list도 [max(x), min(x)](../../python_basics/numbers_list_string/#other-common-list-operators--functions) 기능은 있지만, 평균 구하기 등의 기능은 없음 

1. 중앙값
    ```python
    array1 = np.array([8, 12, 9, 15, 16])
    array2 = np.array([14, 6, 13, 21, 23, 31, 9, 5])

    print(np.median(array1)) # 중앙값
    print(np.median(array2)) # 중앙값 - 짝수 개의 값이 있는 array일 경우, 중앙값 두 개를 평균 낸 값을 return
                            ## array2의 경우, 중앙값이 13과 14 두 개. >> 두 개를 평균내면 13.5
    ```
    ```
    12.0
    13.5
    ```

1. 표준편차, 분산
    ```python
    array1 = np.array([14, 6, 13, 21, 23, 31, 9, 5])

    print(array1.std()) # 표준 편차
    print(array1.var()) # 분산
    ```
    ```
    8.496322733983215
    72.1875
    ```


### nan을 포함한 array 통계량 계산
- 하나라도 np.nan(=Null값)이 포함된 array는 sum, mean 등을 계산하면 nan으로밖에 안나온다:
    ```python
    array1 = np.array([14, 6, 13, 21, 23, np.nan, 9, 5])

    print(array1.sum())  # np.sum(array1)과 동일
    print(array1.mean())  # np.mean(array1)과 동일
    ```
    ```
    nan
    nan
    ```
- np.nansum(), np.nanmean()을 활용하면 nan을 제외한 통계량을 확인 가능:
    ```python
    print(np.nansum(array1))
    print(np.nanmean(array1))
    ```
    ```
    91.0
    13.0
    ```
- +) np.nanstd(), np.nanmin(), np.nanmedian() 등...


## Numpy를 활용한 행렬 연산

### 행렬 '요소별 곱하기'
- 요소별 곱하기(Element-wise Multiplication): 같은 행/열에 있는 요소끼리 곱해서 새로운 행렬을 만드는 연산
- 행렬 덧셈과 마찬가지로 같은 차원을 갖는 행렬 사이에만 연산이 가능하다
- **A ∘ B**라고 표기

```python
# numpy를 이용한 '요소별 곱하기' -- A * B와 같이 별표(*)를 사용하면 된다

A = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

B = np.array([
    [0, 1, 2],
    [2, 0, 1],
    [1, 2, 0]
])

A * B
```
```
array([[ 0,  2,  6],
       [ 8,  0,  6],
       [ 7, 16,  0]])
```


### 행렬 간 덧셈 & 곱셈
1. 행렬 간 덧셈
    ```python
    A = np.array([
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
    ])

    B = np.array([
        [0, 1, 2],
        [2, 0, 1],
        [1, 2, 0]
    ])

    A + B
    ```
    ```
    array([[ 1,  3,  5],
          [ 6,  5,  7],
          [ 8, 10,  9]])
    ```

1. 스칼라곱
    ```python
    5 * A
    ```
    ```
    array([[ 5, 10, 15],
          [20, 25, 30],
          [35, 40, 45]])
    ```

1. 행렬간 곱셉 (내적곱) 
- 전제: A의 열 수 = B의 행 수
- A * B는 '요소별 곱하기'가 되니까 주의

    *두 가지 방법:
    1. np.dot(A, B)
    ```python
    np.dot(A, B)
    ```
    ```
    array([[ 7,  7,  4],
          [16, 16, 13],
          [25, 25, 22]])
    ```
    1. A @ B
    ```python
    A @ B   ## np.dot이랑 동일한 결과를 낸다. 더 간결한 방법.
    ```
    ```
    array([[ 7,  7,  4],
          [16, 16, 13],
          [25, 25, 22]])
    ```

1. 연산 섞어서 계산하기
- 일반 연산과 마찬가지로, ()가 먼저 계산되고, 그 안에서도 덧셈보다 곱셈이 먼저 계산됨.

    ```python
    A @ B + (A + 2*B)  
    ```
    ```
    array([[ 8, 11, 11],
          [24, 21, 21],
          [34, 37, 31]])
    ```

### 전치 행렬, 단위 행렬, 역행렬

```python
A = np.array([
    [1, -1, 2],
    [3, 2, 2],
    [4, 1, 2]
])

A
```
```
array([[ 1, -1,  2],
       [ 3,  2,  2],
       [ 4,  1,  2]])
```

1. 전치 행렬
- 기존 행렬 A에서 행과 열을 바꾼 행렬
- **A<sup>T</sup>**라고 표기 (A의 전치 행렬)

    *두 가지 방법:
    1. np.transpose(A)
        ```python
        A_transpose = np.transpose(A)
        A_transpose
        ```
        ```
        array([[ 1,  3,  4],
              [-1,  2,  1],
              [ 2,  2,  2]])
        ```

    2. A.T
        ```python
        A_transpose = A.T  # .T만 붙여주면 됨. 더 간결한 방법.
        A_transpose
        ```
        ```
        array([[ 1,  3,  4],
              [-1,  2,  1],
              [ 2,  2,  2]])
        ```

1. 단위 행렬(identity matrix)
- 정사각형 모양이며, 대각선으로는 원소가 쭉 1이고, 그 외에는 모두 0인 행렬.
- 어떤 행렬이든 간에 단위 행렬을 곱하면 기존 행렬이 그대로 유지됨.
- 보통 **I**라고 표기

    *np.identity(숫자)로 생성
    ```python
    I = np.identity(3) # 3x3 단위행렬 
    I
    ```
    ```
    array([[1., 0., 0.],
          [0., 1., 0.],
          [0., 0., 1.]])
    ```

    *단위행렬 I를 A에 곱하면 그대로 A가 결과로 나옴
    ```python
    A @ I
    ```
    ```
    array([[ 1., -1.,  2.],
          [ 3.,  2.,  2.],
          [ 4.,  1.,  2.]])
    ```

1. 역행렬(inverse matrix)
-  특정 행렬 A에 곱했을 때 단위 행렬 I가 나오도록 하는 행렬
- **A<sup>-1</sup>**라고 표기 (A의 역행렬)
- 하지만 모든 행렬에 역행렬이 있는 것은 아니다! 무엇을 곱해든 I가 안나오는 행렬도 있음

    *numpy의 linalg 모듈의 pinv 함수를 사용
    - ※이 함수는 역행렬이 없는 경우에도 가장 비슷한 효과를 내는 행렬을 return해준다)
    - cf) `np.linalg.inv(A)`도 역행렬을 return해주는 함수이지만, 역행렬이 없는 경우에는 작동X

    ```python
    A_inverse = np.linalg.pinv(A)
    A_inverse
    ```
    ```
    array([[-0.2, -0.4,  0.6],
        [-0.2,  0.6, -0.4],
        [ 0.5,  0.5, -0.5]])
    ```

    *A와 A의 역행렬을 곱하면 I가 나온다
    ```python
    A @ A_inverse
    ```
    ```
    array([[ 1.00000000e+00,  7.77156117e-16, -8.88178420e-16],
        [ 0.00000000e+00,  1.00000000e+00, -8.88178420e-16],
        [ 0.00000000e+00,  4.44089210e-16,  1.00000000e+00]])
    ```
    - 대각선은 다 1이 맞는데, 다른 부분들에 나오는 숫자들은 0에 가깝긴 하지만(e-16은 0.00000~가 16개 있는 것) 완전히 0이라고 나오지 않는다. >> 이렇게 조금씩 이상한 이유는 무수히 많은 소수0은 컴퓨터가 다 표현하기 어렵기 때문. >> 소수0을 사용할 때는 여기 보이는 것처럼 약간의 오차가 발생할 수 밖에 없다.
    {: .fs-3  .text-grey-dk-000}

+) 행렬식 (determinant)
- 역행렬 존재 여부를 테스트하는 식. 행렬식이 0이면 역행렬이 존재하지 않음.
```python
np.linalg.det(A) 
```
```
-9.999999999999998
```
