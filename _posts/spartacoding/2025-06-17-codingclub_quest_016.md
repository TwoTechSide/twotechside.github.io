---
title: "퀘스트 - 달리기반 Lv4. 가장 높은 월급을 받는 직원은?"
date: 2025-06-17 14:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제

- Employees 테이블:   
  
| EmployeeID | Name    | Department | Salary | ManagerID |
| ---------- | ------- | ---------- | ------ | --------- |
| 1          | Alice   | HR         | 70000  | NULL      |
| 2          | Bob     | IT         | 90000  | 1         |
| 3          | Charlie | IT         | 80000  | 2         |
| 4          | David   | IT         | 85000  | 2         |
| 5          | Eve     | HR         | 75000  | 1         |
| 6          | Frank   | Finance    | 95000  | NULL      |
| 7          | Grace   | Finance    | 80000  | 6         |
| 8          | Heidi   | IT         | 95000  | 2         |
   

- **요구사항:**
    - 각 직원의 이름, 부서, 월급, 그리고 그 직원이 속한 부서에서 가장 높은 월급을 받고 있는 직원의 이름과 월급을 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 기대 결과   
        
        | Name    | Department | Salary | Top_Earner | Top_Salary |
        | ------- | ---------- | ------ | ---------- | ---------- |
        | Alice   | HR         | 70000  | Eve        | 75000      |
        | Bob     | IT         | 90000  | Heidi      | 95000      |
        | Charlie | IT         | 80000  | Heidi      | 95000      |
        | David   | IT         | 85000  | Heidi      | 95000      |
        | Eve     | HR         | 75000  | Eve        | 75000      |
        | Frank   | Finance    | 95000  | Frank      | 95000      |
        | Grace   | Finance    | 80000  | Frank      | 95000      |
        | Heidi   | IT         | 95000  | Heidi      | 95000      |


    - 부서별로 평균 월급이 가장 높은 부서의 이름과 해당 부서의 평균 월급을 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 기대 결과   

        | Department | Avg_Salary |
        | ---------- | ---------- |
        | IT         | 87500      |

**제약사항:**
- 두 쿼리 모두 서브쿼리, JOIN, GROUP BY, HAVING 등의 기능을 활용해주세요.   

<br><br>

- - -
## 문제 풀이   

- 문제1 - 각 직원의 이름, 부서, 월급, 그리고 그 직원이 속한 부서에서 가장 높은 월급을 받고 있는 직원의 이름과 월급을 조회하는 SQL 쿼리를 작성해주세요.   
```sql
select e1.Name Name,
       e1.Department Department,
       e1.Salary Salary,
       e2.Name Top_Earner,
       e2.Salary Top_Salary
from Employees e1 inner join Employees e2 on e1.Department=e2.Department
where e2.Salary = (
    select max(Salary)
    from Employees e3
    where e3.Department=e1.Department
);
```   
   
- 직원의 부서에 따라 다른 직원까지 탐색해야 하기에 `INNER JOIN`을 사용합니다   
- `WHERE`절로 다른 직원 중 가장 높은 월급을 받고 있는 직원을 탐색하여 조건을 걸어줍니다   

<br>

- 문제2 - 부서별로 평균 월급이 가장 높은 부서의 이름과 해당 부서의 평균 월급을 조회하는 SQL 쿼리를 작성해주세요.   
```sql
select Department,
       avg(Salary) Avg_Salary
from Employees
group by Department
having avg(Salary) = (
    select max(Avg_Salery2)
    from (
        avg(Salary) Avg_Salary2
        from Employees
        group by Department
    ) s1
);
```

- 각 부서별 평균 월급을 탐색한 뒤 가장 높은 값을 `HAVING`에서 조건으로 걸어줍니다   
   
사실 이 문제는 `ORDER BY`, `LIMIT`으로도 풀 수 있을 것 같은데 제약사항에 맞게 `HVAING` 등을 사용하여 풀었습니다   