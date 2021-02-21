---
layout: default
title: Control Flow (제어문)
parent: Python 기초
nav_order: 3
---

# Control Flow (제어문)
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

## if-else문 (조건문)
- 특정한 조건이 충족될 때만(= True일 때만) 특정한 Python 코드들을 실행하게 함
- if 조건문의 결과로 실행되어야 하는 아래 코드들에는 indentation(들여쓰기)를 해주어야 함

*형식:
```python
if condition1:
    body1   # body에는 여러 줄의 코드가 들어가도 됨. (들여쓰기로 구간이 잘 표현만 되면)
else:  # else는 안써도 됨
    body2 
```

*예시:
```python
num1 = -1
if num1 > 0:  # num1이 0보다 클 경우 아래 코드들이 실행
  print(num1)
  print('positive')
else:   # num1이 0보다 크지 않을 경우 아래 코드들이 실행
  print(num1)
  print('negative')
```
```
-1
negative
```

### Comparison tests
- a < b (b보다 작다), a <= b (b보다 작거나 같다)
- a > b (b보다 크다), a >= b (b보다 크거나 같다)
- == (같다), != (다르다)
- 'in' (포함되어 있다), 'not in' (포함되어 있지 않다)

```python
# equality test 예시
a = 2
b = 3
a != b  # 둘이 다르므로 True
```
```
True
```

```python
# in / not in
x = [3, 5, 9]
if 3 in x:
  print('included')
else:
  print('not included')
```
```
included
```

### 'pass'
: 조건이 True일 경우 아무 일도 일어나지 않게 설정하고 싶다면, 'pass'를 사용하면 된다
```python
# 예시: 
bag = ['apple', 'water', 'cell phone']
if 'apple' in bag:  
  pass  # apple이 bag에 있기 때문에, 아무 결과값도 출력되지 않는다. 
else:
  print('need an apple')
```
```
```

### if - elif - else
- for more than one condition, we use 'if - elif - else' statement
- elif는 무수히 많이 사용 가능. 

*형식:
```python
if condition1:
    body1
elif condition2:
    body2
elif condition3:
    body3
else:
    body4
```

** if-elif-else 구문은 위에서부터 점점 밑으로 내려가며 test되다가,  
충족되는 condition을 만나면 해당 body가 실행되며 더 이상 아래 조건들은 test되지 않는다

```python
# 예시:
a = 35
if a > 30:  # 첫 번째 조건이 충족되므로 여기서 process가 끝나고, 25가 출력됨
  a= a-10
  print(a)
elif 20 <a <= 40:  
  a = a-5
  print(a)
else:
  print(a)

# 첫번째와 두번째 조건에 겹치는 부분이 있지만, 이미 첫번째 조건이 충족되어서 process가 끝나기에 더 이상 밑으로 진행되지 않는다.
## 하지만 물론 조건들은 서로 겹치지 않게 작성하는 게 더 좋음!
```
```
25
```

** cf) if-elif 대신 if만 반복해서 쓰면, 독립적인 세트로 간주되어 모두 test됨.
```python
# if-elif 대신 if만 두 개 쓰는 경우
a = 3
if a > 0:
  a = a+1
  print(a)
if a > 2:
  print(a)

# if가 두 개면, 서로 독립적인 두 개의 다른 세트로 간주됨. 
# 첫번재 조건에서 true여도 두번째 조건도 test되고, 두번째 조건도 또 true이면 둘 다 실행됨.
```
```
4
4
```

<div class="code-example" markdown="1">
+) **예시: 자동 학점 계산기**

```python
## 'input()' 함수로 점수 받아서 학점 계산해보기
num = input('Enter your score: ')
x = int(num)  # input()을 통해 받은 숫자는 string type으로 저장되므로, int()로 변환해야 계산 가능
if x < 50:
  print('F')
elif 50 <= x < 60:
  print('D')
elif 60 <= x <70:
  print('C')
elif 70 <= x < 80:
  print('B')
else:
  print('A')
```
```
Enter your score: 70
B
```
</div>

## for 반복문
*형식:
```python
for item in list-like variable:
    body
```
- item은 해당 list-like variable의 각 값들을 취하는 temporary variable
- 특정 list-like variable에 있는 값들을 하나씩 취해서 차례로 특정 body 구문을 실행

*예시:
1. for문 예시1: list 안의 variable 차례로 print하기
```python
x = ['a', 'b', 'c']
for k in x:
  print(k)
```
```
a
b
c
```

1. for문 예시2: adding up all elements in a list
```python
x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
total = 0
for k in x:
  total = total + k   # x 안의 값을 하나씩 취해서 차례로 더해줌
print(total)
```
```
55
```

1. for문 예시3: adding up even numbers only
```python
# for문과 if문을 함께 사용해, 특정 조건의 값들만 더하기
x = [2, 3, 1, 4, 5]
total = 0
for k in x:
  if k % 2 == 0:
    total = total + k
print(total)
```
```
6
```

### range() function
- for a number n, `range(n)` returns a sequence of 0, 1, ... n-1
    - ex) range(5) --> 0, 1, 2, 3, 4 차례로 접근

**추가:
- `range(a, b)`: 시작값과 끝값을 지정
    - a, a+1, ... b-1
- `range(a, b, k)`: 시작값, 끝값, 간격을 지정
    - a, a+k, a+2k, ... b-1 이런 식으로 k 간격으로 a ~ b-1 사이의 값들을 접근
    - ex) range(2, 10, 2) -> 2, 4, 6, 8

1. range(n) 예시
```python
for k in range(5):
  print(k)
```
```
0
1
2
3
4
```

1. range(a, b, k) 예시
```python
for a in range(3, 10, 2):
  print(a)
```
```
3
5
7
9
```
1. 응용: range(len(s))를 통해 리스트 's'에 있는 값들의 index number에 접근
```python
s = ['a', 'b', 'c']
for k in range(len(s)):
  print(s[k])
### 물론 `for k in s: print(k)` 이렇게 하는 거랑 결과는 똑같음,,,
```
```
a
b
c
```

### 'continue'와 'break'
: 특정 condition 아래 'for' loop를 멈추거나 escape하는 방법

1. continue: 특정 조건을 만족하는 element에 대해서는 나머지 body가 실행되지 X, 다음 element로 process가 넘어간다 (남아있는 element가 있다면 for문 자체는 계속되는 것)
```python
x = [2, -1, 3]
total = 0
for k in x:
  if k < 0:  # k가 음수일 경우에는 continue 
    continue   ## 아래 total = total + k 부분이 실행되지 않고, 다음 element로 넘어간다
  total = total + k
print(total) # 결국, 양수만 더하는 결과가 나옴.
```
```
5
```

2. break: 특정 조건이 충족되는 순간, for문이 아예 다 멈춰버린다 (다음 element가 남아있어도 넘어가지 않음)
```python
x = [2, -1, 3]
total = 0
for k in x:
  if k < 0:  # k가 음수일 경우에 아예 해당 'for' process가 다 멈춤. 다음 element로도 넘어가지 않음.
    break
  total = total + k
print(total)
## 2는 일단 total = total + k까지 내려가서 더해지지만, -1에서 if가 충족되면서 break를 만나 for문이 멈추면서 그대로 total은 2에서 멈춤
```
```
2
```

### List Comprehension
* 리스트 내포 (List Comprehension): 리스트 안에 for문을 내포해 간결하게 작성
```python
# list comprehension 예시
a = [1, 2, 3, 4]
result = [num * 3 for num in a]
print(result)
```
```
[3, 6, 9, 12]
```
* if 조건을 함께 내포하는 것도 가능
```python
# List comprehension 예시2: if 조건도 함께 내포
a = [1, 2, 3, 4]
result = [num * 3 for num in a if num % 2 == 0]
print(result)
```
```
[6, 12]
```

## while 반복문
- 특정 조건이 True인 동안 영원히 돌아간다. (조건이 False가 될 때까지)
- 조건을 잘 설정하지 않으면 정말 '영원히' 돌아가니, 조심!

*형식:
```python
while condition:
    body
```

*예시:
```python
a = 10
while a > 0:
  print(a)
  a = a - 1
```
```
10
9
8
7
6
5
4
3
2
1
```

### 'continue'와 'break'
: while문도 continue와 break를 활용해 특정 element를 skip하거나 전체 loop를 강제로 중단시킬 수 있다

1. continue 사용 예시
  ```python
  a = 0
  while a < 10:
    a = a + 1
    if a % 2 == 0:  # 2의 배수일 경우에는 print(a)가 실행되지 않은 채 다시 맨 처음으로 돌아간다
      continue
    print(a)
  ```
  ```
  1
  3
  5
  7
  9
  ```

1. break 사용 예시
  ```python
  a = 0
  while a < 10:
    a = a + 1
    if a % 3 == 0:  # 2의 배수를 만나면 while loop가 아예 중단된다 
      break
    print(a)
  ```
  ```
  1
  2
  ```

## try - except문 (오류 예외 처리)
1. 기본 try - except문
- 오류가 발생할 시 except 블록이 실행된다

    ```python
    try:
        실행해야 하는 코드
    except:
        오류가 날 경우 실행할 코드
    ```

1. 발생 오류 종류를 포함한 except문
- except문에 미리 정해둔 오류 이름과 일치할 때만 except 블록 실행

    ```python
    try:
        실행해야 하는 코드
    except 발생 오류:
        오류가 날 경우 실행할 코드
    ```

    *예시:
    ```python
    try:
        a = [1,2]
        print(a[3])
    except IndexError:
        print("인덱싱 불가능")
    ```
    ```
    인덱싱 불가능
    ```

    +) 여러 개의 오류를 처리하는 것도 가능
    ```python
    try:
        실행해야 하는 코드
    except 발생 오류1:
        오류1이 날 경우 실행할 코드
    except 발생 오류2:
        오류2가 날 경우 실행할 코드
    ```

1. try ... finally
- finally절은 try문 수행 도중 예외 발생 여부와 상관없이 항상 수행된다

    ```python
    try:
        실행해야 하는 코드
    finally: 
        오류 여부와 상관 없이 실행되어야 하는 코드
    ```

    *예시:
    ```python
    f = open('message.txt', 'w')
    try:
        실행해야 하는 코드
    finally:
    f.close()  # try문 중 오류 여부와 상관없이 반드시 실행되어야 하는 코드

    ## 예외 발생 여부와 상관없이 finally절에서 f.close()로 열린 파일을 닫을 수 있다.
    ```

1. 'pass'로 오류 회피하기  
```python
try:
    실행해야 하는 코드
except:
    pass  # 오류가 나면 그냥 pass해버리기
```
*예시:
```python
# 오류 회피 예시
try:
  f = open("없는 파일", 'r')
except FileNotFoundError:
  pass  ## 파일이 찾아지지 않으면 그냥 pass.
```