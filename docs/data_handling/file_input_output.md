---
layout: default
title: 파일 읽고 쓰기
parent: Data Handling
nav_order: 1
---

# 파일 읽고 쓰기
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

## Accessing a file
- 우선 `open('file_name', 'mode')`로 파일에 access
- 'mode'의 종류: ‘r’: read, ‘w’: write, ‘a’: append

```python
# 파일이 같은 폴더에 있을 경우
f = open('example.txt', 'r')  ## 이렇게 파일명만 적어주면 됨

# 파일이 다른 폴더에 있을 경우
f = open('C:/Users/admin/Desktop/chaelist_blog/example.txt', 'r')  ## 절대 경로로 접근

# 파일이 현재 python 파일을 실행시킨 폴더 내 다른 폴더에 있을 경우
f = open('data/example.txt', 'r')  ## data라는 폴더 내에 파일이 있는 경우

# 파일이 현재 python 파일을 실행시킨 폴더 밖의 폴더에 있을 경우
f = open('../example.txt', 'r')  ## ../를 통해 traverse up the directory
```

## Reading a file

### f.read()
: 파일 전체를 하나의 string value로 읽어준다
```python
f = open('example.txt', 'r')   # 'r'은 default mode이기 때문에 안써줘도 결과는 동일
f.read()   # 아래처럼, 하나의 string value로 읽어옴
```
```
'1\n2\n3\n4\n5'
```
```python
f.close()  # 특정 파일을 open한 다음 read / write하고 나면 닫아줘야 한다
```

### f.readlines()
: read the entire file, as a list type (각 줄을 각각의 list variable로 받아서, 하나의 list로 반환)
- 가장 흔히 사용하는 file reading method.

```python
f = open('example.txt', 'r')
f.readlines()   # 각 줄을 element로 하는 list를 반환
```
```
['1\n', '2\n', '3\n', '4\n', '5']
```
```python
f.close()
```


### f.readline()
: read the file line by line
- 한줄씩 차례대로 readline해줘야 해서, 잘 안 쓰는 method.

```python
f = open('example.txt', 'r')
f.readline()   # 이런 식으로 한줄씩 읽힌다
```
```
'1\n'
```
```python
f.readline()   # 이런 식으로 한줄씩 읽힌다
```
```
'2\n'
```
```python
f.close()
```


### 활용 Tip
1. 보통, readlines()에 for문을 결합해서 이용한다.
    ```python
    f = open('example.txt', 'r')
    for line in f.readlines():
        print(line.strip())   # 보통 이렇게 for문을 사용해서 한 줄 한 줄 접근

    f.close()
    ```
    ```
    1
    2
    3
    4
    5
    ```

1. txt 파일에서 읽어온 값들은 일단 다 string 형태로 저장된다.
    ```python
    f = open('example.txt', 'r')
    lines_list = f.readlines()  # 이렇게 저장해두면 불러오기 편하다
    f.close()

    line = lines_list[0].strip()

    print(line, type(line))  # type을 출력해보면, txt 파일에서 읽어온 value들은 다 일단 string 형태.

    num_line = int(line)  # '' 사이의 값이 숫자일 경우, 이렇게 int()를 적용해  숫자로 바꿔줄 수 있다
    print(num_line, type(num_line))  # 이제 type이 integer로 바뀌어서, 사칙연산 가능
    ```
    ```
    1 <class 'str'>
    1 <class 'int'>
    ```


## Writing a file
- `f.write('내용')`의 형태로 작성
- 다 쓴 file은 close해주어야 제대로 정보가 저장된다

```python
# 예시
f1 = open('name.txt', 'w') ## 해당 python 파일이 실행된 동일한 폴더에 저장됨
f1.write('Chung & Cho')  # 쓰고 싶은 내용을 작성
f1.close()
```

1. write하는 내용은 무조건 string value여야 한다!

    ```python
    f2 = open('numbers.txt', 'w')
    num = 3
    f2.write(num)  # 이렇게 int 형태의 데이터를 write하려고 하면 error
    f2.close
    ```
    ```
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-15-7b27a5c24a65> in <module>()
        1 f2 = open('numbers.txt', 'w')
        2 num = 3
    ----> 3 f2.write(num)
        4 f2.close

    TypeError: write() argument must be str, not int
    ```
    cf) int 형태의 데이터는 str(num)으로 string으로 바꿔주면 write 가능
    ```python
    f2 = open('numbers.txt', 'w')
    num = 3
    f2.write(str(num))
    f2.close()
    ```

1. 한글 처리 방법: `encoding='utf-8'`
```python
f3 = open('한글.txt', 'w', encoding='utf-8')  # encoding 방법을 지정 가능
f3.write('김태희')
f3.close()
```

1. 여러 내용 이어서 write
    ```python
    f_test = open('test.txt', 'w')
    f_test.write('test1'+'\n')
    f_test.write('test2')  # 이런식으로 이어서 f.write()를 또 해주면 계속 이어서 써짐
    ## f.close()를 해주기 전까지는 계속해서 한 줄씩 추가 가능

    f_test.close()  
    ```

1. 이미 만든 파일에 <u>append</u>하기 (내용 추가로 더 쓰기)
```python
f_test = open('test.txt', 'a')  # 'a': append
f_test.write('\n'+'test3') # 'a'로 open한다음에 write 해주면 이미 존재하는 파일에 이어서 쓸 수 있음.
f_test.close() 
```
※ `open('이미 있는 파일명', 'w')` 이렇게  이미 있는 파일을 'w'(write)로 열면 그 위에 overwrite하게 되기 때문에, 이어서 계속 쓰고 싶다면 'a'(append)로 열어줘야 한다
