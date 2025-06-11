---
title: "퀘스트 - 걷기반 5) 공부하다보니 팀 프로젝트 시간이 왔어요!"
date: 2025-06-11 17:30:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
공부를 한 결과를 점검하기 위해 팀 프로젝트를 수행해야 합니다! 이제, 아래와 같은 team_projects(프로젝트) 테이블이 있습니다. 

| id  | name       | start_date | end_date   | aws_cost |
| --- | ---------- | ---------- | ---------- | -------- |
| 1   | 일조       | 2023-01-01 | 2023-01-07 | 30000    |
| 2   | 꿈꾸는이조 | 2023-03-15 | 2023-03-22 | 50000    |
| 3   | 보람삼조   | 2023-11-20 | 2023-11-30 | 80000    |
| 4   | 사조참치   | 2022-07-01 | 2022-07-30 | 75000    |

<span>17.</span> `team_projects`테이블에서 AWS 예산(aws_cost)이 40000 이상 들어간 프로젝트들의 이름을 선택하는 쿼리를 작성해주세요!   
<span>18.</span> `team_projects`테이블에서 2022년에 시작된 프로젝트를 선택하는 쿼리를 작성해주세요! 단, **start_date < '2023-01-01'**조건을 <span style="color: red">사용하지 말고</span> 선택하는 쿼리를 작성해주세요!   
<span>19.</span> `team_projects`테이블에서 현재 진행중인 프로젝트를 선택하는 쿼리를 작성해주세요. 단 지금 시점의 날짜를 하드코딩해서 쿼리하지 말아주세요!   
<span>20.</span> `team_projects`테이블에서 각 프로젝트의 지속 기간을 일 수로 계산하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>17.</span> `team_projects`테이블에서 AWS 예산(aws_cost)이 40000 이상 들어간 프로젝트들의 이름을 선택하는 쿼리를 작성해주세요!   

```sql
select name
from team_projects
where aws_cost >= 40000;
```

<br>

<span>18.</span> `team_projects`테이블에서 2022년에 시작된 프로젝트를 선택하는 쿼리를 작성해주세요! 단, **start_date < '2023-01-01'**조건을 <span style="color: red">사용하지 말고</span> 선택하는 쿼리를 작성해주세요!   

```sql
select *
from team_projects
where year(start_date) = 2022;
```
- 날짜 데이터를 추출하는 함수중 `YEAR`을 사용해야 합니다   
- `YEAR`함수 외에도 `MONTH`, `DAY`도 있습니다

<br>

<span>19.</span> `team_projects`테이블에서 현재 진행중인 프로젝트를 선택하는 쿼리를 작성해주세요. 단 지금 시점의 날짜를 하드코딩해서 쿼리하지 말아주세요!   

```sql
select *
from team_projects
where curdate() between start_date and end_date;
```
- 현재 날짜를 `YYYY-MM-DD`로 반환하는 `CURDATE()`함수를 사용해야 합니다   
- `hh:mm:ss`를 반환하는 `CURTIME()`도 있으며, `NOW()`를 사용하면 `YYYY-MM-DD hh:mm:ss`가 반환됩니다   

<br>

<span>20.</span> `team_projects`테이블에서 각 프로젝트의 지속 기간을 일 수로 계산하는 쿼리를 작성해주세요!   

```sql
select name,
       datediff(day, start_date, end_date) as working_days
from sparta_students;
```
- `SELECT(인자, start, end)`함수로 시간을 계산할 수 있습니다   
- 인자에는 `HOUR`, `DAY`, `MONTH` 등이 들어갑니다   