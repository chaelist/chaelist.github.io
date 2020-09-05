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

## 1. Numbers 
1.  Integer(정수) - ex. -2, 0, 1
1.  Float(소수) - 1.1, 3.14
1.  Bolean - True or False
<br/><br/>

### *type( ) 함수
: 각각 variable의 type을 확인하는 방법
```python
# type() 함수로 각각의 type을 체크
a = 1.1
b = 5
c = True
print('a:', type(a), 'b:', type(b), 'c:', type(c))  
```

```python
a: <class 'float'> b: <class 'int'> c: <class 'bool'>
```
<br/>

### *산술 연산자 (Arithmetic Operators)

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
<br/><br/>

## 2. List
a = ['python', 1, 5] 와 같이, [ ]로 표현됨
<br/><br/>

### *빈 리스트 생성
```python
# 빈 리스트를 만드는 방법
a = []
b = list()
print(a, b) # 두 방법 모두 동일
```

```python
[] []
```

### *Indexing & Slicing


