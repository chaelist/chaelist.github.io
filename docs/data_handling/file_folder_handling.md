---
layout: default
title: 파일 & 폴더 다루기
parent: Data Handling
nav_order: 2
---

# 파일 & 폴더 다루기
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

***사용할 모듈: os, shutil, zipfile (모두 별도의 설치 없이 import해서 사용 가능)**
```python
import os
import shutil
import zipfile
```

## 조회 & 정보 탐색

### 운영체제 & 경로

1. 현재의 운영체제 확인

    ```python
    os.name
    ```
    - windows면 'nt', macOS나 Linux라면 'posix'라고 출력됨

1. working directory 확인 (현재 이 코드 파일이 존재하는 directory 위치)

    ```python
    print(os.getcwd())   # 아래와 같이 절대 경로로 표현되어 나온다
    ```
    ```
    C:\Users\admin\Desktop\python
    ```
    - windows면 \로, macOS면 /로 구분된다

1. 특정 파일/폴더의 절대 경로 확인하기

    ```python
    print(os.path.abspath('Python 기초'))
    ```
    ```
    C:\Users\admin\Desktop\python\Python 기초
    ```

1. 현재 운영체제에서 사용하는 경로 구분 기호 확인

    ```python
    print(os.path.sep)
    ```
    - windows면 \, macOS면 /가 출력됨

1. os.path.join을 사용한 경로 구분
- os.path.join을 사용하면 내가 사용하는 운영체제에 알맞게 구분 기호를 붙여서 경로를 만들어줌 (운영체제별 경로 구분 구애받지 않고 사용하는 방법)

    ```python
    path = os.path.join('User', 'Folder', 'image.jpg')
    print(path)
    ```
    ```
    User\Folder\image.jpg
    ```

1. 경로 검증하기 (실제 해당 경로에 해당 파일/폴더가 존재하는지 확인)

    ```python
    # 파일 / 폴더 모두 동일하게 검증 가능
    print(os.path.exists('Python 기초'))
    print(os.path.exists('없는 파일.png'))
    ```
    ```
    True
    False
    ```
    - 경로를 다룰 때에는, `if os.path.exists('path'):`처럼 if문으로 경로의 존재 여부를 먼저 검증하고 그 다음 코드를 써주면 더 안전하다

&nbsp;
{: .fs-1 .lh-0}

+) 경로 관련 정보:
- windows의 경우, 단순히 '\\'로만 경로를 구분하기보다, '\\\\' 이렇게 두 번 써주는 게 좋다.   
    -- ex) 'user\nine' 이런 경우에는 '\n' = 엔터로 인식되어버리기 때문.
- 아무것도 쓰지 않거나 '.'이라고 쓰면 working directory를 의미
- '..'이라고 쓰면 wd의 상위 디렉토리를 의미, '../..'라고 쓰면 한 번 더 거슬러 올라간 상위 디렉토리를 의미


### 파일/폴더 정보 탐색

1. 용량 확인
- 파일 용량: 컴퓨터의 저장장치(HDD, SSD)에 차지하는 공간의 양

    ```python
    os.path.getsize('파일/폴더명')
    ```
    ```
    4096
    ```
    - 이 때 출력되는 값은 byte 단위.
    - 1kB = 1000byte, 1MB = 1000kB, 1GB = 1000MB

1. 확장자 정보 분리
- 일반적인 파일명의 경우 '.'은 잘 안쓰니까 split('.')으로 나눠도 되지만, os.path.splittext를 사용하면 아래와 같은 특수 경우에도 안전하게 확장자 정보를 얻을 수 있음

    ```python
    file_ex = 'quarter.report.pdf'
    filename, extension = os.path.splitext(file_ex)
    print('파일명:', filename)
    print('확장자:', extension)
    ```
    ```
    파일명: quarter.report
    확장자: .pdf
    ```

1. 생성한 날짜 확인

    ```python
    import datetime

    birthtimestamp = os.path.getctime('Python/test.txt')  # 파일/폴더에 모두 적용 가능
    birthtime = datetime.datetime.fromtimestamp(birthtimestamp)
    print(birthtime)
    ```
    ```
    2020-04-01 00:13:57.907304
    ```
    - `os.path.getctime('path')`의 결과는 timestamp 형태로 return되므로, `datetime.datetime.fromtimestamp`를 사용해 변환해줘야 한다  
    (*timestamp: 1970년 1월 1일부터 지난 시간을 초로 환산한 것)
    - `os.stat('path').st_ctime`을 사용해도 동일한 결과

1. 마지막으로 수정한 날짜 확인

    ```python
    mtimestamp = os.path.getmtime('Python/test.txt')
    mtime = datetime.datetime.fromtimestamp(mtimestamp)
    print(mtime)
    ```
    ```
    2021-04-01 14:12:23.278385
    ```
    - `os.stat('path').st_atime`을 사용해도 동일한 결과

1. 마지막으로 access한 날짜 확인

    ```python
    atimestamp = os.path.getatime('Python/test.txt')
    atime = datetime.datetime.fromtimestamp(atimestamp)
    print(atime)
    ```
    ```
    2021-04-01 14:09:57.907304
    ```
    - `os.stat('path').st_mtime`을 사용해도 동일한 결과



### 파일/폴더 조회

1. 특정 directory 내 모든 element 조회

    ```python
    os.listdir('.')   # walking directory를 조회할 경우, os.listdir()이라고만 써도 됨
    ```
    ```
    ['Python 기초', '데이터 사이언스', '파일 자동화.ipynb']
    ```
    - `os.listdir('path')`를 활용하면 해당 경로 내의 모든 파일/폴더 정보가 list로 반환됨

1. 특정 경로 내의 모든 폴더/파일 돌아보기
- 경로에 들어 있는 모든 폴더와 파일을 조회해주고, 또 그 안의 폴더의 폴더와 파일을 조회해주고..... (더 이상 조회할 폴더가 없을 때까지 반복)

    ```python
    for path, dirs, files in os.walk('datascience_intro'):
        print('Path:', path)
        print('Folders:', dirs)
        print('Files:', files)
        print('-----------')
    ```
    ```
    Path: datascience_intro
    Folders: ['.ipynb_checkpoints', 'data']
    Files: ['Exploratory Data Analysis.ipynb', 'Pandas 기초.ipynb', '데이터 정제.ipynb']
    -----------
    Path: datascience_intro\.ipynb_checkpoints
    Folders: []
    Files: ['데이터 정제-checkpoint.ipynb']
    -----------
    Path: datascience_intro\data
    Folders: ['empty_folder']
    Files: ['iris.csv', 'test_scores.csv', 'titanic.csv']
    -----------
    Path: datascience_intro\data\empty_folder
    Folders: []
    Files: []
    -----------
    ```
    - `os.walk('path')`는 path, folders, files 순서로 3개의 element를 반복적으로 return해줌  


### 폴더/파일 여부 판별

```python
# dir인지 판별
print(os.path.isdir('Python/test.txt'))
print(os.path.isdir('Python'))
print(os.path.isdir('.ipynb_checkpoints'))

# 파일인지 판별
print(os.path.isfile('Python/test.txt'))
print(os.path.isfile('파일 자동화.ipynb'))
print(os.path.isfile('Python'))
```
```
False
True
True
True
True
False
```
- ※ 주의: 이름이 틀린 경우(해당 경로가 존재하지 않는 경우)에도 에러가 나지는 않고, 그냥 False가 반환됨!

## 복사 & 이동 & 삭제

1. os.rename: 이름 변경 / 경로 이동

    1. 단순히 이름만 바꿔서 써주면 그대로 같은 dir 내에 이름만 바뀌어서 저장됨
    ```python
    os.rename('기존이름', '새이름')
    ```

    1. 경로 자체를 바꿔서 써주면 위치도 새 경로로 이동시킬 수 있음
    ```python
    os.rename('기존경로/기존이름.txt', '새경로/새이름.txt')
    ```

    +) shutil.move: 파일 이동 (os.rename과 거의 동일하게 동작)

    ```python
    shutil.move('기존경로/기존이름.txt', '새경로/새이름.txt')
    ```
    - os.rename과의 차이: shutil.move를 활용하면 다른 드라이브 / 다른 파일 시스템을 갖는 위치로도 이동 가능 (ex. C드라이브에서 D드라이브로 이동)
    - 같은 드라이브 내에서 이동하는 경우, os.rename만 사용해도 충분

1. shutil.copy: 파일 복사

    ```python
    shutil.copy('원본 파일 이름', '복제본 파일 이름')
    ```
    - 물론 shutil 모듈 없이도, 그냥 `open`으로 복제하는 것도 가능:
        - read 모드로 복제할 파일을 읽어와서 f.read()로 전체 내용을 저장 후, 다시 write 모드로 새 파일을 만들어서 f.write()으로 저장햐둔 내용을 적어주면 됨
    
    ※ 주의: '복제본 파일 이름'과 같은 경로/이름의 파일이 이미 존재할 경우, 새 파일이 기존 파일을 덮어씌우게 된다 (if os.path.extist('path')로 검증하고 복제하도록 코드를 적으면 더 안전)

1. shutil.copytree: 폴더 복사

    ```python
    shutil.copytree('기존폴더명', '새폴더명')
    ```

1. os.remove: 파일 삭제

    ```python
    os.remove('파일명.txt')
    ```

1. 폴더 삭제

    1. 빈 폴더 삭제
    ```python
    os.rmdir('폴더명')
    # os.rmdir를 사용하면 내용물이 없는 폴더만 삭제 가능하다
    # 내용물이 있는 폴더를 os.rmdir로 삭제하려고 하면 error
    ```

    1. 내용물이 있는 폴더 삭제
    ```python
    shutil.rmtree('폴더명')
    ```

1. 폴더 생성

    1. os.makedirs('폴더명'): 여러 디렉토리를 한 번에 생성하는 것도 가능
    ```python
    os.makedirs(r'years/2021') 
    # 현재 경로에 years라는 폴더조차 없는 상황 → 이 코드를 실행하면 years 디렉토리가 생기고 그 안에 2021 디렉토리도 함께 생김
    ```

    1. os.mkdir('폴더명'): 하나의 디렉토리만 생성 가능

        ```python
        os.mkdir(r'years/2021') 
        # 현재 경로에 years라는 폴더가 없다면 이 코드는 error를 발생시킴

        os.mkdir('2021')
        # 위와 같은 코드는 정상적으로 실행됨 (폴더 하나만 생성)
        ```

## 압축 파일 생성

### shutil 활용

1. 하나의 폴더 내 내용물을 한번에 압축

    ```python
    shutil.make_archive('archive', 'zip', 'folder') # 두 번째 인자로는 압축 방식을 넣어주면 됨
    
    # folder라는 폴더의 모든 내용물이 archive.zip이라는 압축파일로 저장됨
    ```
    - 

1. 압축 해제

    ```python
    shutil.unpack_archive('archive.zip', 'unpacked_folder')

    # archive.zip 압축파일의 내용물이 unpacked_folder라는 폴더로 압축해제가 되어 저장됨
    ```


### zipfile 활용

1. 각각의 파일을 선별해서 압축

    ```python
    with zipfile.ZipFile('archive.zip', 'w', compression=zipfile.ZIP_DEFLATED) as z:
        z.write('압축할 파일 1')
        z.write('압축할 파일 2')
        z.write('압축할 파일 3')
    
    # 파일 1~3이 archive.zip이라는 압축파일로 저장됨
    ```
    - `compression=zipfile.ZIP_DEFLATED`을 안쓰면 파일 1~3이 zip 파일로 합쳐지긴 하지만, 용량이 줄지는 않음

1. 압축 해제

    ```python
    with zipfile.ZipFile('archive.zip', 'r') as z:
        z.extractall('unpacked_folder')

    # archive.zip 압축파일의 내용물이 unpacked_folder라는 폴더로 압축해제가 되어 저장됨
    ```