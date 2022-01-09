---
layout: default
title: Datetime 다루기
parent: Data Handling
nav_order: 5
---

# Datetime 다루기
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

## datetime 객체 생성

1. 직접 정보를 입력해 생성

    ```python
    import datetime as dt  # import해서 사용. 별도의 설치는 필요 없다

    custom_date = dt.datetime(2020, 11, 12)  # 2020년 11월 22일
    print(custom_date)  # 시간 정보를 추가하지 않으면 자동으로 00시 00분 00초로 생성됨
    ```
    ```
    2020-11-12 00:00:00
    ```

    +) 시간 정보 추가:

    ```python
    custom_date = dt.datetime(2020, 11, 12, 10, 11, 12)
    print(custom_date)
    ```
    ```
    2020-11-12 10:11:12
    ```

2. 현재 datetime으로 생성

    ```python
    now = dt.datetime.now()
    print(now)
    ```
    ```
    2022-01-09 06:16:51.301662
    ```

    +) 시간까지 세세하게 가져오는 대신, 오늘의 날짜만 가져오기:


    ```python
    now = dt.date.today()
    print(now)
    ```
    ```
    2022-01-09
    ```


3. 특정 str 포맷으로부터 생성
- `datetime.datetim.strptime('str 형태의 날짜', '포맷')`의 구조로 작성

    ```python
    str_date = '2020-11-12'
    custom_date = dt.datetime.strptime(str_date, '%Y-%m-%d') 
    print(custom_date)
    ```
    ```
    2020-11-12 00:00:00
    ```


## datetime 정보 출력

1. 날짜, 시간 정보 추출

    ```python
    now = dt.datetime.now()
    print(now.date())
    print(now.time())
    ```
    ```
    2022-01-09
    06:16:51.301662
    ```

    +) 연도, 월, 일, 시간, 분, 초 모두 구분해서 추출:

    ```python
    print(now.year)
    print(now.month)
    print(now.day)
    print(now.hour)
    print(now.minute)
    print(now.second)
    ```
    ```
    2022
    1
    9
    6
    16
    51
    ```

1. 요일 정보 추출

    ```python
    print(now.weekday())  # 0: 월요일 ~ 6: 일요일
    print(now.isoweekday())  # ISO 달력 형식: 1: 월요일 ~ 7: 일요일
    ```
    ```
    6
    7
    ```

1. 특정 포맷으로 정보 출력하기

    ```python
    print('isoformat:', now.isoformat())  # yyyy-mm-dd'T'HH:mm:ss.SSSXXX 형태
    print('ctime:', now.ctime())  # 요일 월 일자 HH:mm:ss 연도 형태
    ```
    ```
    isoformat: 2022-01-09T06:16:51.301662
    ctime: Sun Jan  9 06:16:51 2022
    ```


1. 원하는 str 포맷으로 정보 출력하기
- strptime()과 반대의 개념
- `datetime_object.strftime('포맷')`의 구조로 작성

    ```python
    print(now.strftime('%Y.%m.%d'))
    print(now.strftime('%Y/%m/%d %H시 %M분 %S초'))
    ```
    ```
    2022.01.09
    2022/01/09 06시 16분 51초
    ```


### ※ strftime(), strptime() 포맷 코드

| 포맷코드 |	설명  | 예 |
|:--------:|:--------:|:----:|
|   %a  |   요일 줄임말  |   Sun, Mon, ... Sat  |
|   %A  |   요일  |   Sunday, Monday, ..., Saturday |
|   %w  |   요일을 숫자로 표시, 월요일~일요일, 0~6  |   0, 1, ..., 6 |
|   %d  |   일  |   01, 02, ..., 31 |
|   %b  |   월 줄임말  |   Jan, Feb, ..., Dec |
|   %B  |   월  |   January, February, …, December |
|   %m  |   숫자 월  |   01, 02, ..., 12 |
|   %y  |   두 자릿수 연도  |   01, 02, ..., 99 |
|   %Y  |   네 자릿수 연도  |   0001, 0002, ..., 2017, 2018, 9999 |
|   %H  |   시간(24시간)  |   00, 01, ..., 23 |
|   %I  |   시간(12시간)  |   01, 02, ..., 12 |
|   %p  |   AM, PM  |   AM, PM |
|   %M  |   분  |   00, 01, ..., 59 |
|   %S  |   초  |   00, 01, ..., 59 |
|   %Z  |   시간대  | (비어있음)<sup>1)</sup>, UTC, GMT |
|   %j  |   1월 1일부터 경과한 일수  |   001, 002, ..., 366 |
|   %U  |   1년중 주차, 일요일이 한 주의 시작으로  |   00, 01, ..., 53 |
|   %W  |   1년중 주차, 월요일이 한 주의 시작으로  |   00, 01, ..., 53 |
|   %c  |   날짜, 요일, 시간을 출력, 현재 시간대 기준  |   Sat May 19 11:14:27 2018 |
|   %x  |   날짜를 출력, 현재 시간대 기준  |   05/19/18 |
|   %X  |   시간을 출력, 현재 시간대 기준  |   '11:44:22' |

<sup>1)</sup> naive datetime object의 경우(timezone 정보가 들어 있지 않은 경우), 아무것도 출력되지 않는다


## datetime 연산

1. datetime 간 비교
- datetime object는 int처럼 >, <, ==를 사용해 비교할 수 있다

    ```python
    date1 = dt.datetime(2020, 11, 12)
    date2 = dt.datetime(2021, 1, 19)

    if date1 > date2:
    print('date1이 더 나중')
    elif date1 < date2:
    print('date2가 더 나중')
    ```
    ```
    date2가 더 나중
    ```

1. datetime - datetime
- 두 시점 사이의 기간이 timedelta 형태로 저장됨

    ```python
    period = date2 - date1  # 두 시점 사이의 기간이 timedelta object로 저장됨

    print(period)
    print(type(period))
    print(period.days)
    ```

    +) datetime ± timedelta:

    ```python
    timedelta1 = dt.timedelta(days=2, hours=5)

    print(date1 + timedelta1)
    print(date1 - timedelta1)
    ```
    ```
    2020-11-14 05:00:00
    2020-11-09 19:00:00
    ```