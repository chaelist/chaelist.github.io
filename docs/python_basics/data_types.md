---
layout: default
title: Data Types
parent: Python 기초
nav_order: 1
---

# Data Types
{: .no_toc }
<br/>

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Numbers 
1.  Integer(정수) - ex. -2, 0, 1
1.  Float(소수) - 1.1, 3.14
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

```python
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
print(a / b) # division (나누어진 결과를 소수점으로 표시))
print(a % b) # modulus. 나머지 값을 반환. (ex. 5 % 3 = 2)
print(a ** b) # exponentiation (지수. 제곱) (ex. 3**5는 3의 5제곱을 의미)
```

```python
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

```python
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

```python
python
5
5
```

### Slicing
**list_name[index1:index2]**와 같은 방식으로, 특정 구간의 index들에 모두 접근
<div markdown="1">
- index1 <= index < index2
- index number가 index1보다 크거나 같고 index2보다 작은 모든 요소를 반환 (index1 이상, index2 미만)
</div>

```python
# Slicing
a = ['python', 1, 5]

print(a[0:2]) # index0, index1 (index2는 미포함) 
print(a[1:]) # index1이상~끝까지
print(a[:2]) # 처음~index1까지
print(a[:]) # 그냥 a itself (처음~끝)
```

```python
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

```python
[1, 'python', 3, 4]
```



