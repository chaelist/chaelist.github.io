---
layout: default
title: Numbers, List, String
parent: Python 기초
nav_order: 1
---

# Numbers, List, String
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

## Numbers
1. Integer(정수) - ex. -2, 0, 1
1. Float(소수) - ex. 1.1, 3.14
1. Bolean - True or False

### type() 함수
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
print(a // b) # quotient. 나눈 몫을 반환 (ex. 5 // 3 = 1)
print(a % b) # modulus. 나머지 값을 반환. (ex. 5 % 3 = 2)
print(a ** b) # exponentiation (지수. 제곱) (ex. 3**5는 3의 5제곱을 의미)
```
```
8
2
15
1.6666666666666667
1
2
125
```
<br/>

## List
a = ['python', 1, 5] 와 같이, [ ]로 표현됨

```python
# 빈 리스트를 생성하는 방법
a = []
b = list()
print(a, b) # 두 방법 모두 동일
```
```
[] []
```

### Indexing
index number를 이용해 각 element에 접근할 수 있다.
- index number는 0부터 시작 
    - 첫번째 요소의 index number: 0
    - 두번째 요소의 index number: 1
- 역순 indexing도 가능
    - 마지막 요소의 index number: -1
    - 마지막에서 두번째 요소의 index number: -2
- [ ]를 사용해 indexing
    - a[0]: a의 첫번째 요소의 값을 반환 

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
list_name[index1:index2]와 같은 방식으로, 특정 구간의 index들에 모두 접근

- index1 <= index < index2 (index1 이상, index2 미만)
- index number가 index1보다 크거나 같고 index2보다 작은 모든 요소를 반환

```python
# Slicing
a = ['python', 1, 5]

print(a[0:2]) # index0, index1 (index2는 미포함) 
print(a[1:]) # index1이상 ~ 끝까지
print(a[:2]) # 처음 ~ index1까지
print(a[:]) # 그냥 a itself (처음 ~ 끝)
```
```
['python', 1]
[1, 5]
['python', 1]
['python', 1, 5]
```

**+) list_name[index1:index2:step]**
- index1 이상, index2 미만의 요소 중, 1번째, 1+step번째, 1+2step번째,...의 요소를 반환

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numbers[1:5:2] # index number가 1, 3인 요소 반환
print(numbers[:6:3]) # index number가 0, 3인 요소 반환
print(numbers[::2]) # index number가 0, 2, 4, 6, 8인 요소 반환
```
```
[2, 4]
[1, 4]
[1, 3, 5, 7, 9]
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
1. **append()**: 한 개의 element '추가'
```python
# append
x = [1, 2, 3, 4]
x.append(5)
x
```
```
[1, 2, 3, 4, 5]
```

1. **extend()**: 새로운 list를 추가해 '확장'
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

    +) list끼리 +로 더해줘도 extend의 효과:
    ```python
    x = [1, 2, 3, 4]
    x = x + [6,7]
    x
    ```
    ```
    [1, 2, 3, 4, 6, 7]
    ```


1. **insert()**: list의 '특정 index에' element를 추가
```python
x = [1, 2, 3, 4] 
x.insert(2, 5)  # index number = 2인 위치에 5라는 element를 추가
x
```
```
[1, 2, 5, 3, 4]
```


1. **remove()**: 한 개의 element를 제거
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

1. **index()**: 해당 element의 index number를 가져옴
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

1. **count()**: 해당 element의 개수를 셈
```python
# count
x = [1, 2, 3, 4, 3]
x.count(3)  # 3이 몇 개인지 세기
```
```
2
```

### Other Common List Operators / Functions
1. '**in**' operator: 특정 element가 list 안에 있는지 없는지, boolean값을 반환
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

1. **min(x), max(x)**: 최소값, 최대값을 반환
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
cf) string element들로 이루어진 list의 min(x), max(x)
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

1. **sum(x)**: list에 있는 모든 값을 다 더해줌 (*값이 모두 integer 혹은 float여야 함)
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

1. **len(x)**: list의 element 수를 알려줌
```python
# len(x)
x = [1, 2, 3, 4]
len(x)  # element가 4개니까 4
```
```
4
```

1. **del x[index]**: 해당 index number를 갖는 element를 삭제
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

    <div class="code-example" markdown="1">

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

    </div>

1. **sort()**: list 안의 값들을 순서대로 정렬해준다 (숫자는 오름차순, string은 첫 글자 알파벳순)
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

    +) **sorted(x)**: sort()와 마찬가지로, list 내용물을 알파벳순으로 정렬.
    - `x.sort()`는 x 자체를 변경 / `sorted(x)`는 x는 변형하지 않은 채 알파벳 순으로 정렬된 리스트를 반환

    ```python
    x = [1, 4, 3, 2, 5]
    sorted(x)
    ```
    ```
    [1, 2, 3, 4, 5]
    ```

1. **reverse()**: list 안의 element 순서를 반대로 뒤집어준다
```python
x = [1, 4, 3, 2, 5]
x.reverse()
print(x)
```
```
[5, 2, 3, 4, 1]
```

    +) **reversed(x)**: x는 변형하지 않고, x의 element를 반대 순서로 정렬하는 iterator를 반환해준다
    - `sorted(x)`와 달리, 변형된 리스트가 return되는 것이 아니라 iterator가 반환된다

    ```python
    x = [1, 4, 3, 2, 5]
    [i for i in reversed(x)]   # iterator가 반환되므로 이런 식으로 사용해야 list로 반환시킬 수 있음
    ```
    ```
    [5, 2, 3, 4, 1]
    ```

    +) indexing을 통한 list 뒤집기:

    ```python
    x = [1, 4, 3, 2, 5]
    x[::-1]
    ```
    ```
    [5, 2, 3, 4, 1]
    ```

## String
문자열. sequence of characters. ' '이나 " "를 활용해 표현


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

1. **len()**: string의 글자수를 세는 개념
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
    print('Tom\\s Book')  # \를 이용
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
1. **split()**: whitespace(공백)을 기준으로 split
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

    +) **join()**: split된 문자열을 다시 모아주기

    ```python
    split_list = ['Today', 'is', 'a', 'good', 'day']
    print(' '.join(split_list))  # 중간에 공백을 두고 list의 element들을 합쳐줌
    print('/'.join(split_list))  # 중간에 /를 두고 합쳐줌
    ```
    ```
    Today is a good day
    Today/is/a/good/day
    ```


1. **strip()**: 양쪽 끝의 whitespace를 제거
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

1. **replace('a', 'b')**: 모든 'a'를 'b'로 대체
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
    {: .fs-4 }
    {: .lh-0 }
ex) replace('a', '')라고 하면 'a'를 다 없애주는 기능. (두번째 '' 안을 비워두면 됨)  
    {: .fs-3 } 

    ```python
    s = 'python, is, important,'
    print(s.replace(',', ''))
    ```
    ```
    python is important
    ```

1. **find()**: 해당 단어가 존재하면 첫번째 character의 index number를 출력 / 존재하지 않으면 -1을 출력
```python
s = 'Data science is important'
print(s.find('science'))  # 첫글자 's'의 index number 출력
print(s.find('python'))   # 존재하지 않으므로 -1 출력
```
```
5
-1
```
cf) 존재 유무만을 확인하고 싶다면 find 대신 in을 사용해도 된다
```python
s = 'Data science is important'
print('science' in s)
print('python' in s)
```
```
True
False
```

    <div class="code-example" markdown="1">

    +) find를 사용하는 상황 예시

    ```python
    data = 'From stephen.marquard@uct.ac.za Sat Jan   5 09:14:16 2008' 
    # 이 데이터에서 보낸 사람의 메일 도메인만을 추출하고 싶음

    atpos = data.find('@')  # '@'의 위치를 알아냄
    print(atpos)

    sppos = data.find(' ', atpos) 
    # ' '(빈칸)이 몇 번째에 있나 atpos, 즉 21번째 문자 뒤에서부터 탐색 (atpost 앞의 빈칸은 무시)
    print(sppos)

    host = data[atpos+1 : sppos]  
    # '@' 뒤부터 그 다음 나오는 ' '(빈칸)까지를 slicing해서 메일 도메인만 추출
    print(host)
    ```

    ```
    21
    31
    uct.ac.za
    ```

    </div>


1. **upper()**, **lower()**, **capitalize()**
- `upper()`: 모든 문자를 uppercase로 바꿔줌
- `lower()`: 모든 문자를 lowercase로 바꿔줌
- `capitalize()`: 문자열의 가장 첫번째 문자는 uppercase로, 나머지는 lowercase로 맞춰줌

    ```python
    # 숫자/특수기호에는 반영되지 않고, 알파벳에만 적용됨
    a = 'hAve a Nice daY 12#'

    print(a.upper())
    print(a.lower())
    print(a.capitalize())
    ```
    ```
    HAVE A NICE DAY 12#
    have a nice day 12#
    Have a nice day 12#
    ```



1. **startswith()**, **endswith()**
- `startswith()`: 문자열이 ()안의 특정 문자로 시작되는지 확인, 결과는 boolean 값으로 반환
    -- `if X.startswith('Y')`: 이런 식으로 if문에서 주로 사용
- `endswith()`: 문자열이 특정 문자로 끝나는지 여부를 판별

    ```python
    line = 'Have a nice day'
    print(line.startswith('Have')) # True
    print(line.startswith('H')) # True
    print(line.startswith('h')) # 대문자 H와 소문자 h는 다르기 때문에, False가 반환됨

    print(line.endswith('day'))
    ```
    ```
    True
    True
    False
    True
    ```

1. **isalpha()**, **isalnum()**, **isdigit()**
- `isalpha()`: 문자열이 모두 문자로만 이루어져 있는지 판별 (영어/한글 등 언어는 상관X)
- `isalnum()`: 문자열이 모두 문자/숫자로만 이루어져 있는지 판별. 특수문자/공백 등이 함께 들어 있으면 False를 return
- `isdigit()`: 문자열이 모두 숫자로만 이루어져 있는지 판별.

    ```python
    line1 = 'Python코딩은즐거워'
    line2 = 'start123'
    line3 = '12345'
    line4 = 'Python 코딩'

    print(line1.isalpha())
    print(line2.isalpha())
    print(line4.isalpha())  # 공백만 하나 있어도 False가 됨

    print(line1.isalnum())
    print(line2.isalnum())
    print(line3.isalnum())

    print(line2.isdigit())
    print(line3.isdigit())
    ```
    ```
    True
    False
    False
    True
    True
    True
    False
    True
    ```

1. **count()**: list의 element를 세는 것과 동일하게 적용됨

    ```python
    x = 'python python'
    x.count('python')
    ```
    ```
    2
    ```



### String - Number Conversion
1. **int(x)**: from string/float to integer
    ```python
    # string -> integer
    x = '123'  # 이렇게 '' 안이 integer여야만 int(x)로 변환 가능
    print(x, type(x))

    y = int(x)
    print(y, type(y))
    ```
    ```
    123 <class 'str'>
    123 <class 'int'>
    ```

1. **float(x)**: from string to float
    ```python
    # string -> float -> integer
    x = '123.123'  # 이렇게 '' 안이 float이면, int(x)를 바로 할 수 없음. 먼저 float(x)를 해줘야 함.
    print(x, type(x))

    y1 = float(x)
    print(y1, type(y1))

    y2 = int(y1)  # 이제 y1은 float이기에, 여기에 int(x)를 해주면 integer가 됨.
    print(y2, type(y2))
    ```
    ```
    123.123 <class 'str'>
    123.123 <class 'float'>
    123 <class 'int'>
    ```

1. **str(number)**: from number to string
    ```python
    # integer -> string
    z = 123
    print(z, type(z))

    s = str(z)
    print(s, type(s))
    ```
    ```
    123 <class 'int'>
    123 <class 'str'>
    ```

### 문자열 포맷팅 (string formatting)
1. 'format' method
    ```python
    print("오늘은 {}월 {}일입니다".format(11, 12))
    ```
    ```
    오늘은 11월 12일입니다
    ```
    *{ }의 순서 지정하기
    ```python
    # 'format' method - {}의 순서 정하기
    print("저는 {1}, {0}, {2}를 좋아합니다".format("트와이스", "유재석", "비틀즈"))
    ```
    ```
    저는 유재석, 트와이스, 비틀즈를 좋아합니다
    ```
    *소수점 제한 지정하기
    ```python
    # 'format' method - 소수점 제한 지정
    print("{0} 나누기 {1}은 {2:.2f}입니다".format(1, 3, 1/3))  
    # :.2f라고 하면 floating point(소수) 둘째자리까지 출력하라는 뜻
    ```
    ```
    1 나누기 3은 0.33입니다
    ```

    <div class="code-example" markdown="1">
    - `:.4f`는 소수점 넷째짜리까지 출력하라는 뜻
    - `:.0f`는 정수로 출력하라는 뜻 (cf. 정수로 하려면 `:d`라고 해도 됨)
    - `:f`라고 하면 그냥 소수점 제한 없이 floating point로 출력하라는 뜻
    {: .fs-3 }
    </div>


1. % 기호
- 오래된 방식. C/자바 등 언어의 문자열 포맷팅 방식과 유사
- python에서는 권장되지는 않는다 (f-string 등 더 효율적인 방법이 있기 때문)
- %s, %d와 같은 '포맷 스트링'을 사용

    ```python
    name = '최지아'
    age = 25
    print("제 이름은 %s이고 %d살입니다." % (name, age))
    ```
    ```
    제 이름은 최지아이고 25살입니다.
    ```

    <div class="code-example" markdown="1">
    - `%s`: str형
    - `%d`: int형
    - `%f`: float형
    - `%.[숫자]f`: float형 (숫자를 통해 소수점을 지정)
    {: .fs-3 }
    </div>

1. f-string (파이썬 3.6부터 나온 방식)
    ```python
    name = '최지아'
    age = 25
    print(f"제 이름은 {name}이고 {age}살입니다.")
    ```
    ```
    제 이름은 최지아이고 25살입니다.
    ```