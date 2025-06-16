---
title: "퀘스트 - 달리기반 Lv3. 이용자의 포인트 조회하기"
date: 2025-06-16 15:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
상황: 이번에는 이용자들 별로 획득한 포인트를 학생들에게 이메일로 보내려고 합니다. 이를 위한 자료를 가공해봅시다. 특히 users 테이블에는 있으나 point_users 에는 없는 유저가 있어요. 이 유저들의 경우 point를 0으로 처리합시다.   
   
- 데이터 설명
    - users 테이블은 스파르타 코딩클럽에 가입한 유저들의 정보를 날짜별로 기록한 테이블입니다.   
        - user_id: 익명화된 유저들의 아이디(varchar255)   
        - created_at: 아이디 생성 날짜(timestamp)   
        - name: 익명화된 유저들의 이름(varchar255)   
        - email: 이메일(varchar255)   

        ![img](/assets/img/postimg/postimg007.png)   

    - point_users 테이블은 스파르타코딩클럽 가입 유저들의 포인트에 대한 정보를 기록한 테이블입니다.   
        - point_user_id: point_users 테이블의 행을 구별하기 위한 key(varchar255)   
        - created_at: 아이디 생성 날짜(timestamp)   
        - updated_at: 정보 업데이트 날짜(timestamp)   
        - user_id: 익명화된 유저들의 아이디(varchar255)   
        - point: 보유하고 있는 포인트(int)   

        ![img](/assets/img/postimg/postimg008.png)   

- 문제: 다음과 같은 결과테이블을 만들어봅시다.   
    - user_id: 익명화된 유저들의 아이디   
    - email: 유저들의 이메일   
    - point: 유저가 획득한 포인트   
        - users 테이블에는 있지만 point_users에는 없는 user는 포인트가 없으므로 0 으로 처리   
        - 포인트 기준으로 내림차순 정렬
    
    - 1 ~ 10행   
    ![img](/assets/img/postimg/postimg009.png)   
    - 490 ~ 498행   
    ![img](/assets/img/postimg/postimg010.png)   

<br><br>

- - -
## 문제 풀이   
   
```sql
select u.user_id user_id,
       u.email email,
       COALESCE(p.point, 0) point
from users u left join point_users p on u.user_id=p.user_id
order by p.point desc;
```