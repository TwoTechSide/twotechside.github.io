---
title: "퀘스트 - 달리기반 Lv4. 단골 고객님 찾기"
date: 2025-06-16 16:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제

- Orders 테이블:   
  
| OrderID | CustomerID | OrderDate  | TotalAmount |
| ------- | ---------- | ---------- | ----------- |
| 101     | 1          | 2024-01-01 | 150         |
| 102     | 2          | 2024-01-03 | 200         |
| 103     | 1          | 2024-01-04 | 300         |
| 104     | 3          | 2024-01-04 | 50          |
| 105     | 2          | 2024-01-05 | 80          |
| 106     | 4          | 2024-01-06 | 400         |

- Customers 테이블:   

| CustomerID | CustomerName | Country |
| ---------- | ------------ | ------- |
| 1          | Alice        | USA     |
| 2          | Bob          | UK      |
| 3          | Charlie      | USA     |
| 4          | David        | Canada  |

- 요구사항:
    - 고객별로 주문 건수와 총 주문 금액을 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 출력 결과에는 고객 이름, 주문 건수, 총 주문 금액이 포함되어야 합니다. 단, 주문을 한 적이 없는 고객도 결과에 포함되어야 합니다.   
        - b. 기대 결과   
        
        | CustomerName | OrderCount | TotalSpent |
        | ------------ | ---------- | ---------- |
        | Alice        | 2          | 450        |
        | Bob          | 2          | 280        |
        | Charlie      | 1          | 50         |
        | David        | 1          | 400        |

    - 나라별로 총 주문 금액이 가장 높은 고객의 이름과 그 고객의 총 주문 금액을 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 기대 결과   

        | Country | Top_Customer | Top_Spent |
        | ------- | ------------ | --------- |
        | USA     | Alice        | 450       |
        | UK      | Bob          | 280       |
        | Canada  | David        | 400       |

**제약사항:**
- 두 쿼리 모두 서브쿼리, `JOIN`, `GROUP BY`, `HAVING` 등을 사용해 풀 수 있어야 한다.   
- 주문을 한 적이 없는 고객도 첫 번째 쿼리 결과에 포함되어야 한다.   

<br><br>

- - -
## 문제 풀이   

- 고객별로 주문 건수와 총 주문 금액을 조회하는 SQL 쿼리를 작성해주세요.
```sql
select c.CustomerName CustomerName,
       count(o.OrderID) OrderCount,
       coalesce(sum(o.TotalAmount), 0) TotalSpent
from Orders o left join Customers c on o.CustomerID=c.CustomerID
group by c.CustomerName;
```   
   
- 나라별로 총 주문 금액이 가장 높은 고객의 이름과 그 고객의 총 주문 금액을 조회하는 SQL 쿼리를 작성해주세요.
```sql
select c.Country Country,
       c.CustomerName Top_Customer,
       sum(o.TotalAmount) Top_Spent
from Orders o inner join Customers c on o.CustomerID=c.CustomerID
group by c.Country, c.CustomerName
having sum(o.TotalAmount) = (
    select max(SumSpent)
    from (select sum(o2.TotalAmount) SumSpent
          from Customers c2 inner join Orders o2 on c2.CustomerID=o2.CustomerID
          where c2.Country=c.Country
          group by c2.CustomerID
    ) Subquery
);
```

<br>
   
### HAVING과 WHERE의 차이   
   
쿼리문의 작동 순서를 살펴보도록 합시다   

- **FROM**
- **ON, JOIN**
- **WHERE**
- **GROUP BY**
- **HAVING**
- **SELECT**
- **DISTINCT**
- **ORDER BY**
   
`WHERE`과 `HAVING`의 순서가 `GROUP BY`의 앞뒤로 차이가 있음을 볼 수 있습니다   
`WHERE`의 경우 `GROUP BY`를 통해 사용된 집계함수를 조건으로 사용할 수 없으며, 반대로 `HAVING`은 조건으로 사용할 수 있게 됩니다   

<br>

그러면 위 내용을 바탕으로 2번 문제의 SQL문을 이해해보도록 합시다   
   
1. `INNER JOIN`으로 테이블을 모아준 뒤 `GROUP BY`로 데이터를 그룹화 해줍니다   
2. `HAVAING`으로 각 데이터의 `TotalAmount`가 다음 조건의 값과 맞는 경우로 조건을 걸어줍니다   
   1. 서브 쿼리에서 테이블을 모아준 뒤 메인 쿼리문의 `c.Country`와 일치하는 데이터로 조건을 걸어줍니다
   2. `CustomerID`로 그룹화 해준 다음 총 주문 금액 조회합니다   
3. (2.)에 따라 각국에서 가장 주문 금액이 높은 고객의 이름과 총 주문 금액을 조회합니다   
   
<br>

오늘 처음 보게 된 `HAVING`절은 이 문제 하나로는 완벽하게 이해하기가 어려웠으니 복습을 해둡시다   