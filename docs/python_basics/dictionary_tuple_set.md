---
layout: default
title: Dictionary, Tuple, Set
parent: Python 기초
nav_order: 2
---

# Dictionary, Tuple, Set
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

## Dictionary
{Key : Value}와 같은 모양으로 표현됨
- {key:value, key2:value2, …} 이런 식으로 pair들 나열
- 이렇게 키와 값이 연결되는 개념을 보통 '연관 배열 (Associative Arrays)'이라고 한다.
- list는 값들에 순서대로 index가 매겨지지만, dictionary는 가방에 이것저것 라벨을 붙여서 넣는 것과 같다. (dictionaries are like bags - no order!)
- ‘Key’ is unique in the dictionary and must be immutable

```python
# 빈 Dictionary를 생성하는 방법
a = {}
b = dict()
print(a, b) # 두 방법 모두 동일
```

### Dictionary 기초
1. key로 value에 접근하기
```python
dict1 = {'Tom':23, 'John':34, 'Bob':12}
print(dict1['Tom'])   # 'Tom'의 value를 추출
```
```
23
```

2. Dictionary에 값 넣기
```python
purse = dict()  # 빈 dictionary 새로 생성
purse['money'] = 12  # key가 'money'고 value가 12인 pair 저장
purse['candy'] = 3   # key가 'candy'고 value가 3인 pair 저장
print(purse)
```
```
{'money': 12, 'candy': 3}
```
*주의: 특정 key는 하나뿐이며(unique), 변경불가능하다(immutable)
    ```python
    # 같은 key에 또 값을 assign해주면 그냥 그 key의 value가 대체될 뿐이다. (a key must be unique!)
    dict1 = {'Tom': 23, 'John': 34, 'Bob': 12, 'Sarah': 35}

    dict1['Sarah'] = 54 ## {'Sarah':54}가 새로 추가되는 대신, 원래 있던 'Sarah' key의 value가 54로 변경됨.
    print(dict1)
    ```
    ```
    {'Tom': 23, 'John': 34, 'Bob': 12, 'Sarah': 54}
    ```
`# 또한, 특정 key는 변경 불가능하기에, {'Sarah': 35}를 {'SARAH':35}로 바꾸는 방법은 없다`

1. pair의 수 (=key의 수) 세기: **len()**
```python
# len()
dict1 = {'Tom':23, 'John':34, 'Bob':12}
len(dict1)  # returns the number of keys (= number of pairs)
```
```
3
```

1. dictionary 삭제하기: **del**
    ```python
    # dictionary의 특정 element 삭제
    dict1 = {'Tom':23, 'John':34, 'Bob':12}
    del dict1['Tom']
    print(dict1)

    # dictionary 자체를 삭제하는 것도 가능
    del dict1
    print(dict1)  # dict1 자체가 아예 사라졌으므로, 존재하지 않는다고 error가 남.
    ```
    ```
    {'John': 34, 'Bob': 12}
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    <ipython-input-15-ecedb4a10042> in <module>()
        5 
        6 del dict1
    ----> 7 print(dict1)

    NameError: name 'dict1' is not defined
    ```

### Main Dictionary Functions
1. **dict1.clear()**: dictionary 안의 모든 데이터를 삭제
```python
# dict1.clear()
dict1 = {'Tom':23, 'John':34, 'Bob':12}
dict1.clear()  # dict1 안의 모든 값을 제거. dict1이라는 것 자체는 남아 있음 
                 ## cf) del dict1을 하면 해당 dictionary 자체가 사라짐
print(dict1)    
```
```
{}
```

1. **dict1.keys()**: dictionary 안의 모든 key들을 list-like variable로 반환
```python
# dict1.keys()
dict1 = {'Tom':23, 'John':34, 'Bob':12}
dict1.keys()
```
```
dict_keys(['Tom', 'John', 'Bob'])
```
*dict1.keys()의 결과로 반환되는 결과물은 list 형태로 변환이 가능 <br/>
(list-like variable: 'list(x)' function을 사용해서 list로 변경 가능)
```python
# list(dict1.keys())
key_list = list(dict1.keys())
print(key_list)  # list 형태로 출력됨
```
```
['Tom', 'John', 'Bob']
```
cf) list(dict1)을 해도 key만 list로 반환됨
```python
# list(dict1)
print(list(dict1))
```
```
['Tom', 'John', 'Bob']
```

1. **dict1.values()**: dictionary 안의 모든 value들을 list-like variable로 반환
```python
# dict1.values()
dict1 = {'Tom':23, 'John':34, 'Bob':12}
dict1.values()
```
```
dict_values([23, 34, 12])
```
- values() function은 별로 안 중요. keys() function만 기억해도 된다.
(key는 unique하니까, key만 알면 그 value에 access 가능. 하지만 value는 겹칠 수 있어서, value만 아는 건 useless.)
    {: .fs-3 }

1. **dict1.update(another_dicationary)**: 다른 dictionary의 내용을 가져와서 dictionary를 update.
```python
# dict1.update(another_dicationary)
dict1 = {'Tom':21, 'Kai':20}
dict2 = {'Sarah':22}
dict1.update(dict2)  # dict2의 정보를 dict1에 추가
print(dict1) 
```
```
{'Tom': 21, 'Kai': 20, 'Sarah': 22}
```
cf) update할 때, 두 개의 dictionary가 중복되는 key를 갖고 있다면?
```python
dict1 = {'Tom':21, 'Kai':20}
dict2 = {'Sarah':22, 'Tom':34}
dict1.update(dict2)
print(dict1)  # 'Tom'의 value가 dict2의 값으로 업데이트됨
```
```
{'Tom': 34, 'Kai': 20, 'Sarah': 22}
```

1. **dict1.get(name, default_value)**
-  dictionary에 존재하는 키인지 여부에 따라 값을 다르게 처리
-  name이라는 key가 dict1에 있으면 그 값을 가져오고, 아니면 따로 입력한 default value를 반환

    ```python
    # 'get' method 이용 예시
    ## 'names' 리스트에 각 이름이 몇 개씩 담겨있는지 세어서 'counts'라는 딕셔너리에 넣는 것.
    counts = dict()
    names = ['csev', 'cwen', 'csev', 'zqian', 'cwen']

    for name in names:
    counts[name] = counts.get(name, 0) + 1    
    # counts.get(name, 0): counts 딕셔너리에 name 키가 존재할 경우 이의 value를 불러오고, 
    # 그렇지 않을 경우에는 counts 딕셔너리에 {name : 0} 데이터를 추가하라는 의미

    print(counts)
    ```
    ```
    {'csev': 2, 'cwen': 2, 'zqian': 1}
    ```

1. **dict1.items()**: (key, value)의 tuple이 list-like variable로 출력됨
```python
# dict1.items()
dict1 = {'Tom':23, 'John':34, 'Bob':12}
dict1.items()
```
```
dict_items([('Tom', 23), ('John', 34), ('Bob', 12)])
```
+) dict1.items() 활용 예시
```python
jjj = {'chuck':1, 'fred':42, 'jan':100}
for aaa, bbb in jjj.items():
  print(aaa, bbb)
```
```
chuck 1
fred 42
jan 100
```

<div class="code-example" markdown="1">
+) get()과 item() 함수 이용 예시:<br/> 
{: .fs-4 }
{: .lh-0 }
dictionary의 get()과 item() 함수를 이용해, 예시 텍스트에서 가장 많이 나온 단어를 출력하는 코드
{: .fs-3 }

```python
text = "love live love start hands shake you feel love lower less dry deny follow hi love live like yogurt timeline loyal love"
# 연습을 위해, 랜덤한 단어를 나열한 단순한 text를 사용

counts = dict()

words = text.split()
for word in words:
  counts[word] = counts.get(word, 0) + 1

bigcount = None
bigword = None
for word, count in counts.items():
  if bigword is None or count > bigcount:
    bigword = word
    bigcount = count

print(bigword, bigcount)  # 가장 많이 나온 단어와 그 사용 빈도를 출력
```
```
love 5
```
</div>


## Tuple
- list와 비슷하나, [ ] 대신 ( )로 구성됨
- list처럼, 순서가 있어서 index number로 접근 가능 (indexing)
- 하지만 list와 달리 immutable. (한 번 만든 tuple은 modify할 수 없다)
- 변경불가능이기에 더 simple, efficient하다는 장점 -> preferred when making 'temporary variables'

### Tuple 기초
1. tuple에 적용 가능한 함수들 <br/>
(tuple은 list와 비슷하기에, 몇몇 list 함수들은 tuple에도 동일하게 적용된다)
```python
# tuple 예시
x = ('Glenn', 'Sally', 'Joseph')
print(x[0])  # indexing
print(x[:2])  # slicing
print(max(x))  # max, min 기능
print(x.count('Glenn'))  # x.count 기능 (특정 element의 개수 세기)
print(x.index('Glenn'))  # x.index 기능 (특정 element의 index number 찾기)
```
```
Glenn
('Glenn', 'Sally')
Sally
1
0
```

1. tuple is immutable; 한 번 만든 tuple은 변경할 수 없다
    ```python
    x = (9, 8, 7)
    x[2] = 6   ## index number가 2인 element를 6으로 바꾸려 시도 --> error.
    ```
    ```
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-4-0ca6b872377d> in <module>()
        1 x = (9, 8, 7)
    ----> 2 x[2] = 6  ## index number가 2인 element를 6으로 바꾸려 시도 --> error.

    TypeError: 'tuple' object does not support item assignment
    ```
    <div class="code-example" markdown="1">
    *주의: tuple은 한 번 생성된 후에는 변경 불가하기에, <br/>
    `x.sort()`, `x.append()`, `x.extend()`, `x.reverse()` 등의 기능이 없다. <br/>
    (list에 쓰는 method 중, list를 modify하는 속성의 아이들은 tuple에는 모두 적용 불가!)
    {: .fs-3 }
    </div>

### Tuple의 활용법 예시
1. tuple을 사용해 여러 개 variable를 한 번에 assign 가능
    ```python
    (x, y) = (4, 'fred')  # 각각 x=4, y='fred'로 저장됨
    print(x)

    a, b = (99, 98)  # 이렇게 괄호를 지우고 assign해도 된다
    print(a)

    a, b = 99, 98  # 이 것도 가능
    print(b)

    ## 이 특성을 이용해 dictionary.items()의 결과로 나온 (key, value) tuple에 쉽게 접근 가능
    ## (ex. for k, v in di.items(): 와 같은 코드 이용)
    ```
    ```
    4
    99
    98
    ```

1. tuple 간 비교
- 첫번째부터 순서대로 비교; 만약 첫번째 element이 같다면 다음 element끼리 비교하고,..이런 식.

    ```python
    print((0, 1, 2) < (5, 1, 2))
    print((0, 1, 3000) < (0, 3, 4))
    print(('Jones', 'Sally') < ('Jones', 'Sam'))
    print(('Jones', 'Sally') > ('Jones', 'Sam'))
    ```
    ```
    True
    True
    True
    False
    ```

1. dictionary 정리1: sorting by keys
```python
d = {'a':10, 'b':1, 'c':22}
print(d.items())
print(sorted(d.items()))  # list 속 tuple들의 첫번재 element를 기준으로 비교해서 sort해준다
                            ## sorted()를 하면 dict_items 대신 list variable로 출력됨
```
```
dict_items([('a', 10), ('b', 1), ('c', 22)])
[('a', 10), ('b', 1), ('c', 22)]
```

1. dictionary 정리2: sorting by values (instead of keys)
    ```python
    d = {'a':10, 'b':1, 'c':22}
    tmp = list()

    for k, v in d.items():
    tmp.append((v, k))  # 반대로 뒤집어서 list에 넣어줌
    print(tmp)

    tmp = sorted(tmp, reverse=True)  # 역순으로 정렬
    print(tmp)
    ```
    ```
    [(10, 'a'), (1, 'b'), (22, 'c')]
    [(22, 'c'), (10, 'a'), (1, 'b')]
    ```
    <div class="code-example" markdown="1">
    +) List Comprehension
    ```python
    # 위에서 썼던 다음과 같은 코드를
    tmp = list()
    for k, v in d.items():
    tmp.append((v, k))

    # --> 아래와 같이 간결하게 작성 가능
    tmp = [(v, k) for k, v in d.items()]
    ```
    </div>

## Set (집합자료형)
- { } 안에 값들이 나열된 형태. (cf. dictionary는 { } 안에 'key:value'의 "조합"들이 담긴 형태)
- 중복을 허용하지 않는다 
- 순서가 없다 (unordered) -> indexing 불가. (cf. dictionary도 순서가 없어서 indexing 불가)
- 하지만, 쉽게 list나 tuple로 변환할 수 있기에, list나 tuple로 변환 후 indexing하면 됨

### Set 기초
1. Set(집합) 만들기
    ```python
    # 1. list 입력해서 set 만들기
    s1 = set([1, 2, 3])
    print(s1)

    # 2. string 입력해서 set 만들기
    s2 = set("Hello")
    print(s2)  # 중복이 제거되어 나오고, 순서도 뒤죽박죽으로 나옴 (순서가 없는 자료형이라)
    ```
    ```
    {1, 2, 3}
    {'H', 'o', 'l', 'e'}
    ```

1.  '중복을 허용하지 않는다' >> 중복 제거 목적으로 종종 사용
```python
l1 = [1, 2, 3, 4, 1, 5, 2, 3, 6, 9]
s1 = set(l1)
print(s1)  # 중복되는 숫자가 다 제거됨
```
```
{1, 2, 3, 4, 5, 6, 9}
```

1. **add()**: 값 1개 추가하기
```python
# add
s1 = set([1, 2, 3])
s1.add(4)   # list의 append()와 유사
s1
```
```
{1, 2, 3, 4}
```

1. **update()**: 값 여러 개 추가하기
```python
# update
s1 = set([1, 2, 3])
s1.update([4, 5, 6])   # list의 extend()와 유사
s1
```
```
{1, 2, 3, 4, 5, 6}
```

1. **remove()**: 값 1개 제거하기
```python
s1 = set([1, 2, 3])
s1.remove(2)
s1
```
```
{1, 3}
```

### 교집합, 합집합, 차집합
1. 교집합 구하기: `&` 기호 or `intersection()` 함수
    ```python
    # 교집합
    s1 = set([1, 2, 3, 4])
    s2 = set([3, 4, 5, 6])

    print(s1 & s2) # "&" 기호로 간단히 교집합을 구할 수 있다
    print(s1.intersection(s2))  # intersection 함수를 써도 동일한 결과
    ```
    ```
    {3, 4}
    {3, 4}
    ```

1. 합집합 구하기: `|` 기호 or `union()` 함수
    ```python
    # 합집합
    s1 = set([1, 2, 3, 4])
    s2 = set([3, 4, 5, 6])

    print(s1 | s2)  # "|" 기호로 간단히 합집합을 구할 수 있다
    print(s1.union(s2))  # union 함수를 써도 동일한 결과
    ```
    ```
    {1, 2, 3, 4, 5, 6}
    {1, 2, 3, 4, 5, 6}
    ```

1. 차집합 구하기: `-` 기호 or `difference()` 함수
    ```python
    # 차집합
    s1 = set([1, 2, 3, 4])
    s2 = set([3, 4, 5, 6])

    # s1에 대한 s2의 차집합
    print(s1 - s2)   # "-" 기호 사용
    print(s1.difference(s2))  # difference 함수 사용

    # s2에 대한 s1의 차집합
    print(s2 - s1)   # "-" 기호 사용
    print(s2.difference(s1))  # difference 함수 사용
    ```
    ```
    {1, 2}
    {1, 2}
    {5, 6}
    {5, 6}
    ```

