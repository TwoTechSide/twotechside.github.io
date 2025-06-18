---
title: "[웹개발] HTML, CSS, Bootstrap"
date: 2025-06-17 16:00:00 +0900
categories: [Sparta Coding, Web Developement]
tags: [Sparta Coding, Web Developement]
---

## 1~5주차 배울 순서
- 1주차 : HTML, CSS, Bootstrap   
- 2주차 : Bootstrap, Javascript
- 3주차 : JQuery, fetch   
- 4주차 : firebase project 1   
- 5주차 : firebase proejct 2, Github, Python   

<br>
- - -

## HTML, CSS 기초   

> 웹의 동작 개념은 **[1. 요청을 보내고, 2. 받은 HTML 파일을 그려주는 것]**이라고 할 수 있습니다   
> 브라우저에 접속하면서 브라우저는 `요청을 보내고`, 서버에서 준비해둔 것을 **"받아서"** 브라우저에 **그려주는** 것이죠   
   
> **VS Code (Visual Sutdio Code)**라는 마이크로소프트 사에서 개발한 **코드 에디터**로 진행합니다   
{: .prompt-tip }

<br>

![img](/assets/img/postimg/postimg011.png)   

- **VS Code*를 설치한 뒤, 다음 확장 프로그램들을 설치합니다   
    - **Korean Language Pack for Visual Studio Code**
    - **Live Server**
- 새로운 `html`파일을 만든 뒤, "html:5"를 입력하고 **탭**키를 눌러 자동 완성으로 기본 구성을 만듭니다   
- VS Code의 html 화면에 우클릭을 하여 `Open with Live Server`로 실행시킵니다   
   
이제부터 VS Code로 편집을 하면 실시간으로 반영되는 화면을 보면서 웹개발을 배워보도록 합시다   

<br>

### HTML / 로그인 페이지 만들기   
   
`login.html`파일을 새로 하나 만들어서 실행시켜보겠습니다   
   
![img](/assets/img/postimg/postimg012.png)   
   
> `<p>`, `<h1>` 등의 태그는 외울 필요가 없으며, 필요한 상황에 따라 검색하며 사용하면 됩니다   
   
이렇게 로그인 페이지 구성을 만들면 다음은 꾸밀 준비도 해야합니다   
꾸미는 방법은 후술할 `CSS`를 통해 만들 수 있습니다   

<br>

### CSS / 화면 꾸미기   
  
```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>제목을 입력</title>
  <style>
    .mytitle {
    color: red;
  }
  </style>
</head>
<body>
  <h1 class="mytitle">로그인 페이지</h1>
  <p>ID: <input type="text"/></p>
  <p>PW: <input type="text"/></p>
  <button>로그인하기</button>
</body>
```

- `<h1>`태그에 **mytitle**이라는 명찰을 붙여줍니다   
- `<style`>에서 해당 명찰이 달린 태그의 색상을 <span style="color: red;">빨간색</span>으로 바꿉니다   
  
> html에 태그가 다양하게 있듯이, 스타일을 적용하는 css도 다양하게 있습니다   
> 이것도 마찬가지로 필요한 경우 검색을 통해 쉽게 사용할 수 있습니다   

<br>
- - -

## 구글 폰트 가져다 쓰기
   
기본 폰트로만 꾸미는건 조금 아쉬운 감이 없지않아 있습니다   
그러나 무료로 사용할 수 있는 구글 폰트를 가져다 사용한다면 조금 더 멋지게 꾸밀 수 있지 않을까요   
   
[구글 폰트 사이트(https://fonts.google.com/?subset=korean)](https://fonts.google.com/?subset=korean)에 들어가 원하는 폰트를 하나 골라줍니다   
Language 필터에 **Korean**을 적용하면 한국어로 나타나는 폰트들을 확인할 수 있습니다   
   
![img](/assets/img/postimg/postimg013.png)   
   
- 원하는 폰트를 선택한 뒤, "Get Font" - "Get Embed Code"를 누르고 `@import`를 체크   
- 복사한 내용을 `html`의 `<style>`태그에 적용
- "CSS class"의 `font-family: "Nanum Pen Script", cursive;`를 원하는 곳에 사용   

<br>

## 부트스트랩 가져다 쓰기   

> 부트스트랩이란? 예쁜 CSS를 미리 모아둔 것!   

CSS를 이미 다를 줄 아는 것과 미적 감각을 발휘하여 예쁘게 만드는 것은 다른 이야이기입니다   
현업에서 미리 완성된 부트스트랩을 가져다 쓰는 경우가 많은데 저희도 가져와서 사용해보도록 합시다   

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>타이틀</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
</head>
```   

<br>

여기서 부트 스트랩의 버튼 클래스[(링크)](https://getbootstrap.com/docs/5.3/components/buttons/)를 하나 가져와보죠   
해당 링크의 HTML에서 클래스가 `btn btn-primary`인 첫 번째 줄을 복사해서 써보겠습니다   

![img](/assets/img/postimg/postimg014.png)   

그러면 이미 완성된 부트스트랩을 적용할 수 있습니다!   
버튼 뿐만 아니라 다른 다양한 CSS도 있으니 둘러보면서 사용하면 되겠습니다   