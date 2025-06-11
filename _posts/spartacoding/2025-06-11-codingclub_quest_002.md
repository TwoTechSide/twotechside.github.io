---
title: "퀘스트 - 걷기반 1) 돈을 벌기 위해 일을 합시다!"
date: 2025-06-11 15:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
아래와 같은 sparta_employees(직원) 테이블이 있습니다.

| id  | name   | position | salary | hire_date  |
| --- | ------ | -------- | ------ | ---------- |
| 1   | 르탄이 | 개발자   | 30000  | 2022-05-01 |
| 2   | 배캠이 | PM       | 40000  | 2021-09-25 |
| 3   | 구구이 | 파트장   | 35000  | 2023-06-01 |
| 4   | 이션이 | 팀장     | 50000  | 2021-07-09 |

1. `sparta_employees`테이블에서 모든 직원의 이름(name)과 직급(position)을 선택하는 쿼리를 작성해주세요.
2. `sparta_employees`테이블에서 중복 없이 모든 직급(position)을 선택하는 쿼리를 작성해주세요.
3. `sparta_employees`테이블에서 연봉(salery)이 40000과 60000 사이인 직원들을 선택하는 쿼리를 작성해주세요.
4. `sparta_employees`테이블에서 입사일(hire_date)이 2023년 1월 1일 이전인 모든 직원들을 선택하는 쿼리를 작성해주세요.

<br><br>

- - -
## 문제 풀이

1. `sparta_employees`테이블에서 모든 직원의 이름(name)과 직급(position)을 선택하는 쿼리를 작성해주세요.
```sql
select name,
       position
from sparta_employees;
```

<br>

2. `sparta_employees`테이블에서 중복 없이 모든 직급(position)을 선택하는 쿼리를 작성해주세요.
```sql
select distinct position
from sparta_employees;
```
- 중복 없이는 `DISTINCT`를 사용하면 됩니다   

<br>

3. `sparta_employees`테이블에서 연봉(salery)이 40000과 60000 사이인 직원들을 선택하는 쿼리를 작성해주세요.   
```sql
select *
from sparta_employees
where salery between 40000 and 60000;
```
- `where`과 `between .. and ..`로 필터링을 해주면 됩니다   

<br>

4. `sparta_employees`테이블에서 입사일(hire_date)이 2023년 1월 1일 이전인 모든 직원들을 선택하는 쿼리를 작성해주세요.   
```sql
select *
from sparta_employees
where hire_data<'2023-01-01';
```
- '년도-달-일'을 비교할 때 문자 비교를 해주면 되겠습니다   