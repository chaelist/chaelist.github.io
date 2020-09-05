---
layout: default
title: Data Types
parent: Data Handling
nav_order: 1
---

# Data Handling
{: .no_toc }

python data handling 기초
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


View this site's [_config.yml](https://github.com/pmarsceill/just-the-docs/tree/master/_config.yml) file as an example.

## Numbers 
1.  Integer(정수)
1.  Float(소수)
1.  Bolean(True or False)

```python
# Checking the type of a variable
a = 1.1
b = 5
c = True
print('a:', type(a), 'b:', type(b), 'c:', type(c))  # type() 함수로 각각의 type을 체크
```

```python
a: <class 'float'> b: <class 'int'> c: <class 'bool'>
```
<br/>
```python
# Arithmetic Operators
a = 5 
b = 3
print(a + b) # addition
print(a - b) # subtraction
print(a * b) # multiplication
print(a / b) # division (소수점으로 나옴)
print(a % b) # modulus. 나머지를 return. (ex. 5 % 3 = 2)
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