---
title: "퀘스트 - 달리기반 Lv5. 예산이 가장 큰 프로젝트는? "
date: 2025-06-17 15:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제

- Employees 테이블:   

| EmployeeID | Name    | Department | Salary |
| ---------- | ------- | ---------- | ------ |
| 1          | Alice   | HR         | 5000   |
| 2          | Bob     | IT         | 7000   |
| 3          | Charlie | IT         | 6000   |
| 4          | David   | HR         | 4500   |
| 5          | Eve     | Sales      | 5500   |
| 6          | Frank   | IT         | 7200   |

- Products 테이블:   

| ProjectID | ProjectName | Budget |
| --------- | ----------- | ------ |
| 101       | Alpha       | 10000  |
| 102       | Beta        | 15000  |
| 103       | Gamma       | 12000  |
| 104       | Delta       | 8000   |

- EmployeeProjects 테이블:   

| EmployeeID | ProjectID |
| ---------- | --------- |
| 1          | 101       |
| 2          | 101       |
| 3          | 102       |
| 4          | 103       |
| 5          | 104       |
| 6          | 102       |
| 6          | 103       |

<br>

- **요구사항:**
    - 각 직원이 속한 부서에서 가장 높은 월급을 받는 직원들만 포함된 결과를 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 출력 결과에는 직원의 이름, 부서, 그리고 월급이 포함되어야 합니다.   
        - b. 기대 결과   
        
        | Name  | Department | Salary |
        | ----- | ---------- | ------ |
        | Alice | HR         | 5000   |
        | Frank | IT         | 7200   |
        | Eve   | Sales      | 5500   |

    - 직원이 참여한 프로젝트 중 예산이 10,000 이상인 프로젝트만을 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 출력 결과에는 직원 이름, 프로젝트 이름, 그리고 프로젝트 예산이 포함되어야 합니다.   

        | Name    | ProjectName | Budget |
        | ------- | ----------- | ------ |
        | Bob     | Beta        | 15000  |
        | Charlie | Beta        | 15000  |
        | Frank   | Beta        | 15000  |
        | David   | Gamma       | 12000  |
        | Frank   | Gamma       | 12000  |

<br>

**제약사항:**
- 두 쿼리 모두 서브쿼리를 사용해주세요.   
- 서브쿼리를 통해 특정 조건을 만족하는 데이터를 필터링하고, 그 결과를 최종 쿼리에 반영해야 합니다.

<br><br>

- - -
## 문제 풀이   

- 문제1 - 각 직원이 속한 부서에서 가장 높은 월급을 받는 직원들만 포함된 결과를 조회하는 SQL 쿼리를 작성해주세요.   

```sql
SELECT
    e.Name,
    e.Department,
    e.Salary
FROM
    Employees e
HAVING e.Salary = (
    SELECT
        MAX(Salary)
    FROM
        Employees e2
    WHERE
        e.Department=e2.Department
    );
```    

<br>

- 문제2 - 직원이 참여한 프로젝트 중 예산이 10,000 이상인 프로젝트만을 조회하는 SQL 쿼리를 작성해주세요.   

```sql
SELECT
    e.Name,
    p.ProjectName,
    p.Budget
FROM
    Employees e JOIN EmployeeProjects ep ON e.EmpolyeeID=ep.EmployeeID
                JOIN Projects p ON ep.ProejctID=Projects.ProjectID
WHERE
    p.Budget >= 10000;
```   

- 서브쿼리가 필요없는 문제에 해당됩니다   