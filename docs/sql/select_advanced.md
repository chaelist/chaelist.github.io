---
layout: default
title: 데이터 조회 심화
parent: SQL
nav_order: 2
--- 

# 데이터 조회 심화
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

## 집계/산술 함수

### 집계 함수
: 특정 칼럼의 여러 row 값들을 동시에 고려해서 실행되는 함수.  
아래 함수들은 계산할 때 null값은 고려하지 않고 계산된다. (null값은 제외하고 계산)
1. COUNT
```sql
-- email 칼럼에 존재하는 null을 제외한 값의 수를 구해준다
SELECT COUNT(email) FROM tablename; 
```
    - 다만, 특정 칼럼에 COUNT()를 적용하면 null값이 제외된 개수를 구해주기 때문에, 전체 테이블의 row 수를 구해주려면 아래와 같이 전체에 COUNT()를 적용해주는 것이 좋다
    ```sql
    SELECT COUNT(*) FROM tablename; -- 전체 테이블의 row 수 구하기
    ```

1. MAX 
```sql
-- 키의 최댓값 구하기
SELECT MAX(height) FROM tablename;
``` 

1. MIN 
```sql
-- 키의 최솟값 구하기
SELECT MIN(height) FROM tablename;
```

1. AVG
```sql
-- 키의 평균값 구하기
SELECT AVG(height) FROM tablename; 
```

1. SUM
```sql
-- 나이의 총합 구하기
SELECT SUM(age) FROM tablename;
```

1. STD
```sql
- 나이의 표준편차 구하기
SELECT STD(age) FROM tablename;
```

### 산술 함수
: 특정 칼럼의 각 row마다 실행되는 함수
1. ABS: 절대값을 구하는 함수
1. SQRT: 제곱근을 구하는 함수
1. CEIL: 올림을 해주는 함수
    - ex) 178.4라는 데이터 → `CEIL(height)`로 출력하면 179라고 출력됨
1. FLOOR: 내림을 해주는 함수
1. ROUND: 반올림을 해주는 함수
    - ex) `ROUND(값, 1)`이라고 하면 소수점 첫번째 자리까지 반올림해서 출력

+) 그 외 다양한 산술 함수: [https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html){: target="_blank"}


## 문자열 가공 함수

1. SUBSTRING(칼럼, m, n): 특정 칼럼의 m번째 위치부터 n개의 글자만 추출
```sql
SELECT SUBSTRING(address, 3, 3) FROM tablename;
-- '서울특별시 중구 서소문로 00'와 같은 주소 → '특별시'라고 출력됨 (3번째 위치에서부터 3개의 글자만 추출)
```

1. LENGTH(): 문자열의 길이를 구해줌
```sql
SELECT LENGTH(address) FROM tablename;
-- `안드로메다 128행성`이라는 주소 → LENGHT()의 결과는 25
```
    - ※ 다만, LENGTH() 함수는 문자의 Byte길이를 가져오기 때문에 한글의 경우 정확한 길이를 알기 어렵다
    - 대신 **CHAR_LENGTH()** 함수를 사용하면 몇 개의 글자가 사용되었는지 파악 가능. (공백도 포함해 계산)
    ```sql
    SELECT CHAR_LENGTH(address) FROM tablename;
    -- `안드로메다 128행성`이라는 주소 → CHAR_LENGTH()의 결과는 11
    ```
    

1. UPPER, LOWER
    ```sql
    -- 모든 문자를 대문자로 변환
    SELECT UPPER(address) FROM tablename;
    ```
    ```sql
    -- 모든 문자를 소문자로 변환
    SELECT LOWER(address) FROM tablename;
    ```

1. LPAD, RPAD: 문자열의 왼쪽(LPAD) 또는 오른쪽(RPAD)을 특정 문자열로 채워주는 함수  
(*보통 어떤 숫자의 자리수를 맞출 때 자주 사용)

    ```sql
    -- age 컬럼의 값을, 왼쪽에 문자 0을 붙여서 총 10자리로 만들어 준다
    SELECT email, LPAD(age, 10, '0') FROM tablename; 
    -- ex) 28 → 0000000028, 101 → 0000000101
    ```

    ```sql
    -- age 컬럼의 값을, 오른쪽에 !를 붙여서 총 5자리로 만들어 준다
    SELECT email, RPAD(age, 5, '!') FROM tablename; 
    -- ex) 28 → 28!!!, 101 → 101!!  
    ```
    
    - ※ 사실 age 칼럼은 INT 타입이지만, 숫자도 문자열 함수 안에 인자로 넣어주면 그 값이 자동으로 문자열로 형 변환이 되어 계산된다

1. TRIM, LTRIM, RTRIM: 문자열 양 끝에 존재하는 공백을 제거하는 함수
    - LTRIM(word): 왼쪽 공백 삭제
    - RTRIM(word): 오른쪽 공백 삭제
    - TRIM(word): 왼쪽, 오른쪽 양쪽 공백 모두 삭제


## Null값 다루기 & 중복 제거

1. Null값만 / Null값 제외하고 출력
- NULL은 어떤 값도 아니기에, = NULL, != NULL 이렇게는 표현할 수 없고, IS NULL 혹은 IS NOT NULL이라고 표현해야 함

    ```sql
    -- address 칼럼이 null값인 데이터만 조회
    SELECT * FROM tablename WHERE address IS NULL; 
    ```
    ```sql
    -- address 칼럼이 null값이 아닌 데이터만 조회
    SELECT * FROM tablename WHERE address IS NOT NULL; 
    ```
    ```sql
    -- address, height, weight 세 칼럼 중 하나라도 null값이 있는 데이터 조회
    SELECT * FROM tablename 
    WHERE address IS NULL OR height IS NULL OR weight IS NULL; 
    ```

### COALESCE(): NULL값 대체
- COALESCE(값1, 값2): null이 아니면 값1을, null이면 값2를 돌려줌
```sql
SELECT
    COALESCE(height, '####'),
	COALESCE(weight, '---'),
	COALESCE(address, '@@@')
FROM tablename;
-- height 칼럼의 null 부분은 ####가, weight 칼럼은 ---, address는 @@@가 출력됨
```

**+) NULL값을 변환하는 다양한 방법**
1. `COALESCE(height, 'N/A')` -- height 칼럼의 null값을 'N/A'라는 text로 채워줌
1. `COALESCE(height, weight * 2.3, 'N/A')`  -- height 칼럼의 null값을 우선 weight에 2.3을 곱한 값으로 넣어주고, 만약 weight 칼럼도 NULL이면 'N/A'로 채워줌
1. `IFNULL(height, 'N/A')` -- height 칼럼의 null값을 'N/A'라는 text로 채워줌
    - COALESCE(height, 'N/A')와 같지만, IFNULL은 mysql에서만 지원하는 함수이고 COALESCE는 SQL 표준 함수이다
1. `IF(height IS NOT NULL, height, 'N/A')` -- height가 NULL이 아니면 height값을 return하고, NULL이면 'N/A’라는 text를 return
    - IF(조건식, 결과1, 결과2): 조건식이 True면 결과1을, False면 결과2를 return하라는 구문
    - IF 구문은 NULL값 반환 이외의 다양한 조건에도 응용할 수 있다



### DISTINCT(): 중복 제거 확인
- DISTINCT 함수: Unique한 값만 확인하게 해주는 함수
    ```sql
    -- gender 칼럼의 고유값만 확인
    SELECT DISTINCT(gender) FROM tablename; 
    ```
    ```sql
    -- address 칼럼의 가장 앞 두 글자(ex. 경기, 서울, 전라)만 추출해서 고유값만 확인
    SELECT DISTINCT(SUBSTRING(address, 1, 2)) FROM tablename;
    ```
- +) 고유값 개수 구하기
    ```sql
    -- gender 칼럼에 있는 고유값의 개수를 세어줌
    SELECT COUNT(DISTINCT(gender)) FROM tablename; 
    ```


## 컬럼 간 계산/연결
1. 컬럼 산술연산: + (더하기), - (빼기), * (곱하기), / (나누기), % (나머지 구하기)
    ```sql
    -- 이메일, 키, 몸무게, BMI (몸무게(kg) / 키(m)^2)를 출력
    -- BMI 계산할 때 키는 보통 m단위로 넣으므로 100을 나눠서 제곱해줌
    SELECT email, height, weight, weight / ((height/100) * (height/100))
    FROM tablename; 
    ```
    - 연산시, 한 컬럼이라도 NULL이면 계산식의 결과도 NULL

1. alias 붙이기: '**AS**' 사용
- 원래의 컬럼 이름을 다른 이름으로 교체해서 보여주는 것.
- 단순히 계산식만 보이는 것보다, 보다 직관적인 alias를 붙여서 보여주면 이해하기 쉽다
- `SELECT column AS alias`와 같은 식으로 AS를 사용하거나, `SELECT column alias`와 같이 빈칸을 한 칸 두고 뒤에 써줘도 된다  
(하지만 AS를 쓰는 습관을 들이면 조금 더 직관적으로 이해하기 쉬운 코드가 된다)

    ```sql
    SELECT email, height, weight, 
        weight / ((height/100) * (height/100)) AS BMI
    FROM tablename;
    -- 이렇게 적으면 weight / ((height/100) * (height/100)) 계산 결과 칼럼은 'BMI'라는 이름으로 출력됨
    ```

1. **CONCAT**: 여러 컬럼을 원하는 형식으로 연결해 출력
    ```sql
    -- '165.5cm, 67.3kg'와 같은 형식으로, height와 weight 컬럼이 결합되어 출력됨
    SELECT CONCAT(height, 'cm, ', weight, 'kg') AS '키와 몸무게' FROM tablename;
    ```
    - cf) '키와 몸무게'처럼 alias에 빈칸이 있을 경우, ''나 ""로 감싸서 구분해줘야 한다


## CASE: 조건문으로 가공
1. 단순 CASE 함수: CASE 뒤에 컬럼을 적고, 그 컬럼의 값과 어떤 특정한 값이 같은지(=) 비교
- 구조: (WHEN ~ THEN ~ 은 원하는 만큼 사용 가능)
    ```sql
    CASE  칼럼 이름
        WHEN 값 THEN 출력할 내용
        ELSE 출력할 내용 
    END
    ```
- 예시:
    ```sql
    -- age 값이 29면 '스물 아홉'으로, 30이면 '서른'으로 바꿔서 출력
    SELECT email,
    CASE age
        WHEN 29 THEN ‘스물 아홉’
        WHEN 30 THEN ‘서른’
        ELSE age
    END
    FROM tablename;
    ```


1. 검색 CASE 함수: 특정 조건에 따라 특정 값을 돌려준다. (단순 CASE 함수보다 다양한 연산이 가능)
    - 구조: (WHEN ~ THEN ~ 은 원하는 만큼 사용 가능)

        ```sql
        CASE 
            WHEN 조건 THEN 출력할 내용
            ELSE 출력할 내용 
        END
        ```

    - 예시:

        ```sql
        SELECT 
            CONCAT(height, 'cm, ', weight, 'kg') AS '키와 몸무게',
            weight / ((height/100) * (height/100)) AS BMI,

        (CASE
            WHEN weight IS NULL or height IS NULL THEN '비만 여부 알 수 없음'
            WHEN weight / ((height/100) * (height/100)) >= 25 THEN '과체중'
            WHEN weight / ((height/100) * (height/100)) >= 18.25
        AND weight / ((height/100) * (height/100)) < 25
        THEN '정상'
            ELSE '저체중'
        END) AS obesity_check

        FROM tablename
        ORDER BY obesity_check ASC;
        ```

    - BMI에 따라 ‘과체중’ , ‘정상’, ‘저체중’으로 나누어 결과를 보여주는 칼럼을 새로 생성해 출력
    - +) CASE의 경우,  alias를 붙여주는 게 좋다 (이 경우는 obesity_check라는 이름으로 출력됨)
    - +) ORDER BY obesity_check ASC도 붙여줬으므로 obesity_check 칼럼을 기준으로 오름차순 정렬되어 출력


<div class="code-example" markdown="1">

+) 위 SQL문을 서브쿼리를 활용해 더 간결하게 표현하기:
```sql
SELECT email, BMI,
(CASE 
    WHEN weight IS NULL OR height IS NULL THEN '비만 여부 알 수 없음'
    WHEN BMI >= 25 THEN '과체중'
    WHEN BMI >= 18.5 
        AND BMI < 25 THEN '정상'
    ELSE '저체중'
END) AS obesity_check

FROM 
    (SELECT *, weight/((height/100) * (height/100)) AS BMI
    FROM tablename) AS subquery_for_BMI
ORDER BY obesity_check ASC;
```

</div>



## GROUP BY: 그루핑해서 보기

1. 집계함수(COUNT, AVG, MIN, MAX 등)과 함께 사용
    - ※규칙: GROUP BY를 사용할 때는, SELECT 절에 (1) GROUP BY 뒤에 나오는 칼럼들 또는 (2) 집계함수만 쓸 수 있다!

    ```sql
    -- 각 gender별 회원 수 조회 (m: 15, f: 9)
    ex) SELECT gender, COUNT(*) FROM tablename GROUP BY gender; 

    -- 각 gender별 평균 키를 조회 (m: 176.55, f:173.7)
    ex) SELECT gender, AVG(height) FROM tablename GROUP BY gender;

    -- 각 gender별 최소 몸무게를 조회 (m: 58, f: 47)
    SELECT gender, MIN(weight) FROM tablename GROUP BY gender; 
    ```


2. 기존 컬럼이 아닌, 새로운 기준을 만들어 그루핑하는 것도 가능
    ```sql
    -- 지역별(ex. 서울, 경기, 대전) 회원 수 확인
    -- address의 맨 첫글자 2개만 추출해서 활용 (ex. 서울)
    SELECT 
        SUBSTRING(address, 1, 2) AS region,
        COUNT(*)
    FROM tablename 
    GROUP BY SUBSTRING(address, 1, 2); 
    ```

3. 2가지 이상의 기준으로도 그루핑 가능
    ```sql
    -- 지역별*성별 회원 수 확인
    SELECT 
        SUBSTRING(address, 1, 2) AS region,
        gender,
        COUNT(*)
    FROM tablename 
    GROUP BY 
        SUBSTRING(address, 1, 2),
        gender;
    ```

4. 그루핑을 하고 나면, 깔끔하게 정렬을 해주는 게 좋다
    ```sql
    SELECT 
        SUBSTRING(address, 1, 2) as region,
        gender,
        COUNT(*)
    FROM tablename
    GROUP BY 
        region, -- MySQL의 경우, GROUP BY 뒤에 바로 alias를 활용하는 것도 가능
        gender
    ORDER BY
        region ASC, 
        gender DESC ;
    ```


### HAVING
: 그루핑 후, 특정 조건의 데이터만 확인

```sql
SELECT 
    SUBSTRING(address, 1, 2) as region,
    gender,
    COUNT(*)
FROM tablename
GROUP BY 
    region, gender
HAVING region = '서울'
ORDER BY
    region ASC, 
    gender DESC;
-- → 이렇게 하면 region이 서울인 데이터만 조회됨
-- MySQL의 경우, HAVING 뒤에 바로 alias를 활용 가능
```

※ WHERE와 HAVING의 차이:
- WHERE는 테이블에서 맨 처음 row들을 조회할 때 조건을 설정하기 위한 구문이고, 
- HAVING은 이미 조회된 row들을 그루핑했을 때 그 그룹들 내에서 다시 필터링할 때 쓰는 구문


### WITH ROLLUP
: 부분 총계를 포함해 그루핑하기

```sql
SELECT 
    SUBSTRING(address, 1, 2) as region,
    gender,
    COUNT(*)
FROM tablename
GROUP BY 
    region, gender WITH ROLLUP
ORDER BY
    region ASC, 
    gender DESC;
```
- 이렇게 WITH ROLLUP이 들어가면, 세부 그룹들을 좀 더 큰 단위의 그룹으로 중간중간에 합쳐준다.
    - ex) 성*연령 교차 데이터라고 하면, 그냥 그루핑하면 '여성-10대', '여성-20대' 등의 그룹만 생성되는 한편,   
WITH ROLLUP이 들어가면 'NULL-10대' (성별전체-10대를 의미) 이런 식으로 부분총계 값 생성됨
- WITH ROLLUP은 GROUP BY 뒤에 나오는 그루핑 기준의 순서에 맞춰서 계층적인 부분 총계를 보여줌
    - ex) `GROUP BY gender, age` 순서면 '여성-NULL'(=여성-나이전체)는 출력되지만, 'NULL-여성'(=나이전체-여성)은 출력되지 않는다


<div class="code-example" markdown="1">

+) 'Null값을 나타내는 NULL'과 'WITH ROLLUP의 결과로 부분 총계를 표현하기 위해 쓰인 NULL' 구분:
- GROUPING() 함수 사용:
    - 그 인자를 그루핑 기준에서 고려하지 않은 부분 총계인 경우에 1을 리턴, 그렇지 않은 경우 0을 리턴.   
(같은 NULL이더라도 원래 NULL이 있던 곳은 0이 출력되고, 부분 총계를 나타내기 위해 NULL이 쓰인 곳은 1이 출력됨)


    ```sql
    SELECT 
        YEAR(sign_up_day) AS s_year, 
        gender,
        SUBSTRING(address, 1, 2) as region,
        GROUPING(YEAR(sign_up_day)), GROUPING(gender), GROUPING(SUBSTRING(address, 1, 2)),
        COUNT(*)
    FROM tablename 
    GROUP BY 
        s_year, gender, region WITH ROLLUP
    ORDER BY s_year DESC;
    ```

</div>


## SELECT문: 각 절의 작성 순서
(+. 해석/실행되는 순서도 별도로 표시)

1. SELECT: 모든 컬럼 또는 특정 컬럼들을 조회 (해석 순서: 5)
    - SELECT 절에서 컬럼 이름에 alias를 붙인 게 있다면, 이 이후 단계(ORDER BY, LIMIT)부터는 해당 alias를 사용할 수 있다
    - MySQL에서는 GROUP BY나 HAVING에서도 SELECT절에서 붙인 alias를 사용 가능
1. FROM: 어느 테이블을 대상으로 할 지 결정 (해석 순서: 1)
1. WHERE: 해당 테이블에서 특정 조건(들)을 만족하는 row들만 선별 (해석 순서: 2)
1. GROUP BY: row들을 그루핑 기준에 따라 그루핑 (해석 순서: 3)
1. HAVING: 그루핑 작업 후 생성된 여러 그룹 중에서, 특정 조건(들)을 만족하는 그룹만 선별 (해석 순서: 4)
1. ORDER BY: 각 row를 특정 기준에 따라 정렬 (해석 순서: 6)
1. LIMIT: 이전 단계까지 조회된 row들 중 일부 row만 선별해서 출력 (해석 순서: 7)

+) 그 외 작성 순서: [https://dev.mysql.com/doc/refman/8.0/en/select.html](https://dev.mysql.com/doc/refman/8.0/en/select.html){: target="_blank"}