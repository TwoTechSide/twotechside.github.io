---
title: "[백준] JAVA 입출력 시간초과 발생시 주의사항"
date: 2025-07-10 15:00:00 +0900
categories: [Programming, Info]
tags: [Programming, Backjoon, Java, Coding-Test]
---

백준에서 분명 코드는 정상적으로 작동하는 것 같고 시간 복잡도를 최저한으로 작성한 것 같은데   
이상하게 실행시켜보면 시간 초과가 뜨는 경우가 있습니다   
   
이럴 때에는 Java의 `Scanner`과 `System.out.println`를 의심해봐야 합니다   

<br>
- - -

## 1. BufferedReader   

- Scanner
    - 버퍼를 사용하지 않는 입력
    - 사용자가 입력을 할 때마다 전송을 계속 해줘야 하기 때문에 속도가 느립니다   

- BufferedReader
    - **'버퍼(Buffer)'**를 사용하는 입력
    - 데이터를 **'버퍼'**에 저장해두었다가 일정 조건(개행 등)을 만족하면 한 번에 전송하기에 빠릅니다   

> 버퍼(Buffer) : 데이터를 일시적으로 보관해두는 메모리 영역   

마치 **[쓰레기가 생길 때마다 버리러 가는 것]**과 **[쓰레기통에 모았다가 한 번에 버리는 것]**의 차이라고 볼 수 있겠습니다   
   
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class NewClass {
    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String input = br.readLine(); // <- 값 입력
        // Integer.parseInt(br.readLine())을 작성하면 정수형 타입으로 입력받음
    }
}
```

> 처음 BufferedReader 또는 아래의 BufferedWriter을 사용할 경우 예외 처리는 필수입니다   
> 만약 `main`에서 작성중이라면 다음처럼 수정해줘야 합니다   
> - `public static void main(String[] args) throws IOException`   
{:.prompt-tip}

<br>

## 2. BufferedWriter

[ Scanner - BufferReader ]과 비슷한 차이를 보여주는데, 이번에는 출력입니다   

```java
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;

public class NewClass {
  public static void main(String[] args) throws IOException {

    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    bw.write("Hello!\n");   // 버퍼에 값 저장
    bw.flush();             // 버퍼에 저장된 값을 출력
  }
}
```

<br>

## 3. StringTokenizer

BufferedReader로 한 줄을 읽어와야 하는데, 만약 한 줄 안에 띄어쓰기로 구분된 값이 여러 개가 있다면   
StringTokenizer을 사용해서 값을 하나씩 사용하시면 됩니다   

```java
import java.io.*;
import java.util.StringTokenizer;

public class TempClass {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st;

        st = new StringTokenizer(br.readLine());

        int n1 = Integer.parseInt(st.nextToken());  // nextToken()으로 하나씩 불러오기
        int n2 = Integer.parseInt(st.nextToken());
        int result = n1 + n2;

        bw.write("n1 + n2 = " + result +"\n");  // "n1 + n2 = 30"을 출력
        bw.flush();
    }
}
```

