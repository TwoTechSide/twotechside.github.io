---
title: "퀘스트 - 걷기반 10) 이젠 테이블이 2개입니다"
date: 2025-06-13 15:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
다음과 같은 직원(employees) 테이블과 부서(departments) 테이블이 있습니다.   

- employees 테이블   
  
| id  | department_id | name   |
| --- | ------------- | ------ |
| 1   | 101           | 르탄이 |
| 2   | 102           | 배캠이 |
| 3   | 103           | 구구이 |
| 4   | 101           | 이션이 |

- departments 테이블   

| id  | name     |
| --- | -------- |
| 101 | 인사팀   |
| 102 | 마케팅팀 |
| 103 | 기술팀   |
   
<span>38.</span> 현재 존재하고 있는 총 부서의 수를 구하는 쿼리를 작성해주세요!   
<span>39.</span> 모든 직원과 그들이 속한 부서의 이름을 나열하는 쿼리를 작성해주세요!   
<span>40.</span> '기술팀' 부서에 속한 직원들의 이름을 나열하는 쿼리를 작성해주세요!   
<span>41.</span> 부서별로 직원 수를 계산하는 쿼리를 작성해주세요!   
<span>42.</span> 직원이 없는 부서의 이름을 찾는 쿼리를 작성해주세요!   
<span>43.</span> '마케팅팀' 부서에만 속한 직원들의 이름을 나열하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>38.</span> 현재 존재하고 있는 총 부서의 수를 구하는 쿼리를 작성해주세요!   

```sql
select *
from departments;
```

<br>

<span>39.</span> 모든 직원과 그들이 속한 부서의 이름을 나열하는 쿼리를 작성해주세요!   

```sql
select e.name,
       d.name
from employees e inner join departments d on e.department_id=d.id;
```

<br>

<span>40.</span> '기술팀' 부서에 속한 직원들의 이름을 나열하는 쿼리를 작성해주세요!   

```sql
select e.name
from employees e inner join departments d on e.department_id=d.id
where d.name='기술팀';
``` 

<br>

<span>41.</span> 부서별로 직원 수를 계산하는 쿼리를 작성해주세요!   

```sql
select d.name,
       count(e.id)
from departments d left join employees e on d.id=e.department_id
group by d.id;
```

<br>

<span>42.</span> 직원이 없는 부서의 이름을 찾는 쿼리를 작성해주세요!   

```sql
select d.name
from departments d left join employees e on d.id=e.department_id
where e.id is null;
```

<br>

<span>43.</span> '마케팅팀' 부서에만 속한 직원들의 이름을 나열하는 쿼리를 작성해주세요!   

```sql
select e.name
from employees e inner join departments d on e.department_id=d.id
where d.name='마게팅팀';
```