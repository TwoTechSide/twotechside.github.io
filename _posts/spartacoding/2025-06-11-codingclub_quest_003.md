---
title: "퀘스트 - 걷기반 3) 상품 주문이 들어왔으니 처리해봅시다!"
date: 2025-06-11 16:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
이제 상품 주문이 들어왔으니 어떤 고객에게 어떤 주문이 들어왔는지를 파악할 수 있는 orders(주문) 테이블이 아래에 있습니다.

| id  | customer_id | product_id | amount | shipping_gee | order_date |
| --- | ----------- | ---------- | ------ | ------------ | ---------- |
| 1   | 719         | 1          | 3      | 50000        | 2023-11-01 |
| 2   | 131         | 2          | 1      | 10000        | 2023-11-02 |
| 3   | 65          | 4          | 1      | 20000        | 2023-11-05 |
| 4   | 1008        | 3          | 2      | 25000        | 2023-11-05 |
| 5   | 356         | 1          | 1      | 15000        | 2023-11-09 |

<span>9.</span> `orders`테이블에서 주문 수량(amount)이 2개 이상인 주문을 진행한 소비자의 ID(customer_id)만 선택하는 쿼리를 작성해주세요!   
<span>10.</span> `orders`테이블에서 2023년 11월 2일 이후에 주문된 주문 수량(amount)이 2개 이상인 주문을 선택하는 쿼리를 작성해주세요!   
<span>11.</span> `orders`테이블에서 주문 수량이 3개 미만이면서 배송비(shipping_fee)가 15000원보다 비싼 주문을 선택하는 쿼리를 작성해주세요!   
<span>12.</span> `orders`테이블에서 배송비가 높은 금액 순으로 정렬하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>9.</span> `orders`테이블에서 주문 수량(amount)이 2개 이상인 주문을 진행한 소비자의 ID(customer_id)만 선택하는 쿼리를 작성해주세요!
```sql
select customer_id
from orders
where amount>1;
```

<br>

<span>10.</span> `orders`테이블에서 2023년 11월 2일 이후에 주문된 주문 수량(amount)이 2개 이상인 주문을 선택하는 쿼리를 작성해주세요!
```sql
select *
from orders
where order_date>='2023-11-02' and amount>1;
```
- `AND`로 논리 필터링을 사용할 수 있습니다 

<br>

<span>11.</span> `orders`테이블에서 주문 수량이 3개 미만이면서 배송비(shipping_fee)가 15000원보다 비싼 주문을 선택하는 쿼리를 작성해주세요!
```sql
select *
from orders
where amount<3 and shipping_fee>15000;
```

<br>

<span>12.</span> `orders`테이블에서 배송비가 높은 금액 순으로 정렬하는 쿼리를 작성해주세요!
```sql
select *
from orders
order by shipping_fee desc;
```
- 정렬은 `ORDER BY`를 사용하면 되며, 오름차순이 기본이고 `DESC`로 내림차순 정렬을 할 수 있습니다