---
title: "퀘스트 - 달리기반 Lv1. 데이터 속 김서방 찾기"
date: 2025-06-16 14:00:00 +0900
categories: [Sparta Coding, Quest]
tags: [Sparta Coding, MySQL, Quest]
---

## 문제
상황: 여러분들은 스파르타코딩클럽의 분석가로 취직했습니다. DBeaver를 테스트 해볼 겸 “김”씨로 시작하는 이용자들 수를 세어 보기로 했습니다.   
   
- 데이터 설명   
    - user 테이블은 스파르타 코딩클럽에 가입한 유저들의 정보를 날짜별로 기록한 테이블입니다.   
      - user_id: 익명화된 유저들의 아이디(varchar255)   
      - created_at: 아이디 생성 날짜(timestamp)   
      - updated_at: 정보 업데이트 날짜(timestamp)   
      - name: 익명화된 유저들의 이름(varchar255)   
      - email: 이메일(varchar255)   

![img](/assets/img/postimg/postimg003.png)   

- 문제: 다음과 같은 결과테이블을 만들어봅시다.   
    - name_cnt: “김”씨 성을 가지고 있는 교육생의 수   

![img](/assets/img/postimg/postimg006.png)   

<br><br>

- - -
## 문제 풀이   
   
```sql
select count(distinct(user_id)) name_cnt
from user
where substr(name, 1, 1)='김';
```   
   
- `where substr(name, 1, 1)='김'`대신 `where name like '김%'`를 사용해도 될 것으로 보입니다   
- `LIKE`를 사용하는게 더 빠르다는 의견이 많지만 인덱스를 사용할 수 없다는 점도 있으니 상황에 맞게 적절히 사용하면 되겠습니다   