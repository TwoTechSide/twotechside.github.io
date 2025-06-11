---
title: "퀘스트 - 걷기반 4) 이제 놀만큼 놀았으니 다시 공부해봅시다!"
date: 2025-06-11 17:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
아래와 같은 sparta_students(학생) 테이블이 있습니다.

| id  | name   | track   | grade | enrollment_year |
| --- | ------ | ------- | ----- | --------------- |
| 1   | 르탄이 | Node.js | A     | 2023            |
| 2   | 배캠이 | Spring  | B     | 2022            |
| 3   | 구구이 | Unity   | C     | 2021            |
| 4   | 이션이 | Node.js | B     | 2022            |

<span>13.</span> `sparta_students`테이블에서 모든 학생의 이름(name)과 트랙(track)을 선택하는 쿼리를 작성해주세요!   
<span>14.</span> `sparta_students`테이블에서 Unity 트랙 소속이 아닌 학생들을 선택하는 쿼리를 작성해주세요!   
<span>15.</span> `sparta_students`테이블에서 입학년도(enrollment_year)가 2021년인 학생과 2023년인 학생을 선택하는 쿼리를 작성해주세요!   
<span>16.</span> `sparta_students`테이블에서 Node.js가 트랙 소속이고 학점이 'A'인 학생의 입학년도를 선택하는 쿼리를 작성해주세요!   

<br><br>

- - -
## 문제 풀이

<span>13.</span> `sparta_students`테이블에서 모든 학생의 이름(name)과 트랙(track)을 선택하는 쿼리를 작성해주세요!   

```sql
select name,
       track,
from sparta_students;
```

<br>

<span>14.</span> `sparta_students`테이블에서 Unity 트랙 소속이 아닌 학생들을 선택하는 쿼리를 작성해주세요!   

```sql
select *
from sparta_students
where track<>'Unity';
```
- `<>`으로 값이 다른 경우의 필터링을 할 수 있습니다   

<br>

<span>15.</span> `sparta_students`테이블에서 입학년도(enrollment_year)가 2021년인 학생과 2023년인 학생을 선택하는 쿼리를 작성해주세요!   

```sql
select *
from sparta_students
where enrollment_year in (2021, 2023);
```
- `IN`키워드를 사용하여 값이 하나라도 포함되면 표시 할 수 있습니다

<br>

<span>16.</span> `sparta_students`테이블에서 Node.js가 트랙 소속이고 학점이 'A'인 학생의 입학년도를 선택하는 쿼리를 작성해주세요!   

```sql
select *
from sparta_students
where track='Node.js' and grade='A';
```