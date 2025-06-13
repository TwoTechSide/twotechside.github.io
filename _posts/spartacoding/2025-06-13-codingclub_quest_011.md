---
title: "퀘스트 - 걷기반 마지막 연습 문제!"
date: 2025-06-13 16:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
다음과 같은 상품(products) 테이블과 주문(orders) 테이블이 있습니다.

- products 테이블
  
| id  | name   | price |
| --- | ------ | ----- |
| 1   | 랩톱   | 1200  |
| 2   | 핸드폰 | 800   |
| 3   | 타블렛 | 400   |

- orders 테이블   

| id  | product_id | quantity | order_date |
| --- | ---------- | -------- | ---------- |
| 101 | 1          | 2        | 2023-03-01 |
| 102 | 2          | 1        | 2023-03-02 |
| 103 | 3          | 5        | 2023-03-04 |
   
<span>44.</span> 모든 주문의 주문 ID와 주문된 상품의 이름을 나열하는 쿼리를 작성해주세요!   
<span>45.</span> 총 매출(price * quantity의 합)이 가장 높은 상품의 ID와 해당 상품의 총 매출을 가져오는 쿼리를 작성해주세요!   
<span>46.</span> 각 상품 ID별로 판매된 총 수량(quantity)을 계산하는 쿼리를 작성해주세요!   
<span>47.</span> 2023년 3월 3일 이후에 주문된 모든 상품의 이름을 나열하는 쿼리를 작성해주세요!   
<span>48.</span> 가장 많이 판매된 상품의 이름을 찾는 쿼리를 작성해주세요!   
<span>49.</span> 각 상품 ID별로 평균 주문 수량을 계산하는 쿼리를 작성해주세요!   
<span>50.</span> 판매되지 않은 상품의 ID와 이름을 찾는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>44.</span> 모든 주문의 주문 ID와 주문된 상품의 이름을 나열하는 쿼리를 작성해주세요!   

```sql
select o.id,
       p.name
from order o join products p on o.product_id=p.id;
```
- `JOIN`만 쓸 경우 `INNER JOIN`과 동일하게 작동합니다

<br>

<span>45.</span> 총 매출(price * quantity의 합)이 가장 높은 상품의 ID와 해당 상품의 총 매출을 가져오는 쿼리를 작성해주세요!   

```sql
select p.id,
       sum(p.price*o.quantity) total_sales
from orders o inner join products p on o.product_id=p.id
group by p.id
order by total_sales desc
limit 1
```

<br>

<span>46.</span> 각 상품 ID별로 판매된 총 수량(quantity)을 계산하는 쿼리를 작성해주세요!   

```sql
select p.id,
       sum(o.quantity) total_quantity
from products p inner join orders o on p.id=orders.product_id
group by p.id;
``` 

<br>

<span>47.</span> 2023년 3월 3일 이후에 주문된 모든 상품의 이름을 나열하는 쿼리를 작성해주세요!   

```sql
select p.name
from products p inner join orders o on p.id=orders.product_id
where o.order_date>='2023-03-03';
```

<br>

<span>48.</span> 가장 많이 판매된 상품의 이름을 찾는 쿼리를 작성해주세요!   

```sql
select p.name,
       sum(o.quantity) total_quantity
from products p inner join orders o on p.id=o.product_id
order by total_quantity desc
limit 1;
```

<br>

<span>49.</span> 각 상품 ID별로 평균 주문 수량을 계산하는 쿼리를 작성해주세요!   

```sql
select p.id,
       avg(o.quantity) avg_quantity
from products p inner join orders o on p.id=o.product_id
group by p.id;
```

<br>

<span>50.</span> 판매되지 않은 상품의 ID와 이름을 찾는 쿼리를 작성해주세요!   

```sql
select p.id,
       p.name
from products p left join orders o
where o.id is null;
```