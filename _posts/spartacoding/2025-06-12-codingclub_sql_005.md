---
title: "[MySQL] SQL 심화2"
date: 2025-06-12 14:00:00 +0900
categories: [Sparta Coding, MySQL]
tags: [Sparta Coding, MySQL]
---

SQL의 마지막 강의를 듣는 날이네요, 저번에는 아래와 같은 내용들을 배웠습니다   
- `SUBQUERY`
- `JOIN`
   
오늘은 새로운 키워드를 배우는 것보다 문제 해결 능력을 키우는 방향으로 가보겠습니다      

<br>
- - -

## 조회한 데이터에 없는 값이 있다면   

SQL을 사용하다보면 일부 데이터가 없는 경우가 있습니다   
그런데 데이터가 없는 것까지 표시가 된 상태에서 계산을 하려면 문제가 생길 수 있죠   
   
이러한 상황을 해결하는 두 가지 방법을 알아보도록 합시다   

<br>

### 1. 없는 값을 제외하고 표시하기   
   
```sql
select restaurant_name,
       avg(rating) average_of_rating,
       avg(if(rating<>'Not given', rating, null)) average_of_rating2
from food_orders
group by 1;
```
예를 들면 위 쿼리의 `avg(if(rating<>'Not given', rating, null))` 내용을 살펴봅시다   
1. `rating`이 `Not given`이 아닌 경우 그대로
2. `rating`이 `Not given`인 경우 `NULL`로 변환
3. 이후 평균값 계산   
   
바로 평균을 작성했던 `average_of_rating`은 `Not given`까지 포함해서 평균값을 계산하는데, 이 때 `Not given`은 계산되는 데이터가 아니기에 0으로 변환되어 평균값을 계산하게 됩니다   
하지만 `NULL`로 변환시켜서 평균값을 구한다면 `Not given`이었던 데이터는 제외한 상태로 평균값을 계산할 수 있게 됩니다   

```sql
select a.order_id,
       a.customer_id,
       a.restaurant_name,
       a.price,
       b.name,
       b.age,
       b.gender
from food_orders a left join customers b on a.customer_id=b.customer_id
where b.customer_id is not null;
```
- 만약 `NULL`이 포함된 데이터를 제외하고 선택하고 싶다면 `WHERE`에 `... IS NOT NULL`을 추가해서 해당 값이 NULL인 데이터를 제외할 수 있습니다   

<br>

### 2. 다른 값으로 대체하기   
앞에 배웠던 내용을 바꿔 `IF`문에 데이터가 없는 경우 특정 값으로 바꾸는 방법도 있고   
`COALESCE`를 사용하는 방법도 있습니다   
   
```sql
select a.order_id,
       a.customer_id,
       a.restaurant_name,
       a.price,
       b.name,
       b.age,
       coalesce(b.age, 20) "null 제거",
       b.gender
from food_orders a left join customers b on a.customer_id=b.customer_id
where b.age is null;
```
- `coalesce(b.age, 20)`처럼 작성할 경우 `age`가 값이 없더라도 20으로 대체된 값이 선택됩니다   

<br>

## 상식적이지 않은 데이터가 있는 경우   
   
여기서 말하는 '상식적이지 않은 데이터'는 편의점에 계산한 고객의 나이가 2세거나 이메일 작성 시기가 1250년이거나...   
이렇게 전혀 말이 안되는 데이터를 말합니다   
   
```sql
select customer_id, name, email, gender, age,
       case when age<15 then 15
            when age>80 then 80
            else age end "범위를 지정해준 age"
from customers;
```
- 값의 범위를 지정하고, 범위를 벗어나면 다른 값으로 대체해서 해결할 수 있습니다   

<br>
- - -

## SQL로 Pivot Table 만들어보기   

|          | 1시 | 2시 | 3시 | 4시 |
| -------- | --- | --- | --- | --- |
| 10월 1일 | 5   | 3   | 5   | 2   |
| 10월 2일 | 7   | 10  | 1   | 8   |
| 10월 3일 | 3   | 3   | 9   | 4   |
| 10월 4일 | 9   | 16  | 10  | 1   |

Pivot Table은 위처럼 2개 이상의 기준으로 데이터를 집계할 때, 보기 쉽게 배열하여 보여주는 것을 의미합니다   

<br>
   
### 실습 1. 음식점별 시간별 주문건수 Pivot Table 뷰 만들기   

> 조건 : 15~20시 사이, 20시 주문건수 기준 내림차순   

<br>

```sql
select restaurant_name,
       max(if(hh='15', cnt_order, 0)) "15",
       max(if(hh='16', cnt_order, 0)) "16",
       max(if(hh='17', cnt_order, 0)) "17",
       max(if(hh='18', cnt_order, 0)) "18",
       max(if(hh='19', cnt_order, 0)) "19",
       max(if(hh='20', cnt_order, 0)) "20"
from (select a.restaurant_name,
             substring(b.time, 1, 2) hh,
             count(1) cnt_order
      from food_orders a inner join payments b on a.order_id=b.order_id
      where substring(b.time, 1, 2) between 15 and 20
      group by 1, 2
) a
group by 1
order by 7 desc;
```

- 우선 `food_orders`와 `payments`를 `INNER JOIN`하고, 조건에 맞는 데이터를 선택합  
- 그리고 group by를 통해 `restaurant_name`과 시간을 작성한 `hh`, 2개로 그룹화   
- 위 내용으로 만들어진 서브 쿼리를 통해 `SELECT`를 작성   

> 만약 데이터가 없어 `NULL`이 나오는 경우를 피하기 위해 `MAX`를 사용하였습니다   
   
- 마지막으로 `restaurant_name`으로 그룹을 지어준 뒤 조건에 맞게 20시 주문건수 기준 내림차순을 적용   
   
서브 쿼리로 조건에 맞는 '음식점별 시간별 주문건수'를 모아준 뒤, 바깥쪽에서 Pivot Table을 완성하면 됩니다   

<br>

### 실습 2. 성별, 연령별 주문건수 Pivot Table 뷰 만들기   

> 조건 : 나이는 10~59세 사이, 연령 순으로 내림차순   

<br>

```sql
select age,
       max(if(gender='male', order_count, 0)) male,
       max(if(gender='female', order_count, 0)) female
from (select gender,
             case when age between 10 and 19 then 10
                  when age between 20 and 29 then 20
                  when age between 30 and 39 then 30
                  when age between 40 and 49 then 40
                  when age between 50 and 59 then 50 end age,
             count(*) order_count
      from food_orders f inner join customers c on f.customer_id=c.customer_id
      where age between 10 and 59
      group by 1, 2
) a
group by 1
order by 1;
```
- 조건에 맞는 `gender`, `age`를 그룹화 한 서브 쿼리를 작성합니다   
- 그리고 `age`로 그룹화 하여 Pivot Table을 완성하면 됩니다   

<br>

두 번 실습을 해보니 서브 쿼리에서 2개의 조건으로 그룹화한 데이터를 서브 쿼리로 만든 다음, 바깥쪽에서 Pivot Table로 완성시켜주는 것을 알 수 있었습니다   

<br>
- - -

## RANK, SUM / 윈도우 함수 (Window Function)

Window Function은 각 행의 관계를 정의하기 위한 함수로 그룹 내의 연산을 쉽게 만들어줍니다   
내용이 꽤 길기 때문에 천천히 이해하는 것이 중요해보입니다   

<br>

### RANK / 순위를 매겨주는 기능   

```sql
select cuisine_type,
       restaurant_name,
       cnt_order,
       ranking
from (select cuisine_type,
             restaurant_name,
             cnt_order,
             rank() over (partition by cuisine_type order by cnt_order desc) ranking
      from (select cuisine_type,
                   restaurant_name,
                   count(1) cnt_order
            from food_orders
            group by 1, 2
      ) a
 ) b
 where ranking<=3;
```

`rank() over (partition by cuisine_type order by cnt_order desc) ranking`라는 복잡한 문장이 있는데   
차근차근 하나씩 뜯어보겠습니다   
   
- `rank() over` : 랭킹을 정렬   
- `partition by` : 어떤 단위로 묶어줄 것인지   
- `order by` : 어떤 것의 순서로 정렬할 것인지   

즉, `cuisine_type`별로 `cnt_order`에 따라 랭킹을 3위까지 각각 표시해줍니다   
[ 한식 레스토랑 Top3, 중식 레스토랑 Top3, ... ] 이렇게 표시가 되겠습니다   

<br>

### SUM / 전체에서 차지하는 비율, 누적합을 구할 때   

```sql
select cuisine_type,
       restaurant_name,
       cnt_order,
       sum(cnt_order) over (partition by cuisine_type) sum_cuisine,
       sum(cnt_order) over (partition by cuisine_type order by cnt_order) cum_cuisine 
from (select cuisine_type,
             restaurant_name,
             count(*) cnt_order
      from food_orders
      group by 1, 2
) a
order by cuisine_type, cnt_order;
```

이번에는 `SUM`을 사용했는데 2가지 방법을 사용해서 각각 `sum_cuisine`, `cum_cuisine`을 표시했습니다   
   
- `sum(cnt_order) over (partition by cuisine_type)`   
  `cuisine_type`에 따라 `cnt_order`의 합을 보여줍니다   
   
- `sum(cnt_order) over (partition by cuisine_type order by cnt_order)`   
  `cuisine_type`에 따라 `cnt_order`의 오름차순 누적값을 보여줍니다   

만약 `ORDER BY`를 붙이지 않으면 `cuisine_type`에 따라 `cnt_order`의 합계만 보여주고   
붙였을 경우 `cuisine_type`에 따라 0부터 `cnt_order`이 하나씩 더해지는 값이 표시되는 것을 알 수 있습니다   

<br>
- - -

## 포맷 함수 / 날짜 포맷과 조건까지   
   
이번에는 `2025-06-12`같은 날짜 형식을 다루는 함수를 다뤄보겠습니다   

```sql
select date(date) date_type,
       date_format(date(date), '%Y') "년",  -- Y 또는 y (각각 4자리, 2자리 표시)
       date_format(date(date), '%m') "월",  -- M 또는 m
       date_format(date(date), '%d') "일",  -- d 또는 e
       date_format(date(date), '%w') "요일" -- w
from payments;
```
- 만약 `date_type`의 값이 '1978-08-23'일 경우, [년, 월, 일, 요일]에 각각 [1978, 08, 23, 3]이 표시됩니다   
  요일은 일요일을 시작으로 0부터 6까지 나타납니다   
   
<br>

```sql
select date_format(date(p.date), '%Y') "년",
       date_format(date(p.date), '%m') "월",
       date_format(date(p.date), '%Y - %m') "년-월",
       count(*) "주문건수"
from food_orders f inner join payments p on f.order_id=p.order_id
where date_format(date(p.date), '%m')='03'
group by 1, 2, 3
order by 1;
```
- 데이터 형식을 `%Y - %m` 같이 사용할 수 있습니다, 이러면 '년 - 월'이 표시됩니다   