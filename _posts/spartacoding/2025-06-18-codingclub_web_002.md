---
title: "[웹개발] Bootstrap, Javascript"
date: 2025-06-18 14:00:00 +0900
categories: [Sparta Coding, Web Developement]
tags: [Sparta Coding, Web Developement]
---

## 부트스트랩 활용  

**"스파르타플릭스"** 라는 새로운 폴더를 만들어서 써보도록 하겠습니다   
   
> Jumbotron [(https://getbootstrap.com/docs/5.3/examples/jumbotron/)](https://getbootstrap.com/docs/5.3/examples/jumbotron/)   

![img](/assets/img/postimg/postimg015.png)   
   
`F12`또는 화면 우클릭 후 "검사"를 누르면 개발자 도구가 열립니다   

개발자 도구의 좌측 상단 버튼으로 마우스 위치에 따라 HTML을 확인할 수 있습니다   
위 이미지처럼 "Custom jumbotron"이 있는 화면의 `<div>`태그에 **우클릭 - Edit as HTML**을 눌러 내용을 복사합니다   
   
![img](/assets/img/postimg/postimg016.png)   

그리고 `<body>`태그 안에 작성하면 내용을 가져와 사용할 수 있게 됩니다   
   
<br>

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>스파르타플릭스</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <style>
      @import url('https://fonts.googleapis.com/css2?family=Gowun+Dodum&display=swap');

      * {
        font-family: 'Gowun Dodum', sans-serif;
      }
      .main {
        color: white;

        background: url('https://occ-0-1123-1217.1.nflxso.net/dnm/api/v6/6AYY37jfdO6hpXcMjf9Yu5cnmO0/AAAABeIfo7VL_VDyKnljV66IkR-4XLb6xpZqhpLSo3JUtbivnEW4s60PD27muH1mdaANM_8rGpgbm6L2oDgA_iELHZLZ2IQjG5lvp5d2.jpg?r=e6e.jpg');
        background-position: center;
        background-size: cover;
      }
    </style>
</head>
<body>
  <header class="p-3 text-bg-dark">
    <div class="container">
      <div class="d-flex flex-wrap align-items-center justify-content-center justify-content-lg-start">
        <a href="/" class="d-flex align-items-center mb-2 mb-lg-0 text-white text-decoration-none">
          <svg class="bi me-2" width="40" height="32" role="img" aria-label="Bootstrap"><use xlink:href="#bootstrap"></use></svg>
        </a>
        <ul class="nav col-12 col-lg-auto me-lg-auto mb-2 justify-content-center mb-md-0">
          <li><a href="#" class="nav-link px-2 text-danger">spartaflix</a></li>
          <li><a href="#" class="nav-link px-2 text-white">홈</a></li>
          <li><a href="#" class="nav-link px-2 text-white">시리즈</a></li>
          <li><a href="#" class="nav-link px-2 text-white">영화</a></li>
          <li><a href="#" class="nav-link px-2 text-white">내가 찜한 콘텐츠</a></li>
        </ul>
        <form class="col-12 col-lg-auto mb-3 mb-lg-0 me-lg-3" role="search">
          <input type="search" class="form-control form-control-dark text-bg-dark" placeholder="Search..." aria-label="Search">
        </form> <div class="text-end">
          <button type="button" class="btn btn-outline-light me-2">Login</button>
          <button type="button" class="btn btn-danger">Sign-up</button>
        </div>
      </div>
    </div>
  </header>
  <div class="main">
    <div class="p-5 mb-4 bg-body-tertiary rounded-3">
        <div class="container-fluid py-5">
            <h1 class="display-5 fw-bold">킹덤</h1>
            <p class="col-md-8 fs-4">병든 왕을 둘러싸고 흉흉한 소문이 떠돈다. 어둠에 뒤덮인 조선, 기이한 역병에 신음하는 산하. 정체 모를 악에 맞서 백성을 구원할 희망은 오직 세자뿐이다.</p>
            <button class="btn btn-primary btn-lg" type="button">영화 기록하기</button>
            <button class="btn btn-primary btn-lg" type="button">상세정보</button>
        </div>
    </div>
  <div>
</body>
</html>
```

영화 킹덤을 소개하는 배너로 수정해서 바꿔서 사용해보았습니다   
여기서 `button`클래스의 이미지도 바꿔주면 좋을 것 같습니다   
   
이전 시간처럼 부트스트랩에 접속한 뒤, **"Outline Buttons - Light"**로 수정해서 사용하겠습니다   
헤더는 **부트스트랩 - Examples - Snippets/Headers - `<header class="p-3 text-bg-dark">`**를 사용했습니다   
   
![img](/assets/img/postimg/postimg017.png)   
   
이 외에도 카드 박스를 넣거나, 다른 HTML 태그를 넣고 새로운 박스를 만드는 등   
여러가지를 활용하여 화면을 만들어보면 되겠습니다   

<br><br>
- - -

## Javascript

- 자바스크립트란   

> Java와 Javascript의 차이 : 햄과 햄스터...처럼 관련이 없습니다   

자바스크립트는 **HTML + CSS + Javascript**로 묶이며, 웹 브라우저에서 실행되는 프로그래밍 언어 중 하나입니다   
   
웹 페이지의 동적인 기능을 구현하기 위해 자바스크립트가 개발되었으며   
브라우저한테 명령을 내리는 '표준'이라고 생각하면 됩니다   
   
<br>

### Javascript 기초문법   

> 자바스크립트는 HTML에서 CSS를 사용하던 것 처럼 `<script>`태그에 작성하여 사용할 수 있습니다   

```html
<script>
  let a = 'hello';
  console.log(a);
</script>
```

- 변수 : 값을 담는 변수(변하는 숫자)   

위 예시처럼, `let a = 'hello';`를 작성하면서 `a`라는 변수에 **"hello"**가 들어가게 됩니다   
이것을 확인하기 위해 스크립트를 작성한 뒤, **브라우저 - 개발자 도구 - Console**을 누르면 결과를 확인할 수 있습니다   
   
```javascript
let a = 5;
let b = 3;
console.log(a+b); // 8
```

> 변수명은 예시처럼 a, b로 사용할 수도 있지만...   

나중에 유지보수를 하거나, 협력중인 다른 개발자가 보았을 때 어떻게 쓰이는 변수인지 알아보기 어렵습니다   
따라서 변수명은 name, age, total 등으로 알아볼 수 있도록 사용하는 것을 권장합니다   
   
<br>

### 자료형

자바스크립트에서 변수 1개에 여러가지 값을 넣을 수 있는 **리스트**, **딕셔너리**도 사용됩니다   
   
```javascript
let a = ['사과', '배', '수박'];  //리스트 예시
console.log(a[1]);              // '배' 출력

let b = {'name':'bob', 'age':30, 'height':180}; //딕셔너리 예시
console.log(person['name']);                    //'bob' 출력
```

> 첫 번째 예시에서 **배**가 출력되는 이유는 자바스크립트의 순서는 0부터 시작되기 때문입니다   
> 리스트는 대괄호`[, ]`로 감싸고, 중괄호는 `{, }`로 감싸집니다   

보통 리스트는 주제와 공통되는 값을 넣는 식으로 사용되고 (과일 종류: 사과, 배, 수박, ...)   
딕셔너리는 주제에 담긴 정보값을 넣는 식으로 사용됩니다 (사람 A: 키-175, 몸무게-60kg, 나이-20, ...)
   
<br>

### 반복문, 조건문   

```javascript
//반복문
let fruits = ['사과', '배', '감', '귤'];
fruits.foreach((a) => {   //리스트에 있는 값을 각각 a에 할당하면서
  console.log(a);         //console.log로 각 값을 출력
})
```   

앞서 배운 리스트에 있는 값을 하나씩 출력하려면 `console.log`를 계속 써야할 수도 있습니다   
하지만 반복문을 사용한다면 리스트에 값이 100개가 넘더라도 짧은 내용으로 출력할 수 있습니다   

<br>
   
```javascript
//조건문
let age = 24;

if (age > 20) {
  console.log('성인');
} else {
  console.log('청소년');
}

//반복문과 함께 사용하기
let ages = [12, 15, 18, 20, 25, 37, 12, 48];

ages.forEach((a) => {
  if (a > 20) {
    console.log('성인');
  } else {
    console.log('청소년');
  }
});
```

조건문은 `if ~ else ~`로 사용되며, 조건이 **참**이거나 **거짓**임에 따라 실행이 다르게 됩니다   

<br>

### Javascript 활용문법(DOM)

자바스크립트로 알림창을 띄우고 HTML을 조작할 수도 있습니다   
   
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script>
      function hey() {
        // 알림창 띄우기 : alert
        alert("안녕하세요!");

        // HTML 변경 - getElementById
        document.getElementById("hello").style.color = "red";
      }
    </script>
  </head>
  <body>
    <div id="hello">반갑습니다</div>
    <button class="form-button" type="button" onclick="hey()">
      스타일 바꾸기
    </button>
  </body>
</html>
```

만약 `hey`라는 함수를 작동시키는 버튼을 누를 경우   
"안녕하세요!" 알림창이 뜬 다음 `<div id="hello">`의 "반갑습니다" 색상이 빨간색으로 바뀌게 됩니다   

<br>

### JQuery

순수 자바스크립트(=바닐라 자바스크립트)만으로도 모든 기능을 구현할 수는 있지만   
코드가 매우 복잡해지고 브라우저 간 호환성 문제도 고려해야하기 때문에 쉬운 방법은 아닙니다   
   
그렇기 때문에 JQuery라는 라이브러리가 등장하게 되었으니 다음부터는 JQuery를 사용해보겠습니다   
   
```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
```
JQuery를 적용하는 방법은 `<head>`와 `</head>`사이에 위 코드를 넣어주면 됩니다   