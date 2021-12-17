---
layout: default
title: 유용한 Python 내장함수
parent: Data Handling
nav_order: 4
---

# 유용한 Python 내장함수
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

## map, zip

1. **map**(function, iterable): input으로 받은 함수를 input으로 받은 iterable 내 element에 모두 적용해주는 함수

    ```python
    print(list(map(int, [1.4, 2.3, 3.5, 4.0])))  # 각 element를 꺼내서 int(x)에 넣어준 후 다시 list에 넣는 느낌
    ```
    ```
    [1, 2, 3, 4]
    ```

    +) function 부분에 기존 함수가 아닌 lambda 식을 넣는 것도 가능
    ```python
    print(list(map(lambda x: x**2, [1, 2, 3, 4, 5])))
    ```
    ```
    [1, 4, 9, 16, 25]
    ```

1. **zip**(iterables): 각 iterable의 element들을 연결해주는 함수

    ```python
    # 보통은 이런 식으로 반복문에서 index를 붙여줄 때 많이 활용
    list1 = ['A', 'B', 'C', 'D', 'E']
    for i, element in zip(range(len(list1)), list1):
        print(i, ':', element)
    ```
    ```
    0 : A
    1 : B
    2 : C
    3 : D
    4 : E
    ```

    +) zip(*zipped_element)를 해주면 unzip할 수 있음 
    ```python
    print(list(zip(*zip(range(len(list1)), list1))))
    ```
    ```
    [(0, 1, 2, 3, 4), ('A', 'B', 'C', 'D', 'E')]
    ```

## Itertools
: 효율적인 루핑을 위한 이터레이터를 만드는 함수들을 제공하는 모듈

### 조합형 iterator

1. **product**(iterables, repeat=1): input으로 받은 iterable들의 데카르트곱을 반환
- iterable: 요소를 하나씩 반환할 수 있는 객체. sequence type인 list, str, tuple 등.
- 데카르트곱 (= 곱집합): 각 집합의 원소를 각 성분으로 하는 tuple의 집합

    ```python
    from itertools import product

    print(list(product('ABC', 'xyz')))
    ```
    ```
    [('A', 'x'), ('A', 'y'), ('A', 'z'), ('B', 'x'), ('B', 'y'), ('B', 'z'), ('C', 'x'), ('C', 'y'), ('C', 'z')]
    ```

    +) iterable이 list로 묶여 있는 경우, *로 list를 해제하고 넣어주면 된다

    ```python
    list1 = ['ABC', 'xyz']
    print(list(product(*list1)))
    ```
    ```
    [('A', 'x'), ('A', 'y'), ('A', 'z'), ('B', 'x'), ('B', 'y'), ('B', 'z'), ('C', 'x'), ('C', 'y'), ('C', 'z')]
    ```

    +) repeat을 설정하면 iterable이 n번 반복해서 존재하는 것으로 간주

    ```python
    print(list(product('01', repeat=2)))  # product('01', '01)과 동일한 개념
    ```
    ```
    [('0', '0'), ('0', '1'), ('1', '0'), ('1', '1')]
    ```

    ```python
    print(list(product('A', repeat=4)))
    ```
    ```
    [('A', 'A', 'A', 'A')]
    ```

    ```python
    print(list(product('01', 'ab', repeat=2)))
    ```
    ```
    [('0', 'a', '0', 'a'), ('0', 'a', '0', 'b'), ('0', 'a', '1', 'a'), ('0', 'a', '1', 'b'), ('0', 'b', '0', 'a'), ('0', 'b', '0', 'b'), ('0', 'b', '1', 'a'), ('0', 'b', '1', 'b'), ('1', 'a', '0', 'a'), ('1', 'a', '0', 'b'), ('1', 'a', '1', 'a'), ('1', 'a', '1', 'b'), ('1', 'b', '0', 'a'), ('1', 'b', '0', 'b'), ('1', 'b', '1', 'a'), ('1', 'b', '1', 'b')]
    ```


1. **combinations**(iterable, r): iterable에서 원소 개수가 r개인 '조합' 뽑기
- 조합: 순서를 생각하지 않고 뽑는 것
- iterable이 n개의 원소를 갖는다면, <sub>n</sub>C<sub>r</sub>개의 조합이 가능

    ```python
    from itertools import combinations

    print(list(combinations([1, 2, 3, 4], 2)))   # 4C2 = 6가지 경우의 수
    ```
    ```
    [(1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]
    ```

1. **combinations_with_replacement**(iterable, r): iterable에서 원소 개수가 r개인 중복 조합 뽑기
- iterable이 n개의 원소를 갖는다면, <sub>n</sub>H<sub>r</sub>개의 조합이 가능
- <sub>n</sub>H<sub>r</sub> = <sub>n+r-1</sub>C<sub>r</sub>

    ```python
    from itertools import combinations_with_replacement

    print(list(combinations_with_replacement([1, 2, 3, 4], 2)))  # 4H2 = 5C2 = 10가지 경우의 수
    ```
    ```
    [(1, 1), (1, 2), (1, 3), (1, 4), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (4, 4)]
    ```

1. **permutations**(iterable, r=None): iterable에서 원소 개수가 r개인 '순열' 뽑기
- 순열: 순서를 고려하고 뽑는 것
- iterable이 n개의 원소를 갖는다면, <sub>n</sub>P<sub>r</sub>개의 순열이 가능
- r값을 지정하지 않거나 r=None이라고 입력하면, r의 기본값은 iterable의 길이이며 가능한 모든 최대 길이 순열이 생성됨


    ```python
    from itertools import permutations

    print(list(permutations([1, 2, 3, 4], 2)))   # 4P2 = 12가지 경우의 수  # (1, 2)와 (2, 1)은 다른 것으로 간주됨
    ```
    ```
    [(1, 2), (1, 3), (1, 4), (2, 1), (2, 3), (2, 4), (3, 1), (3, 2), (3, 4), (4, 1), (4, 2), (4, 3)]
    ```

    +) r값을 입력하지 않는 경우: 
    ```python
    # r을 입력하지 않으면 default로 iterable의 길이인 4가 됨 → 4P4 = 24가지 경우의 수
    print(list(permutations([1, 2, 3, 4]))) 
    ```
    ```
    [(1, 2, 3, 4), (1, 2, 4, 3), (1, 3, 2, 4), (1, 3, 4, 2), (1, 4, 2, 3), (1, 4, 3, 2), (2, 1, 3, 4), (2, 1, 4, 3), (2, 3, 1, 4), (2, 3, 4, 1), (2, 4, 1, 3), (2, 4, 3, 1), (3, 1, 2, 4), (3, 1, 4, 2), (3, 2, 1, 4), (3, 2, 4, 1), (3, 4, 1, 2), (3, 4, 2, 1), (4, 1, 2, 3), (4, 1, 3, 2), (4, 2, 1, 3), (4, 2, 3, 1), (4, 3, 1, 2), (4, 3, 2, 1)]
    ```

### 무한 iterator

1. **count**(start=0, step=1): start부터 시작해 step만큼씩 무한 증가하는 숫자를 만든다
- 그냥 사용하면 숫자가 무한하게 계속 생성되므로, 보통은 zip과 같은 함수 안에 넣어서 사용

    ```python
    from itertools import count

    print(list(zip(count(10, 2), ['A', 'B', 'C', 'D'])))
    ```
    ```
    [(10, 'A'), (12, 'B'), (14, 'C'), (16, 'D')]
    ```

    *반복문에서 index를 붙여줄 때 활용하기 좋음

    ```python
    list1 = ['A', 'B', 'C', 'D', 'E']
    for i, element in zip(count(0, 1), list1):  
        print(i, ':', element)
    # zip(range(len(list1)), list1)과 결과는 같지만, len(list1)을 계산하지 않아도 된다는 장점이 있다
    ```
    ```
    0 : A
    1 : B
    2 : C
    3 : D
    4 : E
    ```

1. **cycle**(iterable): iterable의 요소를 무한 반복해준다
- count와 마찬가지로, 그냥 사용하면 무한하게 숫자가 생성되므로 주의해서 사용

    ```python
    from itertools import cycle

    print(list(zip(cycle('ABCD'), [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])))
    ```
    ```
    [('A', 1), ('B', 2), ('C', 3), ('D', 4), ('A', 5), ('B', 6), ('C', 7), ('D', 8), ('A', 9), ('B', 10)]
    ```

1. **repeat**(object, [times]): object를 times만큼 반복한다 (times를 지정하지 않으면 무한 반복됨)

    ```python
    from itertools import repeat

    print(list(repeat('A', 5)))  # 'A'를 5번 반복
    ```
    ```
    ['A', 'A', 'A', 'A', 'A']
    ```

    +) 무한 반복된다는 점을 활용해 이런 식으로 사용 가능: 
    ```python
    list(map(pow, range(10), repeat(2)))   # pow(x, y): x의 y제곱값을 return  
                                           # pow: 기본 내장함수도 있고, math.pow()도 있음 (둘 다 가능)
    ```
    ```
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    ```

### 조건형 iterator

1. **filterfalse**(predicate, iterable): predicate(술어, 조건절)이 False인 요소만 반환

    ```python
    from itertools import filterfalse

    print(list(filterfalse(lambda x: x > 5, range(10))))
    ```
    ```
    [0, 1, 2, 3, 4, 5]
    ```

    +) return값이 Boolean이 아닌 술어부의 경우, 값이 0인 경우를 False로 간주

    ```python
    print(list(filterfalse(lambda x: x % 2, range(10))))
    ```
    ```
    [0, 2, 4, 6, 8]
    ```

1. **takewhile**(predicate, iterable): 조건이 True인 동안만 요소를 반환 (iterable을 처음부터 탐색하다가, 조건이 False가 되는 순간 stop)

    ```python
    from itertools import takewhile

    print(list(takewhile(lambda x: x > 5, [6, 10, 7, 2, 9, 3, 1, 11, 12])))
    ```
    ```
    [6, 10, 7]
    ```

1. **dropwhile**(predicate, iterable): 조건이 False가 되는 순간부터 요소를 반환 (takewhile과 반대)

    ```python
    from itertools import dropwhile

    print(list(dropwhile(lambda x: x > 5, [6, 10, 7, 2, 9, 3, 1, 11, 12])))
    ```
    ```
    [2, 9, 3, 1, 11, 12]
    ```

1. **groupby**(iterable, key=None): 연속적인 키와 그룹을 반환하는 iterator 생성 (iterable을 처음부터 탐색하며, 같은 element가 연속적으로 나올 경우 이를 묶어준다)

    ```python
    from itertools import groupby

    dict1 = {}
    iterator = groupby('AAABBCCCCDDEE')
    for i, group in iterator:
        dict1[i] = list(group)

    print(dict1)
    ```
    ```
    {'A': ['A', 'A', 'A'], 'B': ['B', 'B'], 'C': ['C', 'C', 'C', 'C'], 'D': ['D', 'D'], 'E': ['E', 'E']}
    ```

    +) 같은 element여도, 연속적으로 나오지 않고 사이에 다른 element가 있으면 함께 묶이지 않는다
    ```python
    iterator = groupby([1, 1, 1, 1, 2, 3, 1, 1, 4, 4])  # 앞의 1 네 개와 뒤의 1 두 개는 따로 묶임
    for i, group in iterator:
        print(i, ':', list(group))
    ```
    ```
    1 : [1, 1, 1, 1]
    2 : [2]
    3 : [3]
    1 : [1, 1]
    4 : [4, 4]
    ```

## Collections
: 특수 컨테이너 데이터형을 구현해주는 모듈
- 파이썬의 범용 내장 컨테이너 dict, list, set 및 tuple에 대한 대안을 제공해준다. (namedtuple, deque 등)

### Counter
- Counter([iterable-or-mappine]): 해시 가능한 객체를 세는 데 사용하는 딕셔너리 서브 클래스. 요소가 딕셔너리 키로 저장되고 개수가 딕셔너리값으로 저장됨
    - iterable로 counter 생성하는 법 예시: `c = Counter('gallahad')`
    - mapping으로 counter 생성하는 법 예시: `c = Counter({'red': 4, 'blue': 2})` 

1. 대체로 각 element의 빈도를 세어주는 데에 활용

    ```python
    from collections import Counter

    c = Counter('AABCDADCBADFEGEOADGOAEFBBDA')
    print(c)
    ```
    ```
    Counter({'A': 7, 'D': 5, 'B': 4, 'E': 3, 'C': 2, 'F': 2, 'G': 2, 'O': 2})
    ```

1. c.most_common(n): 가장 빈도가 높은 n개의 element를 보여줌

    ```python
    c.most_common(5)
    ```
    ```
    [('A', 7), ('D', 5), ('B', 4), ('E', 3), ('C', 2)]
    ```

1. dictionary type에 적용되는 기능들은 대부분 동일하게 활용 가능

    ```python
    print(c.items())
    ```
    ```
    dict_items([('A', 7), ('B', 4), ('C', 2), ('D', 5), ('F', 2), ('E', 3), ('G', 2), ('O', 2)])
    ```