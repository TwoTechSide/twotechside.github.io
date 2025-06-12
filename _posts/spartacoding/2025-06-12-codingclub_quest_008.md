---
title: "퀘스트 - 걷기반 8) LOL을 하다가 홧병이 나서 병원을 찾아왔습니다."
date: 2025-06-12 16:40:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
이제, 아래와 같은 doctors(의사) 테이블이 있습니다.   

| id  | name   | major    | hire_date  |
| --- | ------ | -------- | ---------- |
| 1   | 르탄이 | 피부과   | 2018-05-10 |
| 2   | 배캠이 | 성형외과 | 2019-06-15 |
| 3   | 구구이 | 안과     | 2020-07-20 |


<span>30.</span> `doctors`테이블에서 전공(major)가 성형외과인 의사의 이름을 알아내는 쿼리를 작성해주세요!   
<span>31.</span> `doctors`테이블에서 각 전공 별 의사 수를 계산하는 쿼리를 작성해주세요!   
<span>32.</span> `doctors`테이블에서 현재 날짜 기준으로 5년 이상 근무(hire_date)한 의사 수를 계산하는 쿼리를 작성해주세요!   
<span>33.</span> `doctors`테이블에서 각 의사의 근무 기간을 계산하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>30.</span> `doctors`테이블에서 전공(major)가 성형외과인 의사의 이름을 알아내는 쿼리를 작성해주세요!  

```sql
select name
from doctors
where major='성형외과';
```

<br>

<span>31.</span> `doctors`테이블에서 각 전공 별 의사 수를 계산하는 쿼리를 작성해주세요!   

```sql
select name,
       count(*) cnt_doctor
from doctors
group by major;
```

<br>

<span>32.</span> `doctors`테이블에서 현재 날짜 기준으로 5년 이상 근무(hire_date)한 의사 수를 계산하는 쿼리를 작성해주세요!   

```sql
select count(*) num_of_docter
from doctors
where hire_date <= date_sub(curdate(), interval 5 year);
``` 
- `INTERVAL`은 일, 시간, 분 등 날짜 및 시간 값과 연산할 때 사용되는 문법입니다   
- `DATE_ADD`로 날을 더하거나 `DATE_SUB`로 뺄 수 있습니다   

<br>

<span>33.</span> `doctors`테이블에서 각 의사의 근무 기간을 계산하는 쿼리를 작성해주세요!   

```sql
select name,
       datediff(curdate(), hire_date) work_days
from doctors;
```
- `DATEDIFF(expr1, expr2)`는 `expr1 - expr2`를 계산합니다   