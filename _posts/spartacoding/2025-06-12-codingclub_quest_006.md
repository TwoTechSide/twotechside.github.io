---
title: "퀘스트 - 걷기반 6) 팀 프로젝트 열심히 했으니 다시 놀아볼까요?!"
date: 2025-06-12 16:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
아래와 같은 lol_users(LOL 유저 테이블)이 있습니다.

| id  | name   | region | rating | join_date  |
| --- | ------ | ------ | ------ | ---------- |
| 1   | 르탄이 | 한국   | 1300   | 2019-06-15 |
| 2   | 배캠이 | 미국   | 1500   | 2020-09-01 |
| 3   | 구구이 | 한국   | 1400   | 2021-01-07 |
| 4   | 이션이 | 미국   | 1350   | 2019-11-15 |


<span>21.</span> `lol_users`테이블에서 각 유저의 레이팅(rating) 순위를 계산하는 쿼리를 작성해주세요!   
전체 지역(region) 기준이고 순위는 레이팅이 높을수록 높아야해요. (e.g. rating 1400 유저의 순위 > rating 1350 유저의 순위)   
<span>22.</span> `lol_users`테이블에서 가장 늦게 게임을 시작한(join_date) 유저의 이름을 선택하는 쿼리를 작성해주세요!   
<span>23.</span> `lol_users`테이블에서 지역별로 레이팅이 높은 순으로 유저들을 정렬해서 나열하는 쿼리를 작성해주세요!   
<span>24.</span> `lol_users`테이블에서 지역별로 평균 레이팅을 계산하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>21.</span> `lol_users`테이블에서 각 유저의 레이팅(rating) 순위를 계산하는 쿼리를 작성해주세요!   
전체 지역(region) 기준이고 순위는 레이팅이 높을수록 높아야해요. (e.g. rating 1400 유저의 순위 > rating 1350 유저의 순위)   

```sql
select id,
       name,
       region,
       rating,
       join_date,
       rank() over (order by rating desc) lol_rank
from lol_users;
```

<br>

<span>22.</span> `lol_users`테이블에서 가장 늦게 게임을 시작한(join_date) 유저의 이름을 선택하는 쿼리를 작성해주세요!   

```sql
select name
from lol_users
order by join_date desc
limit 1;
```
- `LIMIT`을 통해 정해진 숫자만큼의 데이터만 추출할 수 있습니다   

<br>

<span>23.</span> `lol_users`테이블에서 지역별로 레이팅이 높은 순으로 유저들을 정렬해서 나열하는 쿼리를 작성해주세요!   

```sql
select *
from lol_users
order by region, reating desc;
``` 

<br>

<span>24.</span> `lol_users`테이블에서 지역별로 평균 레이팅을 계산하는 쿼리를 작성해주세요!   

```sql
select regison,
       avg(rating) avg_rating
from lol_users
group by region;
```