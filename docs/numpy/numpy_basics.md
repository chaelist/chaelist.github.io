---
layout: default
title: Numpy 기초
parent: Numpy
nav_order: 1
---

# Numpy 기초
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


*Numpy: 대규모 다차원 배열을 쉽게 수학적으로 연산할 수 있게 지원하는 라이브러리
{: .text-purple-300}

## Numpy Array 생성

### list를 받아서 numpy array 생성
```python
import numpy as np    # import해야 사용 가능; 보통 np로 줄여서 import

list1 = [1,2,3]
print(type(list1))

array1 = np.array(list1)  ## 이렇게 하면 list가 numpy array 형태로 바뀜
    # 물론, array1 = np.array([1, 2, 3]) 이렇게 직접 assign하는 것도 가능
print(type(array1))
print(array1)
```
```
<class 'list'>
<class 'numpy.ndarray'>
[1 2 3]
```

+) 다차원 list로 행렬 생성하기
```python
A = np.array([[1,2,3],    # 다차원 list를 assign해줄 수 있다
              [2,3,4]])  
print('type:', type(A))
print('array 형태:', A.shape)   # (2, 3)의 행렬 형태
print(A)
```
```
type: <class 'numpy.ndarray'>
array 형태: (2, 3)
[[1 2 3]
 [2 3 4]]
```



### np.full(), np.zeros(), np.ones()

1. **np.full()**: 모든 값이 같은 numpy array 생성
```python
array1 = np.full(6, 7)  # 7이라는 값으로 채워진 6개짜리 array를 만들라는 뜻
print(array1)
```
```
[7 7 7 7 7 7]
```

1. **np.zeros()**: 모든 값이 0인 numpy array 생성
```python
array2 = np.zeros(6, dtype=int)   # dtype=int라고 쓰지 않으면 'float' 타입으로, 모든 값이 '0.'
        ## np.full(6, 0)이라고 해도 동일한 결과를 낼 수 있음
print(array2)
```
```
[0 0 0 0 0 0]
```
+) 0으로 채워진 (2, 4) 행렬 만들기
```python
C = np.zeros((2, 4))  # ()안에 ()를 또 넣어서 차원을 써줘야 함
C
```
```
array([[0., 0., 0., 0.],
       [0., 0., 0., 0.]])
```

1. **np.ones()**: 모든 값이 1인 numpy array 생성
```python
array3 = np.ones(6, dtype=int)
print(array3)
```
```
[1 1 1 1 1 1]
```

### random 숫자로 채워진 array 생성
: np.random.random(), np.random.randint(), np.random.rand(), np.random.randn()

1. **np.random.random()**: 0~1 사이의 랜덤한 floating number로 구성된 array 생성
- 원소 개수만을 input으로 받는다. 

    ```python
    array1 = np.random.random(6)   # 6개짜리 array를 만들되, 0~1 사이의 랜덤한 floating number로 채우라는 뜻 
    array2 = np.random.random(6)   ## 똑같은 코드로 두 번 생성해도 전혀 다른 숫자들로 구성된 array가 된다
        
    print(array1)
    print(array2)
    ```
    ```
    [0.88406249 0.39299664 0.92697636 0.85366744 0.07601589 0.33080729]
    [0.61871553 0.55877544 0.21509023 0.93186876 0.95658264 0.25799333]
    ```

1. **np.random.randint()**: 정수 형태의 랜덤 숫자로 구성된 array 생성
- np.random.random()과 달리 2개의 변수를 input으로 받는다

    ```python
    np.random.randint(5, size=8)  # 0에서 4까지의 정수로 랜덤하게 원소가 8개인 array 생성
    ```
    ```
    array([3, 4, 0, 3, 4, 4, 4, 3])
    ```

1. **np.random.rand()**: random한 값들을 넣은 '행렬' 만들기 (0~1 사이의 균일분포에서 random한 floating number로 구성)
- () 안에 차원을 적으면 됨

    ```python
    np.random.rand(3, 5)   # 3x5 행렬을 만들겠다는 뜻
    ```
    ```
    array([[0.20518818, 0.83743088, 0.58408188, 0.24389283, 0.28567188],
        [0.50262559, 0.50765122, 0.5385608 , 0.861137  , 0.69626678],
        [0.80102188, 0.44550328, 0.91234899, 0.91133805, 0.90298844]])
    ```

1. **np.random.randn()**: `rand()`와 마찬가지로 random한 값들을 넣은 '행렬' 만들되, 평균 0, 표준편차 1의 가우시안 표준정규분포에서 random한 floating number를 추출해 구성 
- () 안에 차원을 적으면 됨

    ```python
    np.random.randn(3, 5)   # 3x5 행렬
    ```
    ```
    array([[-0.13089314,  1.12283706,  0.50684862, -1.41899365,  0.07714081],
        [-0.20499738,  0.24543592,  0.19121621, -1.12409184, -0.8251468 ],
        [ 1.03761123, -0.99567926,  0.78983874,  1.2103671 , -0.84700707]])
    ```


### np.arange()
: 특정 숫자 범위만큼을 range로 하는 numpy array 생성
- range() 함수와 작동 방식이 거의 유사  [(참고)](../../python_basics/controlflow#range-function)

1. np.arange(a)
```python
array1 = np.arange(6)  # 0부터 5까지를 원소로 하는 array 생성
print(array1)
```
```
[0 1 2 3 4 5]
```

1. np.arange(a, b)
```python
array1 = np.arange(2, 7)  # 2부터 6까지를 원소로 하는 array 생성
print(array1)
```
```
[2 3 4 5 6]
```

1. np.arange(a, b, k)
```python
array1 = np.arange(3, 17, 3) # 3부터 16까지, 3의 간격으로 증가하는 원소들을 갖는 array 생성
print(array1)
```
```
[ 3  6  9 12 15]
```


## Numpy Array 변경

### Data Type 확인 및 변경

1. data type 확인하기
```python
array1 = np.array([1,2,3])
print(array1.dtype)  # 3개의 element가 모두 int이므로, dtype을 하면 int라고 나옴
```
```
int64
```

1. 데이터 유형 혼재된 리스트를 Array로 변경
- 서로 다른 데이터 유형이 섞여 있는 리스트를 array로 변경할 경우, 데이터 크기가 더 큰 데이터 타입으로 형 변환이 일괄 적용된다.

    ```python
    list2 = [1, 2, 'test']
    array2 = np.array(list2)
    print(array2)
    print(array2.dtype)  ## Unicode 문자열로 일괄 변환됨

    list3 = [1, 2, 3.0]
    array3 = np.array(list3)
    print(array3)
    print(array3.dtype)  ## float 타입으로 일괄 변환됨
    ```
    ```
    ['1' '2' 'test']
    <U21
    [1. 2. 3.]
    float64
    ```

1. **astype( )** : array 내 element들을 원하는 타입으로 변경
    ```python
    array_int = np.array([1,2,3])
    array_float = array_int.astype('float64')  # int -> float
    print(array_float, array_float.dtype)

    array_float1 = np.array([1.1, 2.1, 3.1])
    array_int1 = array_float1.astype('int32')  # float -> int
    print(array_int1, array_int1.dtype)
    ```
    ```
    [1. 2. 3.] float64
    [1 2 3] int32
    ```

### tolist()
: numpy array를 list로 바꾸기

```python
array1 = np.array([2, 3, 5, 7])
print(type(array1), array1)
list1 = array1.tolist()  # tolist() 기능을 사용하면 list로 타입이 바뀜
print(type(list1), list1)
```
```
<class 'numpy.ndarray'> [2 3 5 7]
<class 'list'> [2, 3, 5, 7]
```


### reshape()
: numpy array의 shape 변경.

1. reshape(m, n): m행 n열짜리 array로 변환하라는 뜻.
    ```python
    array1 = np.arange(10)
    print('array1:\n', array1)

    array2 = array1.reshape(2, 5)    # 2행 5열짜리 array로 변환
    print('array2:\n', array2)

    array3 = array1.reshape(5, 2)    # 5행 2열짜리 array로 변환
    print('array3:\n', array3)
    ```
    ```
    array1:
    [0 1 2 3 4 5 6 7 8 9]
    array2:
    [[0 1 2 3 4]
    [5 6 7 8 9]]
    array3:
    [[0 1]
    [2 3]
    [4 5]
    [6 7]
    [8 9]]
    ```

1. reshape(m, -1): m행, 그리고 열 수은 자동으로 맞춰서 변환하라는 뜻.
- ※ 인자로 **-1**를 적용하면, 고정된 행/열(숫자가 assign된 행/열)에 맞게 자동으로 shape 변환.
- 반대로, reshape(-1, n)이라고 하면 n열, 그리고 자동으로 맞춘 행 수로 변환
- reshape(1, -1): 원본 ndarray가 어떤 형태라도 (ex. 3차원이라도), 반드시 1개의 행을 갖는 2차원 array로 변환됨 

    ```python
    array1 = np.arange(10)
    print(array1)
    array2 = array1.reshape(-1, 5)          # 고정된 5개 열에 맞게, 행은 2개로 변환됨
    print('array2 shape:', array2.shape)
    array3 = array1.reshape(5, -1)          # 고정된 5개 행에 맞게, 열은 2개로 변환됨
    print('array3 shape:', array3.shape)
    ```
    ```
    [0 1 2 3 4 5 6 7 8 9]
    array2 shape: (2, 5)
    array3 shape: (5, 2)
    ```

1. 3차원 이상으로도 변경 가능
- ex) reshape((m, n, o))라고 하면 3차원 array로 변경 가능

    ```python
    array1 = np.arange(8)
    array3d = array1.reshape((2,2,2))  # 3차원
    print('array3d:\n', array3d)
    print('array3d: %d차원' %array3d.ndim)
    ```
    ```
    array3d:
    [[[0 1]
    [2 3]]

    [[4 5]
    [6 7]]]
    array3d: 3차원
    ```


### Numpy Array 정렬
: np.sort()와 ndarray.sort()

1. **np.sort()**: 원본 행렬을 바꾸지 않은 채, 순서대로 정렬된 새로운 행렬을 return

    ```python
    org_array = np.array([3,1,9,5])

    # np.sort( )로 정렬
    print(np.sort(org_array))  # sort된 새로운 array가 반환됨.
    print(org_array)   # 원래의 행렬은 변하지 않음.

    ## 행렬 자체가 변하는 것이 아니므로, sort_array1 = np.sort(org_array) 이렇게 저장해두고 쓰는 것이 유용하다.
    ```
    ```
    [1 3 5 9]
    [3 1 9 5]
    ```

1. **.sort()**: 원본 행렬 자체를 정렬해줌.
```python
# .sort( )로 정렬
print(org_array.sort())  # 결과로 아무것도 반환되지 않음. 그저 원래 행렬이 바뀐 것 뿐
print(org_array)    # .sort()를 하고 나니, 원래의 행렬이 변함
```
```
None
[1 3 5 9]
```

1. 내림차순으로 정렬하기 위해서는 `[::-1]`을 적용
    ```python
    sort_array1_desc = np.sort(org_array)[::-1]
    print(sort_array1_desc)

    org_array[::-1].sort()
    print(org_array)
    ```
    ```
    [9 5 3 1]
    [9 5 3 1]
    ```

1. 2차원 이상의 행렬: axis 축 값 설정을 통해 row 방향 / column 방향으로 각각 정렬 가능

    ```python
    array2d = np.array([[8,12],
                    [7,1]])

    sort_array2d_axis0 = np.sort(array2d, axis=0)   ## axis=0이면 row 방향
    print('row 방향 정렬:\n', sort_array2d_axis0)

    sort_array2d_axis1 = np.sort(array2d, axis=1)   ## axis=1이면 column 방향
    print('column 방향 정렬:\n', sort_array2d_axis1)
    ```
    ```
    row 방향 정렬:
    [[ 7  1]
    [ 8 12]]
    column 방향 정렬:
    [[ 8 12]
    [ 1  7]]
    ```

### np.argsort()
: 행렬을 직접 정렬하는 대신, 연결된 값의 순서대로 index를 정렬해준다.

```python
org_array = np.array([3,1,9,5])
sort_indices = np.argsort(org_array)  # 오름차순 정렬할 때의 인덱스 순서를 sort_indices에 저장
print(type(sort_indices))   # argsort의 결과물도 numpy array 타입.
print('정렬된 인덱스:', sort_indices)
print('원본 행렬:', org_array)  # 원본 행렬에는 아무런 영향이 없음
```
```
<class 'numpy.ndarray'>
정렬된 인덱스: [1 0 3 2]
원본 행렬: [3 1 9 5]
```

cf) 내림차순
```python
org_array = np.array([3,1,9,5])
sort_indices = np.argsort(org_array)[::-1]
print(sort_indices)
```
```
[2 3 0 1]
```

*활용 예시: argsort( )를 이용한 indexing을 활용해 성적순으로 이름 출력하기
```python
name_array = np.array(['John', 'Mike', 'Sarah', 'Kate', 'Samuel'])
score_array = np.array([78, 95, 94, 98, 88])

sort_score_indices = np.argsort(score_array)  # 성적이 낮은 순서대로 index 정렬
name_array[sort_score_indices]  # indexing을 통해 성적 낮은 순대로 이름 출력
```
```
array(['John', 'Samuel', 'Sarah', 'Mike', 'Kate'], dtype='<U6')
```


## Indexing & Slicing

### Indexing
1. 평범한 indexing
    ```python
    array1 = np.array([2, 3, 5, 7, 11])

    array1[2]  ## index 2, 즉 3번째 element가 반환됨
    ```
    ```
    5
    ```

1. indexing을 통한 array 내 값 수정
    ```python
    array1[0] = 9
    array1[2] = 0

    print(array1)
    ```
    ```
    [ 9  3  0  7 11]
    ```

1. list로 인덱싱
    ```python
    array1 = np.array([2, 3, 5, 7, 11])

    array1[[1, 3, 4]]  ## 1번, 3번, 4번 index의 값들이 추출됨
    ```
    ```
    array([ 3,  7, 11])
    ```

1. numpy array로 numpy array 인덱싱
    ```python
    array1 = np.array([2, 3, 5, 7, 11])

    array2 = np.array([2, 1, 3])
    array1[array2]
    ```
    ```
    array([5, 3, 7])
    ```

1. 2차원 array (행렬) 인덱싱
    ```python
    A = np.arange(1, 10).reshape(3, 3)   ## 1~10까지 원소로 갖는 array를 만들고, 3x3 행렬로 reshape

    print(A)

    print(A[0, 2])   # 0행(첫번째), 2열(세번째)에 위치한 값인 3를 받아오는 것
    print(A[0][2])   # A[0, 2]와 동일한 결과
    ```
    ```
    [[1 2 3]
    [4 5 6]
    [7 8 9]]
    3
    3
    ```
+) 추가: 2차원 array에서 1차원으로만 indexing 하면 1차원 ndarray로 변환됨
    ```python
    print(A[0])   # 첫번째 row를 1차원 ndarray로 반환
    print(A[0].shape)   # 원소 3개짜리 1차원 array가 됨
    ```
    ```
    [1 2 3]
    (3,)
    ```


### Slicing
1. 기본 Slicing
    ```python
    array1 = np.array([2, 3, 5, 7, 9, 11, 13]) 

    print(array1[:3]
    print(array1[3:])
    print(array1[:])
    ```
    ```
    [2, 3, 5]
    [ 7,  9, 11, 13]
    [ 2,  3,  5,  7,  9, 11, 13]
    ```
+) 추가:
    ```python
    array1 = np.array([2, 3, 5, 7, 9, 11, 13])

    array1[2:6:2]  # 3번째 element (index2) 부터 7번째 element (index6) 까지, index를 2씩 건너뛰어서.
                    # 결과적으로 index 2, 4, 6에 해당하는 element들이 출력됨.
    ```
    ```
    [ 5,  9, 13]
    ```

1. 2차원 array (행렬) slicing
    ```python
    A = np.arange(1, 10).reshape(3, 3)

    print(A)

    print('\nA[0:2, 0:2] \n', A[0:2, 0:2])
    print('\nA[1:3, :] \n', A[1:3, :])
    print('\nA[:2, 1:] \n', A[:2, 1:])
    ```
    ```
    [[1 2 3]
    [4 5 6]
    [7 8 9]]

    A[0:2, 0:2] 
    [[1 2]
    [4 5]]

    A[1:3, :] 
    [[4 5 6]
    [7 8 9]]


    A[:2, 1:] 
    [[2 3]
    [5 6]]
    ```
+) 추가: row나 column 중 한 쪽은 slicing, 다른 쪽은 단일 값 indexing을 적용해도 됨
    ```python
    print(A[:2, 0])  # 1번쨰, 2번째 행의 1번째 element를 각각 추출
    print(A[:2, 0].shape)   # 원소 2개짜리 1차원 array가 됨
    ```
    ```
    [1 4]
    (2,)
    ```

### Boolean Indexing
: 조건 필터링과 검색을 동시에 할 수 있는 인덱싱 방식. 자주 이용된다.
```python
array1 = np.arange(1, 10)
print(array1)

print(array1[array1 > 5])   # 5보다 큰 숫자들만 indexing됨
```
```
[1 2 3 4 5 6 7 8 9]
[6 7 8 9]
```