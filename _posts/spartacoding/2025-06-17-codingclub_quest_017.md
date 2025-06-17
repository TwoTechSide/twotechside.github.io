---
title: "퀘스트 - 달리기반 Lv5. 가장 많이 팔린 품목은?"
date: 2025-06-17 14:30:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제

- Products 테이블:   

| ProductID | ProductName  | Category    | Price |
| --------- | ------------ | ----------- | ----- |
| 1         | Laptop       | Electronics | 1000  |
| 2         | Smartphone   | Electronics | 800   |
| 3         | Headphones   | Electronics | 150   |
| 4         | Coffee Maker | Home        | 200   |
| 5         | Blender      | Home        | 100   |

- Products 테이블:   

| OrderID | ProductID | OrderDate  | Quantity | CustomerID |
| ------- | --------- | ---------- | -------- | ---------- |
| 101     | 1         | 2024-02-01 | 2        | 1          |
| 102     | 3         | 2024-02-02 | 1        | 2          |
| 103     | 2         | 2024-02-03 | 1        | 1          |
| 104     | 4         | 2024-02-04 | 3        | 3          |
| 105     | 1         | 2024-02-05 | 1        | 2          |
| 106     | 5         | 2024-02-06 | 2        | 3          |

- Customers 테이블:

| CustomerID | CustomerName | Country |
| ---------- | ------------ | ------- |
| 1          | Alice        | USA     |
| 2          | Bob          | UK      |
| 3          | Charlie      | USA     |

<br>

- **요구사항:**
    - 고객 이름, 총 구매 금액, 고객이 실제 주문한 주문서(OrderID) 수를 출력하는 SQL 쿼리를 작성해주세요.   
        - a. 기대 결과   
        
        | CustomerName | TotalAmount | OrderCount |
        | ------------ | ----------- | ---------- |
        | Alice        | 2600        | 2          |
        | Bob          | 950         | 2          |
        | Charlie      | 800         | 2          |

    - 각 제품 카테고리별로 가장 많이 팔린 제품의 이름과 총 판매량을 조회하는 SQL 쿼리를 작성해주세요.   
        - a. 기대 결과   

        | Category    | Top_Product  | TotalSold |
        | ----------- | ------------ | --------- |
        | Electronics | Laptop       | 3         |
        | Home        | Coffee Maker | 3         |

<br>

**제약사항:**
- 두 쿼리 모두 서브쿼리, JOIN, GROUP BY, HAVING 등의 SQL 기능을 사용해 문제를 풀어야 합니다.   
- 여러 테이블 간의 관계를 명확히 이해하고, 데이터를 효율적으로 결합하는 것이 중요해요!

<br><br>

- - -
## 문제 풀이   

- 문제1 - 고객 이름, 총 구매 금액, 고객이 실제 주문한 주문서(OrderID) 수를 출력하는 SQL 쿼리를 작성해주세요.   
```sql
SELECT
    c.CustomerName AS CustomerName,
    SUM(p.Price*o.Quantity) AS TotalAmount,
    COUNT(o.OrderID) AS OrderCount
FROM
    Customers c JOIN Orders o ON c.CustomerID=o.CustomerID
                JOIN Products p ON o.ProductID=p.productID
GROUP BY
    c.CustomerID;
```   
   
- 3개 이상의 테이블에 `JOIN`을 사용해야 합니다   

<br>

- 문제2 - 각 제품 카테고리별로 가장 많이 팔린 제품의 이름과 총 판매량을 조회하는 SQL 쿼리를 작성해주세요.   
```sql
SELECT
    p.Category AS Category,
    p.ProductName AS Top_Product,
    SUM(o.Quantity) TotalSold
FROM
    Products p JOIN Orders o ON p.ProductID=o.PrudoctID
GROUP BY
    p.Category,
    p.ProductName
HAVING
    SUM(o.Quantity) = (
        SELECT
            MAX(sumQuantity)
        FROM (
            SELECT
                p2.Category,
                SUM(o2.Quantity) AS sumQuantity
            FROM
                Products p2 JOIN Orders o2 ON p2.ProductID=o2.ProductID
            GROUP BY
                p2.Category,
                p2.ProductName
        ) s
        WHERE
            s.Category=p.Category
    );
```   

- 각 카테고리마다의 합계를 조회한 뒤, 최댓값을 가진 데이터를 추출하고 `HAVING`절로 조건을 붙여 표시합니다   
   
이 문제는 `HAVING`절을 배워왔던 저번 문제들 중에서 유사하게 나왔던 문제가 있었으므로   
공부했던 내용을 기반으로 충분히 풀 수 있을만한 문제라고 생각됩니다   
   
그리고 쿼리문 작성 방식을 이번 글의 답안처럼 작성하는게 눈에 더 잘 들어오는 것 같네요   