---
title: "[MySQL] SQL 기초 문법 정리"
date: 2025-06-09 18:00:00 +0900
categories: [Sparta Coding, MySQL]
tags: [Sparta Coding, MySQL]
---

## SQL이란
<span style="color:grey">SQL : Structured Query Language</span>   
SQL은 관계형 데이터베이스에 정보를 저장하고 처리하기 위한 프로그래밍 언어입니다   
기본적인 SQL 명령어를 써보면서 직접 데이터를 조회해보도록 합시다   
<br>
- - -

### SELECT, FROM
> 데이터가 저장되어있는 테이블은 엑셀과 유사한 구조로 되어있으며, 칼럼(필드)는 테이블에 있는 열을 의미   

SELECT : 데이터를 가져오는 명령어   
FROM : 데이터를 가져올 테이블을 특정해주는 문법   
   
```sql
-- food_orders 테이블에 있는 모든 데이터를 조회
select * from food_orders;
```
select 다음 ' * '이 있으면 모든 데이터를 가져오겠다는 것을 의미합니다   
   
```sql
-- food_orders 테이블에 있는 order_id, restaurant_name 데이터만 조회
select order_id, restaurant_name
from food_orders;
```
만약 food_orders 테이블에 있는 특정 칼럼만 조회하고 싶다면 * 대신 속성 이름을 사용하면 됩니다  
<br> 
   
### WHERE   
음식점에 있는 모든 주문 내역중 일정 값 이상만, 또는 어떤 고객이 주문했는지 등등   
원하는 조건을 걸어 데이터를 조회할 수 있도록 필터링해주는 문법입니다   

```sql
select *
from food_orders
where price > 30000;
```
   
* 필터링 표현식   
   
|---|---|---|
|=|같다|age=21<br>female='gender'|
|<>|같지 않다(다르다)|age<>21<br>gender<>'female'|
|< <br> >|보다 작다<br>보다 크다|age<21<br>age>21|
|<= <br> >=|작거나 같다<br>크거나 같다|age<=21 <br> age>=21|

<br> 
   
#### BETWEEN, IN, LIKE   
```sql
where age between 10 and 20
-- age가 10이상, 20이하일 경우
 
where cuisine_type in ('Korean', 'Japanese')
-- cuisine_type이 ('Korean', 'Japanese')에 속한 경우

where restaurant_name like '%Next%'
-- restaurant_name에 'Next'가 포함된 경우
```
   
만약 '문자열'의 앞에 %가 붙으면 '문자열'이 끝나는 경우, 뒤에 붙으면 '문자열'로 시작되는 경우를 의미합니다   
앞뒤 둘 다 붙으면 '문자열'이 포함되는 경우라고 볼 수 있겠습니다   
<br> 
   
### WHERE 심화, 논리 연산
AND, OR, NOT을 사용하면 WHERE문에 필터를 더욱 다양하게 사용할 수 있습니다   

* 필터링 논리연산산   

|---|---|---|
|AND|그리고|age>20 **AND** gender='female'|
|OR|또는|age>20 **OR** gender='female'|
|NOT|아닌|**NOT** gender='female'|
   
