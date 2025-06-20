---
title: "[웹개발] Firebase"
date: 2025-06-20 14:30:00 +0900
categories: [Sparta Coding, Web Developement]
tags: [Sparta Coding, Web Developement, Javascript, JQuery, Firebase]
---

## Firebace 시작

> **Firebase**는 구글이 개발한 모바일 및 웹 애플리케이션 개발 플랫폼입니다.   
> 개발자들이 백엔드 인프라를 구축하거나 관리하는 복잡한 작업 없이 핵심 기능에 집중할 수 있도록 도와줍니다.   
{: .prompt-tip }   
   
![img](/assets/img/postimg/postimg021.png)   

지금까지 HTML, CSS, Javascript로 눈에 보이는 화면을 꾸미며 **"프론트엔드"** 작업을 수행해왔습니다   
이제부터는 눈에 보이지 않는 **"백엔드"** 작업을 수행해보도록 하겠습니다   
   
- Firebase의 특징   
  - 웹 서버를 대신 만들어 주는 서비스
  - 서버 개발 없이 제작 가능
  - 백엔드 코드 한 줄 없이도 프론트지식(`HTML`, `CSS`, `JS`)만 알아도 웹 서비스 출시 가능
  - 사용량만 넘어가지 않으면 무료   

서버로 데이터를 전송하는 코드는 **"프론트엔드"**에서 작성하고   
데이터를 받으면 데이터베이스에 저장하는 코드는 **"파이어베이스"**에 작성합니다   
   
파이어베이스([링크](https://firebase.google.com/pricing?hl=ko))에 로그인을 한 뒤 콘솔로 이동, 웹 앱 등록을 하고 시작하겠습니다   

> 파이어스토어(Firestore)는 구글의 클라우드 기반 **NoSQL 데이터베이스**입니다.   

![img](/assets/img/postimg/postimg022.png)   

파이어스토어 빌드 - Firestore Database에서 시작합니다   
- 데이터베이스 만들기
- 위치 : asia-northeast3 (Seoul)
- 규칙 : **프로덕션 모드**에서 시작

데이터베이스가 만들어졌다면 규칙 - `allow read, write: if false;`에서 `false`를 `true`로 바꿔준 뒤 **"게시"**를 눌러줍니다   
`true`로 바꿔주면 데이터베이스에서 무언가를 읽어가거나 쓰는 것을 허락하겠다는 의미입니다   

<br>
- - -

## Firestore Database 사용
   
[저번 글](https://twotechside.github.io/posts/codingclub_web_003/)에서 만들었던 앨범 프로젝트를 VSCode로 열고   
자바스크립트를 작성했던 `<script>` 태그를 `<script type="module">`로 고쳐준 뒤, 다음 코드를 최상단에 넣어줍니다   

```javascript
// Firebase SDK 라이브러리 가져오기
import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
import { getFirestore } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
import { collection, addDoc } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
import { getDocs } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";


// Firebase 구성 정보 설정
const firebaseConfig = {
	본인 설정 내용 채우기 
};


// Firebase 인스턴스 초기화
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
```
- **"본인 설정 내용 채우기"**에는 파이어베이스의 [ 프로젝트 설정 - SDK 설정 및 구성 - 구성 내용 ]을 복사 후 붙여넣기   

> `<script>`의 type에 module이 붙는 순간부터 `onclick`으로 무언가를 불러올 수 없게 됩니다   
> 그렇기에 **JQuery**로 `$().click()`을 떠먹여줘야 합니다   
{: .prompt-tip }   

<br>

### 파이어베이스에 데이터 저장하기   

```html
<script>
  /* << 파이어베이스 코드 */

  // $("#id").click(async function () {
  //   await addDoc(collection(db, "콜렉션이름"), 데이터);
  // });
  $("#postingbtn").click(async function () {
    let image = $("#image").val();
    let title = $("#title").val();
    let content = $("#content").val();
    let date = $("#date").val();

    let doc = {
      image: image,
      title: title,
      content: content,
      date: date,
    };
    await addDoc(collection(db, "albums"), doc);
  });

  /* 나머지 자바스크립트 코드 >> */
</script>

<!-- "기록하기" 버튼의 id값 추가, 이전에 만든 onclick은 제거 -->
<button id="postingbtn" type="button" class="btn btn-dark">기록하기</button>
```   

![img](/assets/img/postimg/postimg023.png)   
   
앨범 프로젝트에 **[ 이미지 주소, 제목, 날짜, 내용 ]**을 입력하고 버튼을 누르면 데이터가 저장됩니다   
그리고 프론트에서 새로고침을 하더라도 이 데이터는 서버에 남아있습니다!   


이제 입력한 값을 파이어스토어 데이터베이스에 저장하면서 페이지가 새로고침이 되고   
데이터베이스에 저장된 데이터를 불러와서 화면에 앨범을 초기화해줘야 합니다   
   
```javascript
alert("저장 완료!");
window.location.reload();
```
`$("#postingbtn").click()` 함수 안에 **"저장 완료!**라는 안내문을 띄우고 페이지를 새로고침하도록 해주겠습니다   
   
<br>

### 파이어베이스의 데이터 불러오기   

> `<script>`의 type에 module이 붙는 순간부터 자바스크립트가 웹 로딩이 끝나고 가장 나중에 호출됩니다  
> 따라서 `$(document).ready(function () { });`을 작성할 필요가 없습니다      
{: .prompt-tip }   
   
```javascript
// Firebase 데이터 불러오기
let docs = await getDocs(collection(db, "albums"));
docs.forEach((doc) => {
  let row = doc.data();

  // 이미지, 제목, 날짜, 내용 값 가져오기
  let image = row["image"];
  let title = row["title"];
  let content = row["content"];
  let date = row["date"];

  let temp_html = `
    <div class="col">
      <div class="card h-100">
        <img
          src="${image}"
          class="card-img-top"
          alt="..."
        />
        <div class="card-body">
          <h5 class="card-title">${title}</h5>
          <p class="card-text">${content}</p>
        </div>
        <div class="card-footer">
          <small class="text-muted">${date}</small>
        </div>
      </div>
    </div>`;

  $("#card").append(temp_html);
});
```
   
이전에 만든 makeCard 함수를 참고하여 작성하면 됩니다   
이제 페이지를 처음 불러오는 순간부터 데이터베이스에 있던 값을 기준으로 앨범이 채워지게 됩니다!   