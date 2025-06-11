---
title: "퀘스트 - 걷기반 2) 이제 좀 벌었으니 flex 한 번 해볼까요?!"
date: 2025-06-10 14:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
여러분이 구매하고 싶은 상품들의 정보가 있는 products(상품) 테이블이 아래에 있습니다.

| id  | product_name    | price | category |
| --- | --------------- | ----- | -------- |
| 1   | 맥북 프로       | 1200  | 컴퓨터   |
| 2   | 다이슨 청소기   | 300   | 생활가전 |
| 3   | 갤럭시탭        | 600   | 컴퓨터   |
| 4   | 드롱기 커피머신 | 200   | 주방가전 |

<span>5.</span> `products`테이블에서 제품 이름(product_name)과 가격(price)만을 선택하는 쿼리를 작성해주세요.   
<span>6.</span> `products`테이블에서 제품 이름에 '프로'가 포함된 모든 제품을 선택하는 쿼리를 작성해주세요.   
<span>7.</span> `products`테이블에서 제품 이름이 '갤'로 시작하는 모든 제품을 선택하는 쿼리를 작성해주세요.   
<span>8.</span> `products`테이블에서 모든 제품을 구매하기 위해 필요한 돈을 계산하는 쿼리를 작성해주세요.   

<br><br>

- - -
## 문제 풀이

<span>5.</span> `products`테이블에서 제품 이름(product_name)과 가격(price)만을 선택하는 쿼리를 작성해주세요.   
```sql
select product_name, price
from products;
```
   
<span>6.</span> `products`테이블에서 제품 이름에 '프로'가 포함된 모든 제품을 선택하는 쿼리를 작성해주세요.   
```sql
select *
from products
where product_name like '%프로%';
```
> 답안은 `SELECT id, product_name, price, category`로 되어있는데 `SELECT *`로 작성해도 같은 결과입니다   
   
<span>7.</span> `products`테이블에서 제품 이름이 '갤'로 시작하는 모든 제품을 선택하는 쿼리를 작성해주세요.   
```sql
select *
from products
where product_name like '갤%';
```
   
<span>8.</span> `products`테이블에서 모든 제품을 구매하기 위해 필요한 돈을 계산하는 쿼리를 작성해주세요.   
```sql
select sum(price) as total_price
from products;
```