---
layout: default
title: Function & Module
parent: Python 기초
nav_order: 4
---

# Function & Module
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

## Function (함수)
: 해당 함수에 저장된 특정 task를 수행해서 그 결과를 반환
- built-in functions: ex) print(), min() 등...
- user-created functions: create when you want to repeat a task <br/>
    (여러 줄의 코드를 반복하고 싶을 때 하나의 function으로 저장해두고 사용하면 효율적)


### 함수 만들기
: `def`를 사용해 함수 생성

*형식:
```python
def 함수명(매개변수):
    수행할 task
```

*예시:
```python
# 특정 문장의 단어 수를 구하는 함수. (공백 기준으로 나누어서 count)
def count_words(s):   ## 함수의 특성을 잘 드러내는 이름을 설정해주면 좋다
  words = s.split()
  return len(words)

z = 'today is a good day.'
count_words(z)  # 위에서 만든 함수를 사용
```
```
5
```

### global 변수 설정

1. 일반적으로, 함수 안의 변수는 함수 안에서만 효력을 갖는다

    ```python
    a = 1

    def add_one(a):
        a = a + 1

    add_one(a)
    print(a)  # 함수 안에서 a를 변경시켜도 global한 a는 변함이 없으므로 1이 출력됨
    ```
    ```
    1
    ```
    
    cf) `return`을 통해 변경된 a를 내보내면 밖에서 접근/출력 가능
    ```python
    a = 1

    def add_one(a):
        a = a + 1
        return a
    
    a = add_one(a) # 이렇게 해야 a가 2로 바뀐다. +1이 반영된 결과를 내보내서, a에 값으로 넣어주는 것.
    print(a) # 2가 출력됨 
    ```
    ```
    2
    ```

1. `global` 명령어를 사용하면 함수 밖 변수에 효력을 미칠 수 있다

    ```python
    a = 1

    def add_one():
    global a    # 함수 밖의 a (=global한 a) 
    a = a + 1

    add_one()  # global한 a에 1을 더한 것이기에, 실제 함수 밖의 a가 2로 변함
    print(a)  # 2가 출력됨 
    ```
    ```
    2
    ```

※ 하지만 global 명령어는 가급적 사용하지 않는 것이 좋다. (함수는 독립적으로 존재하는 것이 좋기 때문)  
&nbsp;&nbsp;&nbsp;&nbsp; return을 사용해서 값을 밖에서 받을 방법을 만들어 주는 방법이 가장 좋음!
<br/><br/>


### lambda: 함수의 간결한 버전
: 보통 함수를 한 줄로 간결하게 표현하고 싶을 때 def 대신 lambda를 사용한다
- def를 사용할 정도로 복잡하지 않거나, def를 사용할 수 없을 때 주로 사용

*형식:
```python
lambda 매개변수: 표현식
```

*예시:
```python
sq = lambda a: a**2
print(sq(2))  # 2**2의 결과인 4가 출력됨
```
```
4
```
+) 매개변수를 2개 이상 넣어도 된다
```python
add = lambda a, b: a + b
print(add(1, 2))  # 1 + 2의 결과인 3이 출력됨
```
```
3
```

### lambda와 map(), filter(), reduce()
(lambda의 가치를 극대화해주는 함수들)

1. map(함수, 리스트): 리스트에서 원소를 하나씩 꺼내 함수를 적용하고, 그 결과를 새로운 리스트로 반환
```python
# assign된 리스트의 각 원소의 제곱을 원소들로 하는 새로운 리스트를 반환
list(map(lambda x: x**2, [0, 1, 2, 3, 4]))  
```
```
[0, 1, 4, 9, 16]
```

1. filter(함수, 리스트): 리스트의 각 원소들에 함수를 적용해, 결과가 True인 값들만 넣은 새로운 리스트를 반환
```python
# assign된 리스트의 각 원소 중 2로 나누어 떨어지는 값, 즉 짝수만을 원소로 하는 새로운 리스트를 생성
list(filter(lambda x: x % 2 == 0, [0, 1, 2, 3, 4]))
```
```
[0, 2, 4]
```
```python
## 다른 예시: 3보다 작은 수만 가지고 리스트 생성
list(filter(lambda x: x < 3, [0, 1, 2, 3, 4])) 
```
```
[0, 1, 2]
```

1. reduce(함수, 순서형 자료): 순서형 자료(list/string/tuple)의 원소들을 함수에 누적 적용해, 최종 결과를 반환
- map(), filter()와 달리, 기본 내장 함수가 아니라서 import해주어야 사용 가능.

    ```python
    from functools import reduce   ## reduce는 내장 함수가 아니라 import해야 함

    reduce(lambda x, y: x + y, [0, 1, 2, 3, 4] 
    # 1) 0+1의 결과인 1이 x가 되고, 다음 원소인 2가 y가 되어 1+2=3
    # 2) 1+2의 결과인 3이 x가 되고, 다음 원소인 3이 y가 되어 3+3=6
    # 3) 3+3의 결과인 6이 x가 되고, 다음 원소인 4가 y가 되어 6+4=10
    ## 결국, <모든 원소를 더한 결과>인 10이 반환됨.
    ```
    ```
    10
    ```



## Module
: a file containing codes
- 특정 module은 특정 목적에 맞는 function, class 등을 담고 있다
    - ex) 'math' 모듈은 math에 필요한 기능들을, 'matplotlib' 모듈은 visualization에 필요한 기능들을 포함
- `import`를 통해 모듈을 가져와서 사용한다
- 이미 존재하는 모듈을 사용하는 것 외에, 직접 모듈을 만들어서 사용할 수도 있다
    - .py 파일에 원하는 코드들을 저장해두고, 다른 .py / .ipynb 파일에서 import해주면 된다
    - ※ import 하려는 모듈이 같은 디렉토리에 있어야 한다
    - ※ import filename.py와 같이 파일명을 그대로 쓰는 것이 아니라, import filename이라고만 써야 한다


```python
import math  # 'import'를 통해 모듈을 가져옴

dir(math)  ## dir()를 통해 모듈에 포함된 함수 등 목록을 확인 가능
```
```
['__doc__', '__loader__', '__name__', '__package__', '__spec__', 'acos', 'acosh', 'asin', 'asinh',
'atan', 'atan2', 'atanh', 'ceil', 'copysign', 'cos', 'cosh', 'degrees', 'e', 'erf', 'erfc', 'exp', 
'expm1', 'fabs', 'factorial', 'floor', 'fmod', 'frexp', 'fsum', 'gamma', 'gcd', 'hypot', 'inf', 
'isclose', 'isfinite', 'isinf', 'isnan', 'ldexp', 'lgamma', 'log', 'log10', 'log1p', 'log2', 'modf', 
'nan', 'pi', 'pow', 'radians', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'tau', 'trunc']
```

### Module 중 일부 함수만 import
```python
from math import sqrt

math.sqrt(4)   # sqrt: square route. 루트 씌운 값 출력
```
```
2.0
```
+) 여러 개를 가져오고 싶으면 ,로 구분
```python
from math import sqrt, pow, trunc
```

### Module 이름을 다르게 import
```python
import math as ma

ma.sqrt(4)
```
```
2.0
```
- `import pandas as pd`, `import numpy as np` 등 관행적으로 줄여서 사용하는 이름이 존재하는 모듈도 많음
