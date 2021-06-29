---
layout: default
title: 데이터베이스 & 테이블 구축
parent: SQL
nav_order: 4
--- 

# 데이터베이스 & 테이블 구축
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

## 데이터베이스 생성
- CREATE DATABASE로 생성
    ```sql
    CREATE DATABASE 데이터베이스명;
    ```
- +) 이미 저장되어 있는 데이터베이스명으로 또 생성할 경우의 error 피하기:
    ```sql
    CREATE DATABASE IF NOT EXISTS 데이터베이스명;
    ```

## 테이블 생성
- CREATE TABLE로 생성
    ```sql
    CREATE TABLE `데이터베이스명`.`테이블명` (
        `id` INT NOT NULL AUTO_INCREMENT,
        `name` VARCHAR(50) NOT NULL,
        `follower_count` INT NULL,
        PRIMARY KEY (id));
    ```

- 테이블 역시, `CREATE TABLE IF NOT EXISTS`로 작성하면 이미 저장된 이름으로 중복 생성할 때의 error를 피할 수 있다
- 어떤 데이터베이스 내에 생성할지 명확히 표시했다면, `CREATE TABLE 테이블명`이라고만 적는 것도 가능
    ```sql
    CREATE TABLE IF NOT EXISTS `테이블명` (
        `id` INT NOT NULL AUTO_INCREMENT,
        `name` VARCHAR(50) NOT NULL,
        `follower_count` INT NULL,
        PRIMARY KEY (id));
    ```
- ※ backtick(`): 식별자(identifier)임을 나타내기 위해 사용하는 기호
    - identifier: 데이터베이스, 테이블, 칼럼 등의 object에 붙는 이름
    - 식별자를 `로 감싸주지 않아도 실행에는 문제가 없음. 이름임을 확실히 드러내고 싶어서 써주는 경우가 많을 뿐. 
    - cf) '(작은 따옴표)나 "(큰 따옴표)는 문자열 값을 나타낼 때 사용되니, `와 구분해서 사용

&nbsp;
{: .fs-1 .lh-0}

### 칼럼의 속성 
- PRIMARY KEY(PK): 해당 테이블에서 각 row를 식별할 수 있게 해주는 칼럼. 테이블별로 1개씩만 지정 가능하다
- NOT NULL(NN): 칼럼에 NULL값을 허용하지 않겠다는 제한 조건
    PRIMARY KEY는 반드시 NOT NULL이여야 한다
- AUTO_INCREMENT(AI): 따로 값을 입력해주지 않아도 자동으로 칼럼의 값이 1씩 늘어난다.   
    - PRIMARY KEY로 쓰는 칼럼(ex.id 칼럼)의 경우, AUTO_INCREMENT 속성을 달아주는 것이 편하다

&nbsp;
{: .fs-1 .lh-0}

### 칼럼의 데이터 타입

&nbsp;
{: .fs-1 .lh-0}

**1. Numeric types (숫자형 타입)**

1) 정수형 타입
- TINYINT: 작은 범위의 정수를 저장할 때 쓰는 데이터 타입
    - 최소 -128 ~ 쵀대 127까지의 정수를 저장 가능
    - ※ `SIGNED`: 양수-0-음수 / `UNSIGNED`: 0-양수를 의미
    - TINYINT SIGNED: -128 ~ 127
    - TINYINT UNSIGNED: 0 ~ 255
    - *그냥 TINYINT라고만 하면 SIGNED가 붙은 것으로 간주하는 것이 default.
- SMALLINT
    - SMALLINT SIGNED: -32768 ~ 32767
    - SMALLINT UNSIGNED : 0 ~ 65535
- MEDIUMINT 
    - MEDIUMINT SIGNED : -8388608 ~ 8388607
    - MEDIUMINT UNSIGNED : 0 ~ 16777215
- INT
    - INT SIGNED : -2147483648 ~ 2147483647
    - INT UNSIGNED : 0 ~ 4294967295
- BIGINT
    - BIGINT SIGNED : -9223372036854775808 ~ 9223372036854775807
    - BIGINT UNSIGNED : 0 ~ 18446744073709551615

2) 실수형 타입
- DECIMAL: 보통 DECIMAL(M, D)의 형식으로 나타내는데, M은 최대로 쓸 수 있는 전체 숫자의 자리수, D는 최대로 쓸 수 있는 소수점 뒤 자리수를 의미
    - ex) DECIMAL (5, 2)라면 -999.99 부터 999.99 까지의 실수가 가능
    - M은 최대 65, D는 30까지의 값을 가질 수 있다
    - DECIMAL이라는 단어 대신 DEC, NUMERIC, FIXED를 써도 된다
- FLOAT
    - -3.402823466E+38 ~ -1.175494351E-38, 0, 1.175494351E-38 ~ 3.402823466E+38  범위의 실수들을 나타낼 수 있는 데이터 타입
    - fyi) -3.402823466E+38 은 (-3.402823466) X (10의 38제곱), -1.175494351E-38 은 (-1.175494351) X (10의 38제곱 분의 1) 
- DOUBLE
    - -1.7976931348623157E+308 ~ -2.2250738585072014E-308, 0, .2250738585072014E-308 ~ 1.7976931348623157E+308  범위의 실수들을 나타낼 수 있는 데이터 타입


**2. 날짜 및 시간 타입 (Date and Time Types)**

- DATE: 날짜를 저장하는 데이터 타입 
    - `YYYY-MM-DD` 형식으로 표기 - ex. '2020-03-26'
- DATETIME: 날짜와 시간을 저장하는 데이터 타입
    - `YYYY-MM-DD HH:MM:SS` 형식으로 표기 - ex. '2020-03-26 09:30:27'
- TIMESTAMP: 날짜와 시간을 저장하는 데이터 타입
    - DATETIME과 마찬가지로 `YYYY-MM-DD HH:MM:SS` 형식으로 표기
    
    <div class="code-example" markdown="1">

    ※ DATETIME과 TIMESTAMP의 차이
    - TIMESTAMP는 타임 존(time_zone) 정보도 함께 저장한다는 점에서 DATETIME과 차별화된다
    - ex) `SET time_zone = '-11:00';` 이렇게 시간대를 UTC-11로 바꿔주면 DATETIME으로 저정한 시간은 그대로지만, TIMESTAMP로 저장한 시간은 바뀜 (TIMESTAMP로 저장할 당시에는 UTC+9 시간대로 저장됨)
    - *UTC (Coordinated Universal Time): 국제 사회에서 통용되는 표준 시간 체계로 '국제 표준시'라고도 함. 영국 런던이 기준. → 한국은 UTC+9 시간대

    </div>

- TIME: 시간을 나타내는 데이터 타입.
    - `HH:MM:SS`의 형식으로 표기 - ex. '09:27:31'


**3. 문자열 타입 (String type)**

- CHAR: 문자열을 나타내는 기본 타입으로, Character의 줄임말
    - `CHAR(30)` 이런식으로 표기하는데, 괄호 안의 숫자는 문자를 최대 몇 자까지 저장할 수 있는지를 의미    
    (30이라고 쓰면 최대 30자의 문자열을 저장할 수 있다는 뜻)
    - () 안에는 0부터 255까지의 숫자를 적을 수 있음
- VARCHAR: CHAR처럼 문자열의 최대 길이를 지정할 수 있는 문자열 타입 - ex. `VARCHAR(30)`
    - () 안에 최소 0부터 최대 65,535 (216 − 1)를 쓸 수 있다

    <div class="code-example" markdown="1">

    ※ CHAR와 VARCHAR의 차이
    - VARCHAR는 '가변 길이 타입' -- VARCHAR이라는 단어 자체가 가변 문자열(varying character)의 줄임말
    - CHAR는 '고정 길이 타입'
    - `CHAR(10)`은 어떤 길이의 문자열이 저장되더라도 항상 그 값이 10만큼의 저장 용량을 차지하는 반면, `VARCHAR(10)`의 경우 만약 값이 'Hello'와 같이 5자라면 저장 용량도 5만큼만 차지한다   
    (저장 용량이 설정된 최대 길이에 맞게 고정되는 게 아니라 실제 저장된 값에 맞게 최적화되는 것!)
    - 대신, VARCHAR 타입으로 값이 저장될 때는 해당 값의 사이즈를 나타내는 부분(1byte 또는 2byte)이 저장 용량에 추가된다
    → 값의 길이가 크게 변하지 않을 컬럼에는 CHAR 타입을 사용하고, 길이가 들쑥날쑥할 컬럼에는 VARCHAR 타입을 쓰는 게 좋다

    </div>

- TEXT: 문자열을 저장하는 데이터 타입으로 최대 65,535자까지 저장 가능
    - VARCHAR와 내부 구현 면에서 일부 차이가 있어서, 보통 길이가 긴 문자열은 TEXT계열의 타입으로 저장 (메시지, 댓글 등)
- MEDIUM TEXT: 16,777,215 (2<sup>24</sup> − 1) 자까지 저장 가능
- LONGTEXT: 4,294,967,295 (2<sup>32</sup> − 1) 자까지 저장 가능

&nbsp;
{: .fs-1 .lh-0}

## 테이블에 row 추가/업데이트/삭제

&nbsp;
{: .fs-1 .lh-0}

**1. 테이블에 row 추가**
- `INSERT INTO 테이블명 (칼럼명 나열) VALUES (추가할 값들 나열)`;의 구조로 작성

    ```sql
    INSERT INTO members
        (id, name, gender, email, phone, sign_up_date)
        VALUES (1, 'Chaelist', 'F', 'chaechae@chaelist.com', '010-1234-5678', '2015-05-22');
    ```

- 모든 칼럼에 값이 있는 row를 추가한다면 아래와 같이 칼럼명은 제외하고 적어도 된다:

    ```sql
    INSERT INTO members
        VALUES (1, 'Chaelist', 'F', 'chaechae@chaelist.com', '010-1234-5678', '2015-05-22');
    ```

- 일부 칼럼에만 값을 추가해줄 경우, 해당 row의 나머지 column은 NULL값으로 채워진다. (NOT NULL 제약이 없는 경우)

    ```sql
    INSERT INTO members
        (id, name, gender, sign_up_date)
        VALUES (1, 'Chaelist', 'F', '2015-05-22');
    ```

- AUTO_INCREMENT 속성을 갖는 칼럼 'id'의 경우, 값을 입력해주지 않아도 자동으로 이전 값보다 1 큰 수가 할당된다

    ```sql
    INSERT INTO members
        (name, gender, email, sign_up_date)
        VALUES ('Honu', 'M', 'honu@racoon.com', '2016-01-19');
    ```

- 여러 row를 삽입할 경우, INSERT INTO문을 계속 적어주는 대신 각각을 tuple로 묶어서 나열해줄 수 있다

    ```sql
    INSERT INTO food_menu
        (menu, price, ingredient)
        VALUES ('떡볶이', 4500, '어묵, 떡, 양파..'),
            ('참치김밥', 3000, '참치, 김, 단무지..'),
            ('오므라이스', 7000, '달걀, 양파, 애호박..');
    ```
    - INSERT INTO문을 여러 번 반복해서 써주는 것보다, 각각을 tuple로 묶어서 하나의 쿼리로 넣어주는 것이 속도가 빠르다 (한번에 다량의 데이터를 넣을 때는 확연한 속도 차이를 보임)


**2. row 정보 업데이트**
- `UPDATE 테이블명 SET 변경할 컬럼 정보 WHERE 조건;`의 구조로 작성

    ```sql
    UPDATE members SET phone = '010-8765-4321' WHERE id = 1;
    ```

- 같은 row의 정보를 한 번에 2개 이상 바꿀 때는 SET 뒤에 나열해주면 됨

    ```sql
    UPDATE members 
        SET phone = '010-8765-4321', name = 'ChaeChae' 
        WHERE id = 1;
    ```

- ※ 정보를 업데이트할 때는, WHERE절을 잘 적어주는 것이 중요하다
    - ex) `UPDATE members SET name = 'ChaeChae'`라고만 적으면 'name' 칼럼의 모든 값이 다 변경됩

- +) workbench에서 safe update 모드가 설정되어 있으면, primary key 외의 컬럼을 WHERE 절에서 사용하는 UPDATE문은 실행이 안된다.
    - MySQL인 경우, [MYSQLWorkbench > Preferences 클릭 > SQL Editor 선택 > 아래 Others 부분에 'Safe Updates'에 체크되어 있는 것을 해제]한 후 재접속하면 primary key 외의 컬럼으로도 접근해서 UPDATE 가능. 


+) 컬럼의 기존 값을 기준으로 값 업데이트하기
- ex) 기말고사 성적에 채점 오류가 있었어서, 모든 row의 score에 +3을 해줘야 하는 상황:

    ```sql
    UPDATE final_exam_result SET score = score + 3;
    -- 이렇게 해주면 모든 row의 점수가 3씩 더해져서 저장됨
    ```


**3. row 삭제**
- `DELETE FROM 테이블명 WHERE 조건;`의 구조로 작성

    ```sql
    DELETE FROM members WHERE id = 4;
    ```

- ※ 업데이트할 때와 마찬가지로, DELETE할 때에도 WHERE절을 신경써야 한다
    - ex) `DELETE FROM memebers;`라고만 하면 테이블의 모든 row가 삭제되어 버린다


<div class="code-example" markdown="1">

+) 물리 삭제 vs 논리 삭제
- 물리 삭제: 삭제해야 할 row를 물리적으로 'DELETE'해서 없애버리는 것
    - ex) `DELETE FROM order WHERE id = 4;`
- 논리 삭제: DELETE 대신, '삭제 여부' 컬럼을 별도로 만들어서, 'YES'와 같이 값을 업데이트해줘서 row가 삭제되었음을 표시해주는 것
    - ex) `UPDATE order SET cancelled = 'Y' WHERE id = 4;`  
- 추후 분석에 활용할 수 있도록 데이터를 남겨두고 싶을 때는 논리 삭제를 이용하는 것이 좋다
    - ex) 사용자가 주문을 취소한 경우, 아예 row 자체를 삭제해버리기보다 row는 그대로 두고 취소된 주문임을 명시해주는 방식을 사용하면 추후에 취소된 주문 데이터를 따로 모아서 분석 가능
- 다만, 논리 삭제 방식으로 삭제하면   
    1) 삭제되지 않은 유효한 row들만 조회할 때 WHERE절에 매번 별도 조건을 넣어줘야 하며,  
    2) 삭제된 row여도 실제 DELETE된 것은 아니기에 DB 용량을 계속 차지한다는 단점이 있다

