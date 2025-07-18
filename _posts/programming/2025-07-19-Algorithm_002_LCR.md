---
title: "[Algorithm/Java] 최장 공통 부분 수열 (LCS: Longest Common Subsequence)"
date: 2025-07-19 05:40:00 +0900
categories: [Programming, Algorithm]
tags: [TIL, Java, Algorithm, LCR]
---

## 1. LCS: Longest Common Subsequence   

> 수열이 아닌 문자열을 써서 **"Long Common Substring"**이라고도 하지만 내용은 거의 같습니다   
{:.prompt-info}

LCR 알고리즘 문제가 생각보다 조금 있는 편이더라구요   
이 내용은 이전의 [다이나믹 프로그래밍(DP: Dynamic Programming)](https://twotechside.github.io/posts/TIL_003/)을 알고 있어야 이해하기가 훨씬 수월합니다   
   
사실 이미 DP의 포스트 내용에서 풀었던 백준 - 11053 문제도 LCR 알고리즘으로 풀긴 하지만   
더 근본적인 LCR 문제에 초점을 맞춰서 알아보도록 합시다   

<br>
- - -
    
## 2. LCS 문제 (백준 9251 - LCS)

> [백준 9251 - LCS](https://www.acmicpc.net/problem/9251) 문제는 제목 자체가 **'LCS'**입니다   

- 문제
    - LCS(Longest Common Subsequence, 최장 공통 부분 수열)문제는 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제이다.   
    - 예를 들어, ACAYKP와 CAPCAK의 LCS는 ACAK가 된다.   
- 입력
    - 첫째 줄과 둘째 줄에 두 문자열이 주어진다. 문자열은 알파벳 대문자로만 이루어져 있으며, 최대 1000글자로 이루어져 있다.   

<br>

예시에서 보면 'ACAYKP'와 'CAPCAK'의 결과는 **'ACAK'**가 나와야 합니다   
그러면 문제에 접근하며 풀어보도록 합시다   

<br>

### 2-1. 문제 풀이

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

char[] c1 = br.readLine().toCharArray();
char[] c2 = br.readLine().toCharArray();
```
일단 글자마다 확인을 해야하는 상황이니 `char[]`형태로 받으면 됩니다   
그리고 c1와 c2의 값을 하나하나 일치하는지 확인해야 하기 때문에 이중 for문을 사용해야 되는 것 까지는 알겠습니다   
   
하지만 여기서부터 어떻게 접근해야 하는지 감이 전혀 오지 않는데... 표를 그려서 확인해봅시다   

<br>

   
![img](/assets/img/postimg/postimg043.png)   

> 이중 for문으로 실행시킨다고 가정하면 왼쪽 위에서부터 오른쪽 아래 방향으로 진행됩니다   
> 오른쪽 방향으로 i, 아래쪽 방향으로 j 라고 하면서 보면 되겠습니다   
   
예시에서 가장 긴 수열은 `{A, C, A, K}`니까 답은 4였죠   
위 표에서 **<u>오른쪽 아래인 대각선 방향</u>으로 가장 많이 거친 횟수**를 표시할 수 있어야 합니다   
   
그래서 누적합을 쓰기 위해서는 `'[i, j] = [i-1, j-1] + 1'`을 사용할 수 있어야 합니다   
그러면 만약 2칸 이상 떨어졌던 값은 어떻게 붙이면 될까요
   
<br>

![img](/assets/img/postimg/postimg044.png)   

- 다음 순서로 넘어갔을 때
    - 두 글자가 일치하지 않는다면 -> [i-1, j] 또는 [i, j-1]의 값 중에서 최댓값을 대입
    - 두 글자가 일치하면 -> [i-1][j-1] + 1을 대입

떨어져 있으면 왼쪽 윗 방향의 값을 끌어오면 됩니다   
   
표를 보면 [0, 1], [1, 3]에서 각각 'A'와 'C'가 겹치는데   
[0, 1]의 값인 1을 [0, 2]로 끌고 왔기 때문에 [1, 3]은 2가 되는 것입니다   
   
이 과정을 끝까지 하면 표의 가장 끝 값은 공통 수열의 최대 길이가 저장됩니다   
단, 첫 줄에서 [i-1][j-1]을 불러오면 배열의 범위를 벗어나기 때문에 0으로 초기화된 새로운 줄을 추가해줘야 합니다   
   
<br>

```java
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        char[] c1 = br.readLine().toCharArray();
        char[] c2 = br.readLine().toCharArray();

        // 첫 줄에 0으로 초기화 해주기 위해 배열의 길이 1 추가
        int [][] dp = new int[c1.length+1][c2.length+1];

        for (int i = 1; i <= c1.length; i++) {
            for (int j = 1; j <= c2.length; j++) {

                if (c1[i-1] != c2[j-1])
                    // 문자가 일치하지 않으면 [i-1, j] 또는 [i, j-1]중 더 큰 값을 대입
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);

                else
                    // 문자가 일치하면 [i-1][j-1] + 1 대입
                    dp[i][j] = dp[i-1][j-1] + 1;
            }
        }

        // 마지막 가장 큰 합계가 저장된 dp 배열의 끝 값을 출력
        bw.write(dp[c1.length][c2.length] + "\n");
        bw.flush();

        br.close();
        bw.close();
    }
}
```

<br>
- - -

## 3. 만약 연속된 문자의 개수를 세어야 한다면

<br>

```java
int maxNum = 0;

// ... 이중 for문 진행
if (c1[i-1] == c2[j-1]) {
    dp[i][j] = dp[i-1][j-1] + 1;
    maxNum = Math.max(maxNum, dp[i][j]);
}
```

LCS에 대해 이해가 되었다면 전혀 어렵게 생각할 필요가 없죠   
이전 값을 끌어오는 것만 안하면 됩니다   
   
하지만 dp의 끝에 꼭 최댓값이 저장되지는 않기 때문에, 최댓값이 갱신되면 새로 저장을 해주어야 합니다   