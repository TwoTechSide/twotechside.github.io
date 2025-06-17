---
title: "[MySQL] SQL 심화1"
date: 2025-06-11 14:00:00 +0900
categories: [Sparta Coding, MySQL]
tags: [Sparta Coding, MySQL]
---

저번에 배운 내용의 키워드는 다음과 같습니다 
- `REPLACE`, `SUBSTRING(SUBSTR)`, `CONCAT`
- `IF`, `CASE`

`CASE WHEN ... THEN ... WHEN ... THEN ... ELSE ... END`는 조금 익숙해져야 될 것 같네요
   
이번에는 심화된 내용으로 배워봅시다   

<br>
- - -

## SUBQUERY

서브쿼리는 여러 번의 연산을 한 번의 SQL문으로 수행하는 **방법**입니다   
이때까지 `select`, `from` 등을 쓰며 완성했었는데, 서브로 쿼리를 작성해서 쿼리 안에 쿼리를 넣어 사용할 수 있습니다   
   
```sql
select order_id, restaurant_name, if(over_time>=0, over_time, 0) over_time
from 
(
select order_id, restaurant_name, food_preparation_time-25 over_time
from food_orders
) a;
```

> `FROM` 뒤에 서브쿼리를 사용할 경우, 모든 테이블은 이름이 있어야 하기 때문에에 반드시 별칭을 붙여야 함   

1. `food_orders`의 `order_id`, `restaurant_name`, `food_preparation_time-25 (->over_time)`가 있는 테이블 `a`가 서브 쿼리로 만들어짐
2. 테이블 `a`를 바탕으로 `order_id`, `restaurant_name`, `if(over_time>=0, over_time, 0)`를 표시   

즉, 서브 쿼리에서 계산이 이루어진 다음 다시 바깥쪽 쿼리에서 계산을 또 할 수 있음을 보여줍니다   

<br>
   
### SUBQUERY 실습
> [실습] 음식점의 평균 단가별 segmentation 을 진행하고, 그룹에 따라 수수료 연산하기   
> (수수료 구간 -   
> 　　　　~5000원 미만 0.5%   
> 　　　　~20000원 미만 1%   
> 　　　　~30000원 미만 2%   
> 　　　　 30000원 초과 3%)   

<br>

```sql
select restaurant_name,
       price_per_plate*ratio_of_add "수수료"
from (
    -- 서브쿼리1
     select restaurant_name,
            case when price_per_plate<5000 then 0.005
                 when price_per_plate between 5000 and 19999 then 0.01
                 when price_per_plate between 20000 and 29999 then 0.02
                 else 0.03 end ratio_of_add,
            price_per_plate
     from (
          -- 서브쿼리2
          select restaurant_name, avg(price/quantity) price_per_plate
          from food_orders
          group by 1
     ) a
) b;
```

서브쿼리를 사용하니 내용이 길어지며 복잡하게 느껴지지만 하나씩 살펴보면 이미 배웠던 내용들로도 충분히 이해할 수 있는 내용입니다   
   
1. `food_orders` 테이블에서 `restaurant_name`, `avg(price/quantity)`, `price_per_plate`가 있는 데이터를 추출
2. `a` 테이블에서 `restaurant_name`과 계산된 `price_per_plate`, 그리고 `price_per_plate`가 있는 데이터를 추출
3. `b` 테이블에서 `restaurant_name`, `price_per_plate*ratio_of_add`를 표시

순서를 보면 가장 안쪽 서브쿼리부터 바깥쪽 쿼리까지 진행되는 것을 알 수 있습니다   

<br>
- - -

## JOIN / 필요한 데이터가 서로 다른 테이블에 있을 경우   

SQL에 모든 데이터가 하나의 테이블에 있으란 법은 없습니다, 여러 테이블에 나뉘어져 있을 수도 있습니다   
예를 들면 고객의 정보가 담긴 테이블, 그리고 고객이 구매한 물건이 담긴 테이블같이 말이죠   
   
만약 어떤 물건을 구매한 고객의 정보를 얻기 위해서는 두 테이블을 같이 써야 할 필요가 있어보입니다   
어찌보면 엑셀에서 `VLOOKUP`함수와 비슷할 수도 있겠습니다   

<br>

### LEFT JOIN, INNER JOIN   

`LEFT JOIN` : 공통 칼럼(키값)을 기준으로, 하나의 테이블에 값이 없더라도 모두 조회되는 경우를 의미합니다   
`INNER JOIN` : 공통 칼럼(키값)을 기준으로, 두 테이블 모두에 있는 값만 조회합니다   
   
위 예시대로 만약 고객 정보와 해당 고객이 구매한 물건을 참조하고 싶을 때
- 물건을 구매한 적도 없는 고객의 정보까지 얻으려면 `LEFT JOIN`
- 물건을 구매한 적만 있는 고객의 정보를 얻으려면 `INNER JOIN`   
   
```sql
select *
from food_orders left join payments on food_orders.order_id=payments.order_id;
-- 음식 주문 테이블 food_orders에서 결제 정보 테이블 payments로 LEFT JOIN
```

이처럼 `LEFT JOIN`, `INNER JOIN`과 비슷하게 `RIGHT JOIN`, `OUTER JOIN`도 있습니다   
   
`RIGHT JOIN`은 `LEFT JOIN`과 반대로 고객의 정보가 없더라도 물건을 구매한 이력을 모두 참조하구요   
`OUTER JOIN`은 `LEFT JOIN`과 `RIGHT JOIN`의 합집합으로 보시면 됩니다   
그러면 `INNER JOIN`은 반대로 교집합으로 보면 되겠습니다   
   
<br>

### JOIN 실습   

#### 문제 1
> [실습] 고객의 주문 식당 조회하기   
> 조회 컬럼 : 고객 이름, 연령, 성별, 주문 식당   
> 고객명으로 정렬, 중복 없도록 조회   

<br>

```sql
select distinct c.name,
       c.age,
       c.gender,
       f.restaurant_name
from customers c left join food_orders f on c.customer_id=f.customer_id
order by c.name;
```
- `distinct c.name`을 작성하면 `select`에 표시된 데이터가 모두 겹쳤을 때 중복을 제거합니다   
- 무조건 이름만 겹치지 않도록 제거하는 것이 아니라는거에 헷갈리지 않도록 주의해야합니다   
   
<br>

#### 문제 2
> [실습] 50세 이상 고객의 연령에 따라 경로 할인율을 적용하고, 음식 타입별로 원래 가격과 할인 적용 가격 합을 구하기     
> 조회 컬럼 : 음식 타입, 원래 가격, 할인 적용 가격   
> 할인 : (나이-50)*0.005   
> 고객 정보가 없는 경우도 포함하여 조회, 할인 금액이 큰 순서대로 정렬   

<br>

```sql
select cuisine_type,
       sum(price) price,
       sum(price*discount_rate) discounted_price
from ( -- 서브쿼리
      select f.cuisine_type,
             f.price,
             c.age,
             (c.age-50)*0.005 discount_rate
      from food_orders f left join customers c on f.customer_id=c.customer_id
      where c.age>=50
) a
group by cuisine_type
order by discounted_price desc;
```

- 서브쿼리에서 `f.cuisine_type`을 표시했기 떄문에 바깥쪽 쿼리에서 `f.`을 쓰면 안됩니다   
  만약 테이블명을 붙여야 한다면 서브 쿼리의 테이블 명인 `a.`을 붙여야 합니다   