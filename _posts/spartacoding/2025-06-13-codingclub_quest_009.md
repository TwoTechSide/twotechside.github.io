---
title: "퀘스트 - 걷기반 9) 아프면 안됩니다! 항상 건강 챙기세요!"
date: 2025-06-13 14:30:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
의사가 있으면 당연히 의사에게 진료받는 환자가 있겠죠? 아래와 같은 patients(환자) 테이블이 있습니다.   

| id  | name   | birth_date | gender | last_visit_date |
| --- | ------ | ---------- | ------ | --------------- |
| 1   | 르탄이 | 1985-04-12 | 남자   | 2023-03-15      |
| 2   | 배캠이 | 1990-08-05 | 여자   | 2023-03-20      |
| 3   | 구구이 | 1982-12-02 | 여자   | 2023-02-18      |
| 4   | 이션이 | 1999-03-02 | 남자   | 2023-03-17      |
   
<span>34.</span> `patients` 테이블에서 각 성별(gender)에 따른 환자 수를 계산하는 쿼리를 작성해주세요!   
<span>35.</span> `patients` 테이블에서 현재 나이가 40세 이상인 환자들의 수를 계산하는 쿼리를 작성해주세요!   
<span>36.</span> `patients` 테이블에서 마지막 방문 날짜(last_visit_date)가 1년 이상 된 환자들을 선택하는 쿼리를 작성해주세요!   
<span>37.</span> `patients` 테이블에서 각 의사의 근무 기간을 계산하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>34.</span> `patients` 테이블에서 각 성별(gender)에 따른 환자 수를 계산하는 쿼리를 작성해주세요!   

```sql
select gender,
       count(*) cnt_patients
from patients
group by gender;
```

<br>

<span>35.</span> `patients` 테이블에서 현재 나이가 40세 이상인 환자들의 수를 계산하는 쿼리를 작성해주세요!   

```sql
select count(*) '40세 이상 환자'
from patients
where date_sub(curdate(), interval 40 year) >= birth_date;
```

<br>

<span>36.</span> `patients` 테이블에서 마지막 방문 날짜(last_visit_date)가 1년 이상 된 환자들을 선택하는 쿼리를 작성해주세요!   

```sql
select *
from patients
where date_sub(curdate(), interval 1 year) >= last_visit_date
``` 

<br>

<span>37.</span> `patients` 테이블에서 생년월일이 1980년대인 환자들의 수를 계산하는 쿼리를 작성해주세요!   

```sql
select count(*) '1980년대 환자'
from patients
where birth_date between '1980-01-01' and '1989-12-31';
```