---
layout: default
title: 조인, 서브쿼리, 뷰
parent: SQL
nav_order: 3
--- 

# 조인, 서브쿼리, 뷰
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

## 테이블 합치기

### 결합연산 (JOIN)
: 테이블을 가로 방향으로 붙이는 연산

1. LEFT OUTER JOIN: 왼쪽 테이블을 기준으로 합쳐짐 (왼쪽 테이블에 존재하는 row만 보여짐)
    - ex. 만약 왼쪽 테이블 값(ex. 꽃무늬 원피스)와 연결되는 값이 오른쪽에 2개 이상이라면(ex. 리뷰1, 리뷰2) → 왼쪽 테이블에는 해당 값이 여러 번 표시될 수 있음
1. RIGHT OUTER JOIN: 오른쪽 테이블을 기준으로 합쳐짐 (오른쪽 테이블에 존재하는 row만 보여짐)
1. INNER JOIN: 왼쪽, 오른쪽 테이블 모두에 존재하는 row만 추려서 합쳐짐 (교집합 개념)

```sql
-- ex) item 테이블을 기준으로, stock 테이블을 join
-- item 테이블의 id 칼럼과 stock 데이블의 item_id 칼럼을 key로 해서 join
SELECT
    item.id,
    item.name,
    stock.item_id,
    stock.inventory_count
FROM item LEFT OUTER JOIN stock
ON item.id = stock.item_id;
```

+) 테이블에 alias 붙이기
- JOIN할 때는 테이블에 alias를 붙여서 사용하는 경우가 많다. (간결하게 작성하기 위함)
- ※주의: 한 번 테이블에 alias를 붙였으면, 다른 모든 절에서 그 테이블은 그 alias로만 표현해야 함. (원래의 테이블 이름이랑 섞어서 나타낼 수 없음)

```sql
SELECT
    i.id,
    i.name,
    s.item_id,
    s.inventory_count
FROM item AS i LEFT OUTER JOIN stock AS s
ON i.id = s.item_id;
```


+) 테이블 3개 JOIN하기

```sql
SELECT
    i.name, i.id,
    r.item_id, r.star, r.comment, r.mem_id,
    m.id, m.email
FROM 
    item AS i LEFT OUTER JOIN review AS r
        ON r.item_id = i.id
    LEFT OUTER JOIN member AS m
        ON r.mem_id = m.id
ORDER BY i.name ASC;
```


### 기타 다양한 JOIN들

1. NATURAL JOIN
- 두 테이블에서 같은 이름의 컬럼을 찾아서 자동으로 그것들을 조인 조건으로 설정하고, INNER JOIN을 해준다
- 다만, NATURAL JOIN을 할 수 있는 경우라도 직관적으로 해석 가능한 SQL문을 위해서 ON으로 조인 조건을 명시해주는 것이 더 좋다

    ```sql
    SELECT 
        p.id, p.player_name, p.team_name, 
        t.team_name, t.region 
    FROM player AS p INNER JOIN team AS t
    ON p.team_name = t.team_name;

    --위와 같은 INNER JOIN을 아래와 같이 구현하는 것도 가능:
    SELECT 
        p.id, p.player_name, p.team_name, 
        t.team_name, t.region 
    FROM player AS p NATURAL JOIN team AS t
    ```

1. CROSS JOIN 
- 두 테이블의 row들의 모든 조합을 보여주는 JOIN  
(두 테이블의 카르테시안 곱(Cartesian Product)을 구하는 것)

    ```sql
    -- 가능한 모든 shirts와 pants의 조합을 보고 싶은 경우:
    SELECT * FROM shirts CROSS JOIN pants
    ```

1. Self Join 
- 자기 자신과 JOIN하는 방식. (한 테이블을 자기 자신과 다시 JOIN해주는 것)

    ```sql
    -- 회사 구성원 정보가 담긴 테이블 → 자기 자신과 join해서 각 직원의 상사 정보를 찾아본다
    SELECT employee AS e1 LEFT OUTER JOIN employee AS e2
    ON e1.boss = e2.id;
    ```

1. FULL OUTER JOIN 
- 두 테이블의 LEFT OUTER JOIN 결과와 RIGHT OUTER JOIN 결과를 합쳐주는 JOIN. 두 결과에 모두 존재하는 row들(두 테이블에 공통으로 존재하던 row들)은 한번만 표현해준다 (합집합의 개념)

    ```sql
    SELECT * 
    FROM player AS p FULL OUTER JOIN team AS t
    ON p.team_name = t.team_name;
    ```

    ※ Oracle에서는 위와 같이 `FULL OUTER JOIN` 연산자가 지원되지만, MySQL에서는 지원되지 않는다.  
    → LEFT OUTER JOIN과 RIGHT OUTER JOIN을 UNION해서 계산하면 됨!

    ```sql
    SELECT *
    FROM player AS p LEFT OUTER JOIN team AS t
    ON p.team_name = t.team_name
    UNION
    SELECT *
    FROM player AS p RIGHT OUTER JOIN team AS t
    ON p.team_name = t.team_name
    ```

1. Non-Equi Join 
- Equi: Equality Condition (동등 조건)
- Non-Equi Join: 등호(=)가 아닌 다른 조건을 사용하는 JOIN. 

    ```sql
    -- 특정 회원이 가입한 날(sign_up_date)보다 이후에 올라온 상품들을 확인
    SELECT * 
    FROM member AS m LEFT OUTER JOIN item AS i
    ON m.sign_up_date < i.registration_date   -- 부등호(<)로 비교
    ORDER BY m.sign_up_date ASC;
    ```


### 집합연산 (UNION 등)
: 테이블을 세로 방향으로 합치는 연산 (※ 같은 종류의 테이블끼리만 가능)  
+) 서로 다른 종류의 테이블의 경우, 조회하는 컬럼을 일치시키면 집합 연산이 가능해진다

1. A ∩ B (INTERSECT): A와 B에 모두 존재하는 row만 출력
    ```sql
    SELECT * FROM item    -- 기존 item 테이블
    INTERSECT 
    SELECT * FROM item_new  -- 새로 업데이트된 item 테이블
    ```

1. A - B (MINUS): A에는 존재하지만 B에는 존재하지 않는 row만 출력
    - MINUS 연산자 대신 EXCEPT 연산자를 사용해도 된다

    ```sql
    SELECT * FROM item 
    MINUS 
    SELECT * FROM item_new
    ```

1. A ∪ B (UNION): A와 B에 존재하는 row를 모두 출력하되, 공통으로 존재하는 row는 중복을 제거하고 하나만 표시 (합집합의 개념)
    ```sql
    SELECT * FROM item 
    UNION 
    SELECT * FROM item_new
    ```

1. UNION ALL: A와 B에 존재하는 row를 모두 출력 (중복을 제거하지 않고 그냥 합쳐줌)
    ```sql
    SELECT * FROM item 
    UNION ALL 
    SELECT * FROM item_new
    ```

※ MySQL 8.0에서는 UNION, UNION ALL 연산자만 지원 (Oracle에서는 모두 지원)

<br/>

+) JOIN으로도 집합연산을 유사하게 수행 가능

```sql
SELECT
	old.id AS old_id,
	old.name AS old_name,
    new.id AS new_id,
    new.name AS new_name
FROM item AS old LEFT OUTER JOIN item_new AS new
ON old.id = new.id
WHERE new.id IS NULL;
```
- 이렇게 하면 왼쪽 테이블에는 있지만 오른쪽 테이블에는 없는 row를 확인 가능 (기존 item 테이블엔 정보가 있었지만 오른쪽 새 테이블에선 누락된 것들 확인)
- +) 위 예시처럼 JOIN할 때의 key가 되는 두 테이블 내 칼럼의 이름이 같으면 ON 대신 **USING**을 써도 된다!  
(`ON old.id = new.id` 대신 `USING(id)`라고 사용 가능)



## 서브쿼리 (SubQuery)
: SQL문 안에 부품처럼 들어가는 SELECT문 (전체 SQL문 안에 있는 또 다른 SQL문의 개념)
- ※ 서브쿼리는 꼭 ( ) 안에 써줘야 함!

### 비상관 서브쿼리(Non-correlated Subquery)
: 서브쿼리의 SQL절만 독립적으로 써도 실행되는 쿼리.

1. 단일값을 반환하는 서브쿼리 (스칼라 서브쿼리)
    - ex1) SELECT절에서 사용:

        ```sql
        -- 각 상품의 price와 쉽게 비교할 수 있도록, 별도로 AVG(price) 값을 가져와서 출력
        SELECT 
            id, 
            name, 
            price,
            (SELECT AVG(price) FROM item) AS avg_price
        FROM item;
        ```

    - ex2) WHERE절에서 사용:

        ```sql
        -- 가장 높은 가격을 가진 상품을 출력
        SELECT id, name, price
        FROM item
        WHERE price = (SELECT MAX(price) FROM item);
        ```

    - ex3) HAVING절에서 사용:

        ```sql
        -- 전체 star 평균보다 작은 avg_star를 갖는 group만 출력
        SELECT i.id, i.name, AVG(star) AS avg_star
        FROM item as i LEFT OUTER JOIN review AS r
        ON r.item_id = i.id
        GROUP BY i.id, i.name
        HAVING avg_star < (SELECT AVG(star) FROM review)
        ```

1. 리스트가 결과로 나오는 서브쿼리  
(하나의 column & 여러 row의 형태가 반환되는 서브쿼리)
    - 리스트 형태의 결과가 나오는 서브쿼리는 IN, ANY, ALL 등과 함께 사용 가능

    ```sql
    -- 리뷰 수가 3개 이상인 item의 정보만 출력
    SELECT * FROM item
    WHERE id IN 
        (SELECT item_id
        FROM review
        GROUP BY item_id HAVING COUNT(*) >= 3);
    ```

1. 테이블이 결과로 나오는 서브쿼리
    - ※ 이런 식으로 서브쿼리로 가져온 '<u>derived table</u>'을 사용할 때는, 반드시 alias를 붙여줘야 한다!

    ```sql
    -- 지역별 리뷰 수를 count한 후, 평균값, 최댓값, 최솟값을 출력 
    SELECT 
        AVG(review_count),
        MAX(review_count),
        MIN(review_count)
    FROM
        (SELECT
            SUBSTRING(address, 1, 2) AS region,
            COUNT(*) AS review_count
        FROM review as r LEFT OUTER JOIN member AS m
        ON r.mem_id = m.id
        GROUP BY SUBSTRING(address, 1, 2)
        HAVING region IS NOT NULL) AS review_count_summary;
    ```

### 상관 서브쿼리(Correlated Subquery)
: 독립적으로는 실행되지 않고, 바깥 SQL절과 관계를 맺고 있는 서브쿼리.

1. EXISTS와 함께 사용

    ```sql
    -- 리뷰가 달린 상품들만 조회
    SELECT * FROM item
        WHERE EXISTS (SELECT * FROM review WHERE review.item_id = item.id);
    ```
    
    - 해석:  
        (1) 일단 item 테이블의 첫 번째 row를 생각  
        (2) 그 row의 id(item.id) 값과 같은 값을 item_id(review.item_id) 컬럼에 가진 review 테이블의 row가 있는지 조회  
        (3) 만약에 존재하면(EXISTS)  
        (4) WHERE 절은 True가 되고, (1)에서 생각했던 item 테이블의 row는 조회 대상이 된다  
        (5) item 테이블의 모든 row에 대해 순차적으로 (2)~(4)를 반복한 후 대상 row들만 출력해준다  

1. NOT EXISTS 사용 (EXISTS의 반대 경우)

    ```sql
    -- 리뷰가 달리지 않은 상품들만 조회
    SELECT * FROM item
    WHERE NOT EXISTS (SELECT * FROM review WHERE review.item_id = item.id);
    ```

1. EXISTS, NOT EXISTS 없는 상관 서브쿼리

    ```sql
    -- member 테이블을 조회하면서, 특정 회원과 같은 해에 태어난 회원들 중 가장 작은 키를 가진 회원의 키 정보를 담은 컬럼을 오른쪽 끝에 추가
    SELECT *,
        (SELECT MIN(height)
        FROM member AS m2 WHERE birthday IS NOT NULL AND height IS NOT NULL
        AND YEAR(m1.birthday) = YEAR(m2.birthday)) AS min_height_in_the_year
    FROM member AS m1
    ORDER BY min_height_in_the_year ASC;
    ```

    - *해석:  
        (1) 일단 birthday 컬럼과 height 컬럼에 둘다 값이 있는 회원들만 대상으로 해야하기 때문에 'birthday IS NOT NULL AND height IS NOT NULL'  조건을 걸어둠  
        (2) member 테이블의 첫 번째 row를 생각  
        (3) 그 row에 대해서 같은 YEAR(birthday) 값을 가진 row들을 찾는다  
        (4) 그 다음 해당 row들 중 height의 최솟값을 구한다

 
+) 서브쿼리를 중첩해서 사용하는 것도 가능하지만 (서브쿼리 안에 또 다른 서브쿼리), 가독성이 떨어지므로 '뷰'를 활용하는 것이 나을 수 있다


## 뷰 (View)
- `CREATE VIEW 뷰_이름 AS 특정한_SQL문`의 구조로 '뷰'를 생성해두고, 필요할 때마다 원래 있는 테이블인 것처럼 자연스럽게 가져다 쓰면 된다
- 매번 특정한 방식으로 JOIN / 서브쿼리를 사용해서 구성해야 하는 테이블이 있다면, 미리 VIEW로 저장해두고 쓰면 좋다

```sql
-- VIEW 생성해두기
CREATE VIEW three_tables_joined AS
SELECT i.id, i.name, AVG(star) AS avg_star, COUNT(*) AS count_star
FROM item AS i LEFT OUTER JOIN review AS r ON r.item_id = i.id
	LEFT OUTER JOIN member AS m ON r.mem_id = m.id
WHERE m.gender = 'f'
GROUP BY i.ithree_tables_joinedd, i.name
HAVING COUNT(*) >= 2
ORDER BY AVG(star) DESC, COUNT(*) DESC;
```
→ 위와 같이 view를 생성해두면, 서브쿼리를 중첩해서 써야 했던 구문을 아래와 같이 짧게 쓸 수 있다:
```sql
SELECT * FROM three_tables_joined
WHERE avg_star = (
    SELECT MAX(avg_star) FROM three_tables_joined);
-- VIEW는 다른 평범한 table처럼 가져다 사용하면 된다
```



