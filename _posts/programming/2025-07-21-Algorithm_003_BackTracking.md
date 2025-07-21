---
title: "[Algorithm/Java] 백트래킹 (BackTracking)"
date: 2025-07-21 11:20:00 +0900
categories: [Programming, Algorithm]
tags: [TIL, Java, Algorithm, BackTracking]
---

## 1. 백트래킹 (BackTracking)   

해를 구하던 도중에 만약 해가 나올 것 같지 않은 경우가 되면 다시 이전 단계로 돌아가서 해를 찾는 기법입니다   
이 알고리즘은 N과 M이라는 문제로 이해하는게 훨씬 빠를 것 같으니 바로 문제로 넘어가봅시다   


<br>
- - -
    
## 2. N과 M 문제 (백준 9251)

> [백준 15649 - N과 M(1)](https://www.acmicpc.net/problem/15649)   

- 문제
    - 자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.   
    - 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열   
- 입력
    - 첫째 줄에 자연수 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)   

<br>

예를 들어, 숫자가 4까지 있고 여기서 중복 없이 4개를 고른 수열을 표현하고 싶으면   
(1, 2, 3, 4), (1, 2, 4, 3), (1, 3, 2, 4), ..., (4, 3, 2, 1) 이 출력될겁니다   
   
각 자리마다 올 수 있는 경우의 수가 4가지가 있으니 모든 경우의 수는 4^4 = 256가지나 됩니다   
숫자와 길이가 더 커질 수록 경우의 수가 끔찍할 정도로 커질 것 같습니다   

<br>

### 2-1. 백트래킹으로 접근

만약 (a, b, c, d)라는 수열을 a -> b -> c -> d 순으로 접근할 때, 앞에서 사용했던 값을 확인할 수 있으면 더 좋지 않을까요   
(1, ?, ?, ?)가 있으면 **'1'**을 이미 사용했으니 두 번째 '?'에는 1이 오는 경우는 치워버리는 것입니다   

<br>

```java
static int N;
static boolean[] checked;   // main에서 new int[N+1]으로 초기화 필요

public static void BackTracking() {
    for (int i = 1; i <= N; i++) {
        if (!checked[i]) {
            checked[i] = true;
        }
    }
}
```
`BackTracking`함수를 통해 `checked`의 1, 2, 3, 4가 하나씩 체크될 것입니다   
만약 1이 체크되었을 때에는 (1, ?, ?, ?) 상태가 될테니 두 번째 값으로 넘어가고 또 세 번째 값으로 넘어가야겠죠   
   
여기서는 재귀함수를 사용한다면 될 것 같습니다   

<br>

```java
static int N, M;
static int[] arr;
static boolean[] checked;

public static void BackTracking(int depth) {
    for (int i = 1; i <= N; i++) {
        if (!checked[i]) {
            checked[i] = true;
            arr[depth] = i;
            BackTracking(depth + 1);
            checked[i] = false;
        }
    }
}

// main -> BackTracking(0);
```
- (1, 2, 3, 4) : 체크된 숫자는 `arr`에 기록하며 `checked`에 `true` 대입
- (1, 2, 4, 3) : 3번째 숫자에서 1, 2는 체크되었으므로 패스하고 3의 경우 for문이 끝났으니 4 진행
    - 이때, '3'에서 순회가 끝났으므로 체크를 해제하며 4로 넘어감

재귀함수를 호출하며 백트레킹이 되면 체크한 숫자는 다시 풀어주는 것을 핵심으로 이해하면 좋을 것 같습니다   
`(depth = 4)`에 도달하면 재귀함수를 더 호출할 필요가 없으니 `arr`에 기록한 수열을 출력하고 `return`하면 됩니다   

<br>

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static int N, M;
    static int[] arr;
    static boolean[] checked;
    static BufferedReader br;
    static BufferedWriter bw;

    public static void backTracking(int depth) throws IOException {
        if (depth == M) {   // depth = M인 경우(수열 4개를 다 조회하면), 길이가 M인 수열 출력
            for (int i = 0; i < M; i++)
                bw.write(arr[i] + " ");

            bw.newLine();
            return;
        }

        for (int i = 1; i <= N; i++) {
            if (!checked[i]) {
                checked[i] = true;
                arr[depth] = i;
                backTracking(depth + 1);    // 사용하지 않은 숫자는 checked = true로 바꿔주며 arr에 담아두고 재귀함수 진행
                checked[i] = false;         // 사용이 끝났다면 false로 전환
            }
        }
    }
    
    public static void main(String[] args) throws IOException {
        br = new BufferedReader(new InputStreamReader(System.in));
        bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        arr = new int[M];
        checked = new boolean[N+1];

        backTracking(0);

        br.close();
        bw.close();
    }
}
```

저는 입출력을 `BufferedReader`, `BufferedWriter`을 사용했는데 다른 방법을 사용하셔도 됩니다