---
title: "[MySQL] SQL 기초 문법 정리3"
date: 2025-06-10 16:00:00 +0900
categories: [Sparta Coding, MySQL]
tags: [Sparta Coding, MySQL]
---

이전 글에서 다음 내용들을 배웠습니다   
- `SUM`, `AVG`, `COUNT`, `MIN`, `MAX`
- `GROUP BY`, `ORDER BY`
   
그런데 `GROUP BY`에 칼럼명 대신 숫자를 쓰는 방법도 있습니다   
`group by 1, 2`라고 쓰면 첫 번째 칼럼, 두 번째 칼럼을 범주로 묶을 수 있으며, `ORDER BY`도 가능합니다   
   
그럼 이제 저번과 이어서 3번째 기본 문법을 더 배워봅시다   

<br>
- - -

## REPLACE, SUBSTRING, CONCAT / SQL로 문자열 가공하기
```sql
select restaurant_name "원래 상점명",
       replace(restaurant_name, 'Blue', 'Pink') "바뀐 상점명"
from food_orders
where restaurant_name like '%Blue Ribbon%';
-- 원래 상점명 "Blue Ribbon Sushi Izayaza" / 바뀐 상점명 "Pink Ribbon Sushi Izayaka"
```
* `replace(칼럼, A, B)` : 칼럼에 있는 데이터의 A를 B로 치환   
`food_orders` 테이블의 `restaurant_name`에 `Blue Ribbon`이 포함된 데이터에서 `Blue`를 `Pink`로 교체한 새로운 칼럼 `바뀐 상점명`이 추가된 것을 볼 수 있습니다

<br>
   
```sql
select addr "원래 주소",
       substr(addr, 1, 2) "시도"
from food_orders
where addr like '%서울특별시%';
-- 원래 주소 "서울특별시 종로구 팔판동" / 시도 "서울"
```
* `substr(칼럼, n1, n2)` : 칼럼에 있는 데이터의 n1번째부터 n2번째까지 문자열을 추출
즉, 예시(서울특별시 종로구 팔판동)에서는 1번째부터 2번째까지인 `서울`이 새로운 칼럼 `시도`에 표시됩니다   
* 만약 `n2`를 제외하고 `n1`만 쓸 경우, n1번째부터 끝까지 문자열을 잘라냅니다
* 또한, `substring(칼럼, n1, n2)`로도 사용할 수 있습니다

<br>
   
```sql
select restaurant_name "원래 이름",   
       addr "원래 주소",
       concat('[', substring(addr, 1, 2), '] ', restaurant_name) "바뀐 이름"
from food_orders
where addr like '%서울%';
-- 원래 이름 "Hangawi"
-- 원래 주소 "서울특별시 종로구 팔판동"
-- 바뀐 이름 "[서울] Hangawi"
```
* `concat(s1, s2, s3, ...)` : s1, s2, s3, ...가 붙은 문자열 표시   
앞서 배운 `substring`을 활용한 예시에서는 `[`, `서울`, `]`, `Hangawi`가 붙어 `[서울] Hangawi`가 표시됩니다   

<br>
   
## IF, CASE / 조건에 따라 포맷 변경하기   
앞의 예시에서 `substr`로 문자열을 잘라 썼지만 `경기도`처럼 3글자인 경우도 있습니다   
이런 경우를 대비해 조건에 맞게 자르도록 해봅시다   
   
```sql
select restaurant_name,
       cuisine_type "원래 음식 타입",
       if(cuisine_type='Korean', '한식', '기타') "음식 타입"
       -- 만약 cuisine_type = 'Korean'일 경우 한식, 아니면 기타로 표시
from food_orders;
```
* `if (조건, 참, 거짓)` : 조건에 따라 참, 거짓 값으로 표시됩니다   
   
조금 더 심화해서 다음처럼 복잡한 형태로도 사용할 수 있습니다   
`select substring(if(email like '%gmail%', replace(email, 'gmail', '@gmail'), email), 10) "이메일 도메인"`
- "gmail"이 포함된 경우에만 "@gmail"로 치환, 아니면 유지
- if문이 끝나면 `substring(substr)`로 앞의 10번째 문자부터 표시
- 칼럼명은 "이메일 도메인"으로 표시

<br>
   
```sql
select case when cuisine_type='Korean' then '한식'
            when cuisine_type in ('Japanese', 'Chinese') then '아시아'
            else '기타' end
            as "음식 타입",
        cuisine_type
from food_orders;
```
case문은 처음 `case`를 작성한 뒤, 조건마다 `when 조건 then 값`을 붙여줍니다   
그리고 모든 조건에 맞지 않는 경우는 `else 값`을 작성하고 `end`로 case문을 종료합니다   
   
만약 `when`에 들어가는 조건으로도 충분하다면 `else 값`은 생략해도 됩니다   

<br>
- - -

## 실습 문제
1. 지역과 배달시간을 기반으로 배달수수료 구하기 (식당 이름, 주문 번호 함께 출력)   
   (지역 : 서울, 기타 - 서울일 때는 수수료 계산 * 1.1, 기타일 때는 곱하는 값 없음   
   시간 : 25분, 30분 - 25분 초과하면 음식 가격의 5%, 30분 초과하면 음식 가격의 10%)   
   
2. 주문 시기와 음식 수를 기반으로 배달할증료 구하기   
   (주문 시기 : 평일 기본료 = 3000 / 주말 기본료 = 3500   
   음식 수 : 3개 이하이면 할증 없음 / 3개 초과이면 기본료 * 1.2)

<br><br>

### 문제 풀이
```sql
-- 1번 문제
select case when delivery_time>30 then price*0.1*if(addr like '서울%', 1.1, 1)
            when delivery_time>25 then price*0.05
            else 0 end "수수료",
        restaurant_name,
        order_id,
        price,
        delivery_time,
        addr
from food_orders;

-- 2번 문제
select case when day_of_the_week='Weekday' then 3000*if(quantity>3, 1.2, 1)
            when day_of_the_week='Weekend' then 3500*if(quantity>3, 1.2, 1)
            end "배달항즐료",
       restaurant_name,
       order_id,
       day_of_the_week,
       quantity
from food_orders;
```