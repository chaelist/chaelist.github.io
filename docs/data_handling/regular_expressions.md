---
layout: default
title: Regular Expressions
parent: Data Handling
nav_order: 2
---

# Regular Expressions
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

## 정규 표현식(Regular Expressions)
: 메타 문자(meta characters)를 적절히 활용해 복잡한 문자열을 처리하는 기법
- 메타 문자: . ^ $ * + ? { } [ ] \ \| ( )

### [ ]: 문자 클래스(character class)
: <i>"[ ] 사이의 문자들과 매치"</i>라는 의미

1. `[abc]`: "a, b, c 중 한 개의 문자와 매치"라는 의미
- "a"는 정규식과 일치하는 문자인 "a"가 있으므로 매치되는 부분이 존재
- "before"는 정규식과 일치하는 문자인 "b"가 있으므로 매치되는 부분이 존재
- "dude"는 정규식과 일치하는 문자 "a", "b", "c" 중 어느 하나도 포함하지 않으므로 매치되지 않음

1. \[ \] 안의 두 문자 사이에 하이픈(-)을 이용하면 두 문자 사이의 범위(from - to)를 의미
- `[a-z]`: a~z의 모든 알파벳
- `[0-5]`: 012345
- `[a-zA-Z]`: 알파벳 모두
- `[0-9]`: 숫자 모두

1. \[ \] 안에 ^를 넣으면, 반대(not)의 의미가 됨
- `[^0-9]`: 숫자가 아닌 문자만 매치됨

### 자주 사용하는 문자 클래스
: 아래와 같이 자주 사용하는 정규식은 별도의 표기법으로 표현이 가능하다
- `\d` - 숫자와 매치. [0-9]와 동일
- `\D` - 숫자가 아닌 것과 매치. [^0-9]와 동일
- `\s` - whitespace character와 매치, [ \t\n\r\f\v]와 동일 (맨 앞의 빈 칸은 공백(space)를 의미)
- `\S` - non-whitespace character과 매치, [^ \t\n\r\f\v]와 동일
- `\w` - alphanumeric(문자+숫자) & underscore(\_)와 매치, [a-zA-Z0-9_]와 동일
- `\W` - alphanumeric(문자+숫자) & underscore(\_)가 아닌 것과 매치, [^a-zA-Z0-9_]와 동일

(※ 각각, 대문자로 사용된 것은 소문자의 반대)


### 각 표현식의 의미

1. **Dot(.)**: 줄바꿈 문자인 \n을 제외한 모든 문자와 매치됨을 의미
- `a.b`: "a + 모든 문자 + b"라는 뜻
  - "aab", "a0b" 모두 `a.b`와 매치된다
  - "abc"는 a와 b 사이에 어떤 문자도 없으므로 매치되지 않는다
- cf) `a[.]b`: "a + . + b"라는 뜻. "a.b"와만 매치된다


1. **반복(*)**: 0번 이상 반복된다는 의미
- `ca*t`: * 바로 앞의 문자 'a'가 0번 이상 반복된다는 뜻 (a는 없어도 됨)
  - "ct", "cat", "caaat" 모두 `ca*t`와 매치된다 (0번도 가능)


1. **반복(+)**: 1번 이상 반복된다는 의미
- `ca+t`: + 바로 앞의 문자 'a'가 1번 이상 반복된다는 뜻 (무조건 a가 있어야 함)
  - "cat", "caaat"은 `ca*t`와 매치, 하지만 "ct"처럼 a가 한 번도 없는 것은 매치되지 않는다


1. **{m,n}**: m~n번 반복된다는 의미 (반복 횟수를 제한)
- `ca{2,5}t`: {m,n} 바로 앞의 문자 a가 2번~5번 반복된다는 뜻
  - "caat", "caaat", "caaaat", "caaaaat" 모두  `ca{2,5}t`와 매칭된다


1. **?**: 있어도 되고 없어도 된다는 의미 (0번 or 1번 있으면 됨)
- `ab?c`: "a + b(있어도 되고 없어도 됨) + c"라는 뜻
  - "abc", "ac" 모두 매치된다
  - "abbc" 등 b가 2번 이상이면 매치되지 않는다


1. **^**: Matches the beginning of a line
- `^X`라고 하면 X로 시작하는 아이들만 매치됨
  - "Xylophone"은 `^X`와 매칭된다 (case sensitive하므로 반드시 대문자 X여야 매칭)


1. **$**: $: Matches the end of a line
- `X$`라고 하면 X로 끝나는 아이들만 매치됨
  - "AdEX"는 `X$`와 매칭된다


1. **( )**: Indicates where string extraction is to start & to end
- `^From (\S+@\S+)`라고 하면, 'From'으로 시작하는 `\S+@\S+`들을 찾아주되, ( ) 안의 부분만 extract.
  - "From chaelist@github.com"이라는 string이 있다면, 이 중 "chaelist@github.com"만 extract.


## re 모듈
: 파이썬에서 정규 표현식을 지원하는 re(regular expression) 모듈

### re.search()
: 특정 string이 해당 regular expression과 매칭되는지 여부에 따라 True/False를 return
- if문과 함께 사용해서 원하는 string들만 읽어올 때 사용하면 유용함

```python
import re  # import해줘야 사용 가능

lines = ['From chaelist@github.com', 'hi', 'From me', 'nice to meet you', 'From CYC']

for line in lines: 
  if re.search('^From', line):     ## if line.starswith('From'): 과 동일
    print(line)
```
```
From chaelist@github.com 
From me
From CYC
```

### re.findall()
: 해당 regular expression에 매칭되는 portions of string을 extract. 
- 매치되는 부분을 모두 리스트로 저장해 return

*(※ `re.search()` returns a <u>True/False</u>, `re.findall()` returns the <u>matching strings</u>)*
{: .text-grey-dk-000 }

```python
x = 'My 2 favorite numbers are 19 and 42'

y = re.findall('[0-9]+', x)  # 숫자(0-9)가 1번 이상 반복되는 부분을 모두 찾으므로, [2, 19, 42]가 return됨
print(y)

z = re.findall('[AEIOU]+', x)  # A, E, I, O, U 중 하나가 1번 이상 반복되는 부분을 찾는데, 없으므로 빈 리스트 return
print(z)
```
```
['2', '19', '42']
[]
```

### re.sub()
: 패턴에 일치되는 문자열을 다른 문자열로 바꿔준다
- `re.sub(pattern, repl, string)`의 형태로 사용

```python
# re.sub() 간단한 버전
print(re.sub('\d{4}', 'XXXX', '010-1256-9999'))
```
```
010-XXXX-XXXX
```
+) count 추가:
```python
# re.sub(pattern, repl, string, count)
print(re.sub(pattern='Hello', repl='Bye', count=2, string='Hello, Hello, Hello Everybody.'))
## 일치하는 문자열이 3개이지만 count=2로 한정되어 있으면 딱 2개까지만 변환됨
```
```
Bye, Bye, Hello Everybody.
```
+) **re.subn()**: re.sub()와 매우 유사하지만, 치환된 문자열과 함께 치환된 개수도 return해준다
```python
print(re.subn('\d{4}', 'XXXX', '010-1234-5678')) ## 치환 결과와 치환된 개수를 element로 하는 tuple 반환
```
```
('010-XXXX-XXXX', 2)
```



### re.compile()
: 특정 정규표현식을 컴파일해두고 사용하는 방식

```python
x = 'My 2 favorite numbers are 19 and 42'

y = re.findall('[0-9]+', x)
print(y)

p = re.compile('[0-9]+')  # 이렇게 re.compile을 통해 특정 정규표현식을 저장해두고, 
m = p.findall(x)          # 여기에 .findall('문자열')을 해줘도 re.findall('정규표현식', '문자열')과 동일.
print(m)
```
```
['2', '19', '42']
['2', '19', '42']
```

※ 결론: 아래 1번과 2번은 완전히 동일한 결과를 낸다
```python
# 1번
result = re.findall('정규표현식', '문자열')

# 2번
p = re.compile('정규표현식')
result = p.findall('문자열')
```

## 추가 활용 Tip

1. ( ) 사용해서 더 precise하게 찾기
  ```python
  x = 'From stephan.marquard@uct.ac.za Sat Jan  5 09:14:26 2008'
  y = re.findall('^From (\S+@\S+)', x)  
      ## From으로 시작(^) & space & 1개 이상(+)의 Non-whitespace character(\S) & @ & 1개 이상(+)의 Non-whitespace character(\S)
      ## 이 중, () 안에 있는 부분만 추출한다. ('From '는 추출X)
  print(y)
  ```
  ```
  ['stephan.marquard@uct.ac.za']
  ```

1. 주의할 점: Greedy Matching
- The repeat characters (* and +) push outward in both directions (greedy) to match the largest possible string (*나 + 같은 '반복' 문자를 쓰면, 가능한 가장 크게 매칭되려는 경향이 있다)
- +?, *? 이런 식으로 ?를 뒤에 붙여주면 non-greedy matching이 가능

    *Greedy Matching 예시
    {: .text-grey-dk-000 .fs-3}
    ```python
    x = 'From: Using the : character'
    y = re.findall('^F.+:', x)
    print(y)  # From:도 매칭되고, 이를 포함하는 From: Using the :도 매칭될 때, 범위가 더 큰 후자로 매칭되게 됨.
    ```
    ```
    ['From: Using the :']
    ```

    *Non-Greedy Matching 예시
    {: .text-grey-dk-000 .fs-3}
    ```python
    x = 'From: Using the : character'
    y = re.findall('^F.+?:', x)
    print(y)  # +?로 해주면 non-greedy한 매칭방식이므로, 가장 빨리 찾아지는 From:을 return하고 끝내게 됨
    ```
    ```
    ['From:']
    ```

1. Escape Character (예외 문자)
- 정규식에서 쓰는 문자인데 그냥 실제 그 모양 그대로의 의미를 담고 싶으면, \를 앞에 붙여주면 된다
- ex) $(dollar sign) 그 자체를 써야 한다면, `\$` 이렇게 표시.

    ```python
    x = 'We just received $10.00 for cookies.'
    y = re.findall('\$[0-9]+', x)  # \$: 실제 $ 사인과 매칭되는 것을 찾으라는 의미
    print(y)
    ```
    ```
    ['$10']
    ```

1. 정규표현식을 테스트할 수 있는 사이트: [https://regexr.com/](https://regexr.com/){: target="_blank"}
