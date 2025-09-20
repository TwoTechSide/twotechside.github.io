---
title: "[Algorithm/Java] 우선순위 큐 (Priority Queue)"
date: 2025-09-20 20:00:00 +0900
categories: [Programming, Algorithm]
tags: [TIL, Java, Algorithm, Priority Queue]
---

## 1. 우선순위 큐 (Priority Queue)   
  
**큐(Queue)**라는 자료구조는 대충 FIFO (First In First Out) 성질을 가진 구조였습니다  
우선순위 큐는 FIFO 대신 **우선순위**를 기준으로 사용되는 **큐**라고 보면 되겠습니다  
  
그리고 **힙(Heap)**이라는 **완전 이진 트리(Complete Binary Tree)** 구조인 것이 특징입니다  
`이진 트리(Binary Tree)`가 익숙하지 않다면 한 번 알아보고 오셔도 괜찮을 것 같구요  
  
<br>
- - -
  
## 2. 데이터 저장 및 삭제는 어떻게?  
  
개념적인 부분입니다, 앞서 말한대로 `이진 트리`에 익숙하지 않다면 잠시 넘어갔다가 나중에 이해하셔도 될 것 같구요  
  
![img](/assets/img/postimg2/postimg052.png)  

- **삽입**  
    1. 인덱스 순으로 마지막 위치에 새로운 노드를 추가합니다  
    2. 부모 노드와 비교하며 더 큰 값과 계속 교환합니다  

    - 위 이미지에서 14를 넣는다고 가정한다면 다음 순서처럼 작동될 것입니다  
        1. 14 <-> 6 교환
        2. 14 <-> 11 교환
  
![img](/assets/img/postimg2/postimg053.png)  

- **삭제**
    1. 루트 노드를 삭제하면서 마지막 위치의 노드를 가져옵니다  
    2. 삽입과 반대로 자식 노드와 비교하며 더 큰 값과 계속 교환합니다  
  
    - 위 이미지에서 15를 삭제한다고 하면 다음 순서처럼 작동될 것입니다  
        1. 15 삭제
        2. 마지막 노드인 3 노드를 루트 노드로 이동
        3. 11 <-> 3 교환
        4. 6 <-> 3 교환
  
시간 복잡도는 삽입과 삭제 둘 다 `O(logn)`이 됩니다  
  
<br>
- - -

## 3. 코드로 사용해보기  
  
```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {

        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.add(1);
        pq.add(10);
        pq.add(20);
        pq.add(5);

        System.out.println(pq.poll());  // 1
        System.out.println(pq.poll());  // 5
    }
}
```
  
`Java`에서 기본적으로 `PriorityQueue`를 지원하고 있으니 그대로 사용하면 됩니다  
그런데 위의 설명과 다르게 여기서는 오름차순으로, 작은 값부터 꺼내는 것을 알아두면 될 것 같습니다  
  
<br>

하지만 직접 정의한 람다식 또는 `Comparator`를 PriorityQueue의 생성자에 넣어 더 다양하게 사용하는 것도 가능합니다  
절댓값 기준 내림차순 정렬, 2차원 배열의 0번 인덱스 기준 오름차순 정렬 등...  
  
어떤 구조에 값을 계속해서 넣거나 빼야하면서도 순서는 유지가 필요한 경우마다 사용하면 좋을 것 같습니다  
아래 예시는 "절댓값 내림차순" 정렬입니다  

```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {

        PriorityQueue<Integer> pq = new PriorityQueue<>((o1, o2) -> Math.abs(o2) - Math.abs(o1));

        pq.add(5);
        pq.add(-19);
        pq.add(10);
        pq.add(-3);

        System.out.println(pq.poll());  // -19
        System.out.println(pq.poll());  //  10
    }
}
```