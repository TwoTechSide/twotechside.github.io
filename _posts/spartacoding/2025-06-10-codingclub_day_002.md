---
title: "[MySQL] SQL 기초 문법 정리2"
date: 2025-06-10 14:00:00 +0900
categories: [Sparta Coding, MySQL]
tags: [Sparta Coding, MySQL]
---

이전 글에서 다음 내용들을 배웠습니다   
- `SELECT`, `FROM`, `WHERE`
- WHERE문 조건 `<, >, =, <>`, `IN, BETWEEN, LIKE`, `AND, OR, NOT`
   
이어서 기본 문법을 조금 더 배워보도록 하겠습니다   

<br>
- - -

## 연산 기호 사용
```sql
select food_preparation_time,
       delivery_time,
       food_preparation_time + delivery_time as total_time -- 새로운 칼럼 total time 추가
from food_orders;
```
`food_preparation_time`과 `delivery_time`을 합한 값을 `total_time`이라는 새로운 칼럼으로 표시할 수 있게 됩니다   
`+`뿐만 아니라 `/`, `*`, `-`도 사용할 수 있습니다   

<br>
   
## SUM, AVG, COUNT, MIN, MAX
   
```sql
select sum(food_preparation_time) total_food_preparation_time,
       avg(delivery_time) avg_food_delivery_time
from food_orders;
```
`SUM`, `AVG`를 통해 데이터에 있는 `food_preparation_time`과 `delivery_time`을 각각 합계, 평균을 구할 수 있습니다

```sql
-- count()에 1 또는 *을 작성할 경우 모든 데이터의 개수 표시
-- distinct : 중복 제거를 의미하며 해당 칼럼의 고유한 데이터의 개수 표시
select count(1) count_of_orders,
       count(distinct customer_id) count_of_customers,
       min(price) min_price,
       max(price) max_price
from food_orders;
```
`count(distinct customer_id)`는 `customer_id` 칼럼의 고유값이 몇 개인지를 알아볼 수 있습니다   
그리고 `MIN`, `MAX`를 통해 해당 칼럼의 최솟값과 최댓값을 표시할 수 있습니다   
   
> 만약 SELECT에서 데이터 1개를 표시해주는 문법과 여러개를 표시해주는 문법 또는 칼럼명을 쓸 경우 데이터는 1개만 표시됨 / SELECT COUNT(*), price -> 전체 데이터 개수, price 첫 번째 값    

<br>
   
### WHERE문과 함께 응용하며 문제 풀기

1. 주문 금액(price)이 30,000원 이상인 주문건의 갯수 구하기
2. 한국 음식(cuisine_type)의 주문 당 평균 음식가격 구하기
   
```sql
-- 1번 문제
select count(*) as cnt_orders
from food_orders
where price > 30000;

-- 2번 문제
select avg(price) as avg_prices
from food_orders
where cuisine_type='Korean';
```

<br>
   
## GROUP BY로 범주별 연산 한 번에 끝내기
```sql
select cuisine_type,
       sum(price) sum_of_price
from food_orders
group by cuisine_type; --cuisine_type 별로 cuisine_type, price합계를 구할 수 있음
```

> GROUP BY는 from문 뒤에 작성

카테고리 칼럼을 지정해서 해당 데이터의 평균값, 최솟값 등을 구할 수 있는 방법입니다   
`SELECT`에 cuisine_type을 적지 않아도 작동은 하지만 표시가 되어야 구별할 수 있습니다   

### GROUP BY를 이용하여 문제 풀기

1. 음식점별 주문 금액 최댓값 조회하기
2. 결제 타입별 가장 최근 결제일 조회하기

```sql
-- 1번 문제
select restaurant_name,
	   max(price) max_price
from food_orders
group by restaurant_name;

-- 2번 문제
select pay_type,
	   max(date) recent_date
from payments
group by pay_type;
```

<br>
   
## ORDER BY로 Query 결과를 정렬하기   
```sql
select cuisine_type,
       sum(price) sum_of_price  -- 2. 음식 합계를 구한 뒤
from food_orders
group by cuisine_type           -- 1. cuisine_type별로
order by sum(price);            -- 3. 합계를 오름차순으로 정렬
```

> ORDER BY는 FROM, GROUP BY 뒤에 작성

```sql
select cuisine_type,
       sum(price) sum_of_price  -- 2. 음식 합계를 구한 뒤
from food_orders
group by cuisine_type           -- 1. cuisine_type별로
order by sum(price) desc;       -- 3. 합계를 내림차순으로 정렬
```
만약 내림차순으로 정렬할 경우, `ORDER BY ...`뒤에 `DESC`를 작성하면 됩니다   

### ORDER BY를 이용하여 문제 풀기

1. 음식점별 주문 금액 최댓값 조회하기 - 최댓값 기준으로 내림차순 정렬
2. 고객을 이름 순으로 오름차순으로 정렬하기

```sql
-- 1번 문제
select restaurant_name,
	   max(price) as "max price"
from food_orders
group by restaurant_name
order by max(price) desc;

-- 2번 문제
select *
from customers
order by name;
```
만약 성별과 이름 둘 다 정렬을 하고 싶다면 `ORDER BY`에 쉼표를 통해 추가할 수 있습니다   
<span style="color: grey">예시 : order by gender, name<span>   

<br>
- - -

## 이번 내용으로 문제 풀어보기   

문제 : 음식 종류(cuisine_type)별 가장 높은 주문 금액(price)과 가장 낮은 주문금액을 조회하고, 가장 낮은 주문금액 순으로 (내림차순) 정렬하기   

<br><br>

### 답안
```sql
select cuisine_type,
	   max(price),
	   min(price)
from food_orders
group by cuisine_type 
order by min(price) desc;
```
