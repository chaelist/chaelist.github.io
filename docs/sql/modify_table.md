---
layout: default
title: 테이블 가공
parent: SQL
nav_order: 5
--- 

# 테이블 가공
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

## 컬럼 추가/변경/삭제

1. 컬럼 추가
    - `ALTER TABLE 테이블명 ADD 칼럼 속성;`의 구조로 작성

    ```sql
    -- student라는 테이블에 CHAR(1) 타입 & NULL이 가능한 gender 칼럼을 추가
    ALTER TABLE student ADD gender CHAR(1) NULL;
    ```

1. 컬럼명 변경
    - `ALTER TABLE 테이블명 RENAME COLUMN 칼럼명1 TO 칼럼명2;`의 구조로 작성

    ```sql
    -- student 테이블의 student_number 컬럼을 registration_number라는 이름으로 변경
    ALTER TABLE student
    RENAME COLUMN student_number TO registration_number;
    ```

1. 컬럼 삭제
    -  `ALTER TABLE 테이블명 DROP COLUMN 컬럼;`의 구조로 작성

    ```sql
    -- student 테이블에서 admission_date 컬럼을 삭제
    ALTER TABLE student DROP COLUMN admission_date;
    ```

1. 컬럼의 데이터 타입 변경
    - `ALTER TABLE 테이블명 MODIFY 컬럼 속성;`의 구조로 작성
    - ※ 유의: 데이터 타입을 변경하려면 컬럼 내 값들이 해당 타입이 수용할 수 있는 형태가 되어야 한다 (ex. INT 타입으로 변경하려면 우선 값들을 다 숫자로 바꿔줘야 한다)

    ```sql
    -- 전공명이 저장된 VARCHAR(15) 타입의 컬럼 'major'를 전공코드를 저장하는 INT 타입으로 변경:
    --- UPDATE문으로 각 전공명을 전공코드로 변경해준 후, major 컬럼의 데이터 타입을 INT로 바꿔줌

    UPDATE student SET major = 10 WHERE major = '언론홍보영상학과';
    UPDATE student SET major = 12 WHERE major = '경영학과';
    ALTER TABLE student MODIFY major INT;
    ```


### 컬럼 관련 기타 작업들
1. 컬럼을 가장 앞으로 보내기
    - 속성으로 'FIRST'를 추가: `ALTER TABLE 테이블명 MODIFY 컬럼 속성 FIRST;`의 구조

    ```sql
    -- id 컬럼을 가장 앞으로 보내주기 (주로 primary key를 가장 앞에 둔다)
    ALTER TABLE student MODIFY id INT NOT NULL AUTO_INCREMENT FIRST;
    ```

2. 컬럼 간 순서 바꾸기
    - 'AFTER'를 사용: `ALTER TABLE 테이블명 MODIFY 컬럼 속성 AFTER 컬럼2;`의 구조

    ```sql
    -- gender 컬럼을 name 컬럼 뒤로 보내기
    ALTER TABLE student MODIFY gender CHAR(1) NULL AFTER name;
    ```

3. 컬럼의 이름과 데이터 타입 및 속성 동시에 수정하기
    - 'CHANGE'절을 이용하면 컬럼명과 데이터 타입, 속성을 동시에 수정할 수 있다
    - `ALTER TABLE 테이블명 CHANGE 컬럼명1 컬럼명2 속성`의 구조

    ```sql
    -- role 컬럼의 이름을 'position'으로 바꾸고, 데이터 타입은 VARCHAR(5), 속성은 NOT NULL로 바꿔줌
    ALTER TABLE player CHANGE role position VARCHAR(5) NOT NULL;
    ``` 

4. 하나의 ALTER TABLE문으로 여러 개의 작업을 하는 것도 가능
    - ex) 아래 4가지 작업을 동시에 수행:  
    1) id 컬럼의 이름을 number로 수정  
    2) name 컬럼의 데이터 타입을 VARCHAR(20)로, 속성을 NOT NULL로 수정  
    3) position 컬럼을 테이블에서 삭제  
    4) 새로운 컬럼 height를 추가  

    ```sql
    ALTER TABLE player
      RENAME COLUMN id TO number,
      MODIFY name VARCHAR(20) NOT NULL,
      DROP COLUMN position,
      ADD height DOUBLE NOT NULL;
    ```


## 컬럼에 속성 추가

### NOT NULL, DEFAULT, UNIQUE

1. NOT NULL 속성 추가
    - ALTER ~ MODIFY  문을 사용해서 NOT NULL 속성 추가: `ALTER TABLE 테이블명 MODIFY 컬럼 속성 NOT NULL`의 구조
    - MODIFY 뒤에는 컬럼의 기존 데이터 속성도 함께 써준 후에 NOT NULL을 추가해야 한다.    
      혹은, 변경하려고 하는 데이터 속성을 써줘도 된다 (데이터 타입 변경 & NOT NULL 추가 동시에)
    - NOT NULL 속성을 추가한 컬럼에 값이 없는 row를 추가하려고 하면 error가 난다

    ```sql
    -- student 테이블에 NOT NULL 속성 추가
    ALTER TABLE student MODIFY name VARCHAR(20) NOT NULL;
    ```

1. DEFAULT 속성 추가
    - ALTER ~ MODIFY  문을 사용해서 DEFAULT 속성 추가
    - DEFAULT 값을 추가해두면, 해당 컬럼에 값이 없는 row를 추가할 때 자동으로 DEFAULT 값이 입력됨
    - NOT NULL 속성을 갖고 있지 않은 컬럼들은 Default값이 자동으로 'NULL'로 설정되어 있음: 해당 컬럼에 값이 없는 row를 추가하면, 자동으로 NULL값이 들어간다

    ```sql
    -- major 컬럼의 default값을 101로 설정
    -- 앞으로 major 값이 없는 row를 추가하면 자동으로 101이라는 값이 저장된다는 의미
    ALTER TABLE student MODIFY major INT NOT NULL DEFAULT 101;
    ```

1. UNIQUE 속성 추가
    - ALTER ~ MODIFY  문을 사용해서 UNIQUE 속성 추가
    - UNIQUE 속성을 추가해두면, 컬럼에 이미 존재하는 값과 중복되는 값을 가진 새 row를 입력하려고 하면 error가 난다
    - '학번'처럼, 각 값이 반드시 고유한 값을 가져야 하는 컬럼의 경우 UNIQUE 속성을 주면 좋다
    - cf) UNIQUE 속성이 있는 컬럼이라도, NULL값은 허용될 수 있다.   
      (Primary Key의 경우 반드시 NOT NULL이여야 하는 것과 조금 다른 개념...)

    ```sql
    -- student_number 컬럼에 UNIQUE 속성 추가
    ALTER TABLE student MODIRY student_number INT NOT NULL UNIQUE;
    ```


### CURRENT_TIMESTAMP 속성
: DATETIME, TIMESTAMP 타입의 컬럼에 추가할 수 있는 속성. row가 추가/갱신될 때마다 현재 시각을 저장해줘야 하는 경우 유용하다. (ex. 게시글 업로드 시각, 마지막 수정 시각)  

1. DEFAULT CURRENT_TIMESTAMP 속성
-  테이블에 새 row를 추가할 때 별도로 NOW()값을 주지 않아도 현재 시간이 자동 저장되도록 하는 속성

2. ON UPDATE CURRENT_TIMESTAMP 속성
- 기존 row가 단 하나의 컬럼이라도 수정되면, 업데이트될 때의 시간이 자동으로 저장되도록 하는 속성.

```sql
-- upload_time 컬럼에는 DEFAULT CURRENT_TIMESTAMP 속성을,
-- recent_modified_time 컬럼에는 DEFAULT CURRENT_TIMESTAMP 속성과 ON UPDATE CURRENT_TIMESTAMP 속성을 모두 추가
ALTER TABLE post
  MODIFY upload_time DATETIME DEFAULT CURRENT_TIMESTAMP,
  MODIFY recent_modified_time DATETIME 
    DEFAULT CURRENT_TIMESTAMP 
    ON UPDATE CURRENT_TIMESTAMP;
```
- 이렇게 해두면  
  1) 새로운 게시글을 추가할 때 다른 컬럼들에만 값을 적어줘도 upload_time, recent_modified_time 컬럼에 자동으로 현재 시간이 들어가고,  
  2) 게시글 내용을 수정하면 recent_modified_time의 값이 자동으로 해당 시각으로 업데이트된다  

&nbsp;
{: .fs-1 .lh-0}

**+) NOW() 함수**: 현재 시각을 추가해주는 함수
- NOW() 함수를 사용해도 위와 같은 결과를 낼 수 있음 (다만, 수동으로 NOW()를 매법 입력해줘야 한다)

  ```sql
  INSERT INTO post (title, content, upload_time, recent_modified_time)
    VALUES ("제목", "본문", NOW(), NOW());
  -- upload_time, recent_modified_time 모두 현재 시각이 추가됨
  ```

  ```sql
  UPDATE post 
    SET content = '본문 수정', recent_modified_time = NOW()
    WHERE id = 1;
  -- recent_modified_time이 현재 시각으로 변경됨
  ```


### CONSTRAINT 속성
: 컬럼에 적절한 constraint(제약 사항)을 걸어두면 잘못된 데이터가 입력되는 것을 막아서 데이터 퀄리티를 관리할 수 있다

1. ADD CONSTRAINT
    - 컬럼에 제약 사항을 추가해주는 것
    - `ALTER TABLE 테이블명 ADD CONSTRAINT 제약이름 CHECK (조건);`의 구조로 작성
    - 제약 조건에 위배되는 데이터를 넣으려고 하면 error가 발생한다

    ```sql
    -- student 테이블에 'student_number는 3천만보다 작아야 한다'는 제약 걸기

    ALTER TABLE student
        ADD CONSTRAINT st_rule CHECK (student_number < 30000000);

    -- (여기서 st_rule은 이 제약 사항의 이름)
    -- 위와 같은 제약을 건 후에 student_number에 30000000이 들어가는 row를 삽입하려 하면 error가 발생
    ```

1. DROP CONSTRAINT
    - 걸어둔 CONSTRAINT 삭제하기
    - `ALTER TABLE 테이블명 DROP CONSTRAINT 제약이름;`의 구조

    ```sql
    ALTER TABLE student DROP CONSTRAINT st_rule;
    ```

1. 두 개 이상의 조건이 담긴 CONSTRAINT 만들기
    - 여러 조건을 AND로 연결해주면 된다

    ```sql
    -- email 컬럼에 들어갈 값에는 '@'가 들어가야 하고, gender 컬럼에 들어갈 값은 'm'이나 'f' 둘 중 하나여야 한다는 조건 걸기:
    ALTER TABLE student
        ADD CONSTRAINT st_rule
        CHECK (email LIKE '%@%' AND gender IN ('m', 'f'));
    ```


## 테이블 변경/복사/삭제

1. 테이블 이름 변경
    - `RENAME TABLE 테이블명 TO 테이블명2`의 구조로 작성

    ```sql
    RENAME TABLE student TO undergraduate;
    ```

1. 테이블 복사
    - 'AS'를 사용해 SELECT문으로 가져온 데이터를 복사한 테이블을 생성
    -  `CREATE TABLE 테이블명 AS SELECT문;`의 구조로 작성

    ```sql
    CREATE TABLE undergraduate_copy AS SELECT * FROM undergraduate;
    ```

    +) 'WHERE'절을 사용해 특정 조건의 데이터만 복사한 테이블을 생성하는 것도 가능:

    ```sql
    -- gender가 'u'인 item 테이블의 row들만 새로운 테이블로 복사
    CREATE TABLE item_copy AS SELECT * FROM item WHERE gender = 'u';
    ```

1. 테이블 삭제
    - `DROP TABLE 테이블명;`의 구조로 작성

    ```sql
    DROP TABLE undergraduate_copy;
    ```

1. 테이블의 컬럼 구조 복사
    - 테이블 내의 데이터를 제외하고, 컬럼 구조만 복사해오는 것
    - `CREATE TABLE 테이블명 LIKE 기존_테이블;`의 구조로 작성

    ```sql
    CREATE TABLE undergraduate_copy LIKE undergraduate;
    ```

    +) 컬럼 구조만 복사해둔 테이블에, 다시 데이터까지 복사해서 붙여넣으려면?:

    ```sql
    -- SELECT문으로 가져온 데이터를 INSERT INTO문으로 넣어주기
    INSERT INTO undergraduate_copy SELECT * FROM undergraduate;
    ```

1. 테이블 내 데이터만 삭제
    - 테이블의 구조는 남기고, 데이터만 모두 삭제하기 (cf. DROP TABLE은 테이블 자체를 삭제)
    - `TRUNCATE 테이블명;`의 구조로 작성

    ```sql
    TRUNCATE exam_result;
    ```

    +) DELETE FROM을 사용해도 데이터를 모두 삭제하는 결과를 낼 수 있다:
    ```sql
    DELETE FROM exam_result;
    ```
    - 다만, DELETE FROM으로 삭제하면 데이터가 한줄한줄 제거되는 과정을 거치게 되며, 데이터는 지워지되 사용하던 공간은 그대로 남아 있음  
    (TRUNCATE와 DELETE는 내부적으로 구현되는 방식이 다름)


    



