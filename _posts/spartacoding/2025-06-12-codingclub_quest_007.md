---
title: "퀘스트 - 걷기반 7) 랭크게임 하다가 싸워서 피드백 남겼어요…"
date: 2025-06-12 16:30:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
아래와 같은 lol_users(LOL 유저 테이블)이 있습니다.

| id  | user_name | satisfaction_score | feedback_date |
| --- | --------- | ------------------ | ------------- |
| 1   | 르탄이    | 5                  | 2023-03-01    |
| 2   | 배캠이    | 4                  | 2023-03-02    |
| 3   | 구구이    | 3                  | 2023-03-01    |
| 4   | 이션이    | 5                  | 2023-03-03    |
| 5   | 구구이    | 4                  | 2023-03-04    |

<span>25.</span> `lol_feedbacks` 테이블에서 만족도 점수(satisfaction_score)에 따라 피드백을 내림차순으로 정렬하는 쿼리를 작성해주세요!   
<span>26.</span> `lol_feedbacks` 테이블에서 각 유저별로 최신 피드백을 찾는 쿼리를 작성해주세요!   
<span>27.</span> `lol_feedbacks` 테이블에서 만족도 점수가 5점인 피드백의 수를 계산하는 쿼리를 작성해주세요!   
<span>28.</span> `lol_feedbacks` 테이블에서 가장 많은 피드백을 남긴 상위 3명의 고객을 찾는 쿼리를 작성해주세요!   
<span>29.</span> `lol_feedbacks` 테이블에서 평균 만족도 점수가 가장 높은 날짜를 찾는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>25.</span> `lol_feedbacks` 테이블에서 만족도 점수(satisfaction_score)에 따라 피드백을 내림차순으로 정렬하는 쿼리를 작성해주세요!   

```sql
select *
from lol_feedbacks
order by satisfaction_score desc;
```

<br>

<span>26.</span> `lol_feedbacks` 테이블에서 각 유저별로 최신 피드백을 찾는 쿼리를 작성해주세요!   

```sql
select user_name,
       max(feedback_date)
from lol_feedbacks
group by user_name;
```

<br>

<span>27.</span> `lol_feedbacks` 테이블에서 만족도 점수가 5점인 피드백의 수를 계산하는 쿼리를 작성해주세요!   

```sql
select count(*)
from lol_feedbacks
where satisfaction_score=5;
``` 

<br>

<span>28.</span> `lol_feedbacks` 테이블에서 가장 많은 피드백을 남긴 상위 3명의 고객을 찾는 쿼리를 작성해주세요!   

```sql
select user_name,
       count(*) cnt_feedback
from lol_feedbacks
group by user_name
order by cnt_feedback desc
limit 3;
```

<br>

<span>29.</span> `lol_feedbacks` 테이블에서 평균 만족도 점수가 가장 높은 날짜를 찾는 쿼리를 작성해주세요!   

```sql
select feedback_date
from lol_feedbacks
group by feedback_date
order by avg(satisfaction_score) desc
limit 1;
```