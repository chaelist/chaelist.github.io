---
layout: default
title: Numbers, List, String
parent: Python 기초
nav_order: 1
---

# Numbers, List, String
{: .no_toc }
<br/>

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Numbers 
1.  Integer(정수) - ex. -2, 0, 1
1.  Float(소수) - ex. 1.1, 3.14
1.  Bolean - True or False
<br/>

### type( ) 함수
: 각각 variable의 type을 확인하는 방법
```python
# type() 함수로 각각의 type을 체크
a = 1.1
b = 5
c = True
print('a:', type(a))
print('b:', type(b))
print('c:', type(c))  
```
```
a: <class 'float'>
b: <class 'int'>
c: <class 'bool'>
```

### 산술 연산자 (Arithmetic Operators)

```python
# 연산 예시
a = 5 
b = 3

print(a + b) # addition
print(a - b) # subtraction
print(a * b) # multiplication
print(a / b) # division (나누어진 결과를 소수로 표시)
print(a % b) # modulus. 나머지 값을 반환. (ex. 5 % 3 = 2)
print(a ** b) # exponentiation (지수. 제곱) (ex. 3**5는 3의 5제곱을 의미)
```
```
8
2
15
1.6666666666666667
2
125
```
<br/>

## List
a = ['python', 1, 5] 와 같이, [ ]로 표현됨
<br/>

### 빈 리스트 생성
```python
# 빈 리스트를 만드는 방법
a = []
b = list()
print(a, b) # 두 방법 모두 동일
```
```
[] []
```

### Indexing
index number를 이용해 각 element에 접근할 수 있다.
<div markdown="1">
- index number는 0부터 시작 
    - 첫번째 요소의 index number: 0
    - 두번째 요소의 index number: 1
- 역순 indexing도 가능
    - 마지막 요소의 index number: -1
    - 마지막에서 두번째 요소의 index number: -2
- [ ]를 사용해 indexing
    - a[0]: a의 첫번째 요소의 값을 반환
</div>

```python
# Indexing
a = ['python', 1, 5]

print(a[0])  # 첫번째 element를 의미
print(a[2])  # 세번째 element를 의미
print(a[-1])  # 마지막 element를 의미 (역순 indexing)
```
```
python
5
5
```

### Slicing
**list_name[index1:index2]**와 같은 방식으로, 특정 구간의 index들에 모두 접근

* index1 <= index < index2
* index number가 index1보다 크거나 같고 index2보다 작은 모든 요소를 반환 (index1 이상, index2 미만)

```python
# Slicing
a = ['python', 1, 5]

print(a[0:2]) # index0, index1 (index2는 미포함) 
print(a[1:]) # index1이상~끝까지
print(a[:2]) # 처음~index1까지
print(a[:]) # 그냥 a itself (처음~끝)
```
```
['python', 1]
[1, 5]
['python', 1]
['python', 1, 5]
```

### List 변경하기
List is mutable; List can be modified

```python
# Modifying a list
x = [1, 2, 3, 4]
x[1] = 'python'
print(x)  # index 1번 자리가 'python'으로 변경됨
```
```
[1, 'python', 3, 4]
```

### Main List Fuctions
1. append(): 한 개의 element '추가'
```python
# append
x = [1, 2, 3, 4]
x.append(5)
x
```
```
[1, 2, 3, 4, 5]
```

1. extend(): 새로운 list를 추가해 '확장'
```python
# extend
x = [1, 2, 3, 4]
x.extend([6,7])
x
```
```
[1, 2, 3, 4, 6, 7]
```
cf) append에 list를 넣으면?
```python
x = [1, 2, 3, 4]
x.append([6,7])
x  # extend와 다르게, 아예 list 자체가 하나의 element로 간주되어 들어감.
```
```
[1, 2, 3, 4, [6, 7]]
```

1. remove(): 한 개의 element를 제거
```python
# remove
x = [1, 2, 3, 4]
x.remove(1)
x
```
```
[2, 3, 4]
```
cf) 같은 값의 element가 두 개인 상황에서 remove()를 사용하면?
```python
x = [1,2,3,4,1]
x.remove(1)
x  ## 같은 값이 두 개일 경우, 더 앞에 있는 element만 지워진다.
```
```
[2, 3, 4, 1]
```

1. index(): 해당 element의 index number를 가져옴
```python
# index
x = [1, 2, 3, 4]
x.index(3)   # 3은 3번째 값, 즉 index number가 2인 값이므로 '2'가 반환됨
```
```
2
```
cf) 같은 element가 2개 이상일 때 index() 함수를 사용하면?
```python
x = [1, 2, 3, 4, 3]
x.index(3)  # 맨 처음 나오는 '3'의 index number인 2만 return.
```
```
2
```

1. count(): 해당 element의 개수를 셈
```python
# count
x = [1, 2, 3, 4, 3]
x.count(3)  # 3이 몇 개인지 세기
```
```
2
```

### Other common list operations / functions
1. 'in' operator: 특정 element가 list 안에 있는지 없는지, boolean값을 반환
```python
# 'in'
x = [1, 2, 3, 4]
print(1 in x) # True
print(5 in x) # False
```
```
True
False
```

1. min(), max(): 최소값, 최대값을 반환
```python
# min, max
x = [1, 2, 3, 4]
print(min(x))  # 최소값
print(max(x))  # 최대값
```
```
1
4
```
cf) string element들로 이루어진 list의 min(), max()
```python
y = ['c', 'b', 'a'] 
print(min(y))
print(max(y))
# string의 경우에도, 각각 number로 된 unicode를 갖기에, 해당 unicode를 비교해서 max, min을 return해줌
## 하지만 integar와 string처럼 서로 다른 type들이 섞인 list는 min, max 계산 불가.
```
```
a
c
```

1. sum(): list에 있는 모든 값을 다 더해줌 (*값이 모두 integer 혹은 float여야 함)
```python
# sum
x = [1, 2, 3, 4]
sum(x)
```
```
10
```
cf) integer와 float이 공존하는 list도 sum() 가능
```python
x = [1, 2, 3.5, 4]
sum(x)
```
```
10.5
```

1. len(): list의 element 수를 알려줌
```python
# len(x)
x = [1, 2, 3, 4]
len(x)  # element가 4개니까 4
```
```
4
```

1. del x[index]: 해당 index number를 갖는 element를 삭제
```python
# del x[index]
x = [1, 2, 3, 4]
del x[0]  # index number가 0인 '1'을 삭제
x
```
```
[2, 3, 4]
```
cf) slicing을 통한 del도 가능
```python
x = [1, 2, 3, 4]
del x[0:2]  # index number가 0, 1인 값을 모두 delete
x
```
```
[3, 4]
```
*주의: remove와 del의 차이

```python
x = [1, 2, 3, 4]
x.remove(3)
print('remove의 결과:', x) # 실제 '3'이란 값을 갖는 element가 삭제되는 것.

x = [1, 2, 3, 4]
del x[3]
print('del의 결과:', x) # index number가 3인 element, 즉 '4'가 삭제되는 것.
```
```
remove의 결과: [1, 2, 4]
del의 결과: [1, 2, 3]
```

1. x.sort(): list 안의 값들을 순서대로 정렬해준다 (숫자는 오름차순, string은 첫 글자 알파벳순)

```python
# x.sort()
x = [1, 4, 3, 2, 5]
x.sort()
print(x)

friends = ['Joseph', 'Glenn', 'Sally']
friends.sort()
print(friends)
```
```
[1, 2, 3, 4, 5]
['Glenn', 'Joseph', 'Sally']
```

cf) 대문자와 소문자가 공존할 경우: 대문자가 먼저 알파벳순으로 정렬되고, 그 다음 소문자가 정렬됨

```python
fruits = ['apple', 'Banana', 'carrot', 'Dragonfruit']
fruits.sort()
print(fruits)
```
```
['Banana', 'Dragonfruit', 'apple', 'carrot']
```

<br/>

## String
문자열. sequence of characters. ' '이나 " "를 활용해 표현
<br/>

### String 기초
1. Indexing & Slicing: list처럼, indexing & slicing 가능
```python
s = 'python'
print(s[0])
print(s[-1])
print(s[0:3])
```
```
p
n
pyt
```

1. len(): string의 글자수를 세는 개념
```python
# len()
s = 'python'
len(s)
```
```
6
```

1. string간 덧셈(+)
```python
h = 'Hello'
w = 'World'
s = h + w  # 두 개를 더하면 단순히 앞뒤로 이어붙여진다
s
```
```
'HelloWorld'
```

1. '나 "를 중간에 삽입하는 법
```python
print('Tom\'s Book')  # \를 이용
print("Tom's Book")  # string을 감싸는 따옴표를 다른 종류로 사용
```
```
Tom's Book
Tom's Book
```

1. white space characters (공백)
-  \t: tab
-  \n: enter (newline)
```python
z1 = 'Tom is busy studying. \nI am not busy. \t\tYou?'
print(z1)
```
```
Tom is busy studying. 
I am not busy. 		You?
```

### Main String Fuctions
1. split(): whitespace(공백)을 기준으로 split
-  split('a'): 'a'를 기준으로 split
-  split 결과는 list로 나타남
```python
# split()
s = 'Today is a good day'
print(s.split())
print(s.split('a'))
print(s.split('good'))
```
```
['Today', 'is', 'a', 'good', 'day']
['Tod', 'y is ', ' good d', 'y']
['Today is a ', ' day']
```

1. strip(): 양쪽 끝의 whitespace를 제거
-  strip('n'): 양쪽 끝의 'n' 문자 제거
-  lstrip()은 왼쪽 끝 element만, rstrip()은 오늘쪽 끝 element만 제거
```python
# strip()
t = '\tpyth\ton\n'
t.strip()  ## 양 끝의 whitespace만 제거해주고, 중간의 \t는 제거되지 않는다
```
```
'pyth\ton'
```

1. replace('a', 'b'): 모든 'a'를 'b'로 대체
```python
# replace()
s = 'python is important'
print(s.replace('o', 'a'))

print(s) #유의사항: string은 immutable! replace를 해도 원본 s가 바뀌는 것은 아니다.
## 바뀐 결과를 저장하고 싶으면 새로운 variable로 따로 저장해둬야 함
```
```
pythan is impartant
python is important
```
*무언가를 없애고 싶을 때에도 replace()를 사용
-  ex) replace('a', '')라고 하면 'a'를 다 없애주는 기능. (두번째 '' 안을 비워두면 됨)
```python
s = 'python, is, important,'
print(s.replace(',', ''))
```
```
python is important
```



