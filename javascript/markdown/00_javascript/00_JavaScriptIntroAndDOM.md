# 00_JavaScript Intro & DOM Manipulation

> 2020.10.12 오전 라이브

[강의 코드](https://lab.ssafy.com/ssafy4/javascript)



## 핵심 개념

**JavaScript의 역사**

*"파편화 & 표준화의 역사"*

- 브라우저 전쟁의 결과로 크로스 브라우징 이슈 발생했고 그 결과가 현재까지도 많은 영향을 주고 있다.
- JavaScript는 파편화된 요소가 하나의 표준으로 정립되는 과정을 이해하며 접근해야 한다.



**JavaScript로 할 수 있는 것**

1. DOM(Document Object Model) 조작
   - 문서(HTML)를 객체로 조작
2. BOM(Browser Object Model) 조작

   - navigator, screen, location, history

3. ECMAScript
   - 프로그래밍 언어로 할 수 있는 것
   - 변수, 타입, 조건문, 반복문, 함수 



**DOM(Document Object Model)**

HTML, XML 등과 같은 문서를 다루기 위한 독립적인 문서 모델 인터페이스 

- 모든 문서의 노드는 'DOM Tree'라고 불리는 트리 구조의 모습을 갖는다.

- `window`, `document`와 같은 객체들이 존재 하는데, `window`는 브라우저의 최상위 객체를 의미하며 생략 가능



**DOM Manipulation**

*문서 모델을 객체를 통해 조작*

> "각각의 요소를 직접 `console.log`를 통해 확인 해보세요."



1. 선택 (Selection)

   ```javascript
    //1. Selection - querySelector / querySelectorAll
   //1-1. window & document
   // console.log(window)
   // console.log(document)
   // console.log(window.document)
   
   //1-2. querySelector
   const mainHeader = document.querySelector('h1')
   const langHeader = document.querySelector('#langHeader')
   const frameHeader = document.querySelector('#frameHeader')
   // console.log(mainHeader, langHeader, frameHeader)
   
   //1-3. querySelectorAll
   const langLi = document.querySelectorAll('.lang')
   const frameworkLi = document.querySelectorAll('.framework')
   // console.log(langLi, frameworkLi)
   
   //1-4. 여러 개의 요소 -> 첫 번째로 일치하는 요소
   const selectOne= document.querySelector('.lang')
   // console.log(selectOne)
   
   //1-5. 복합 선택자
   const selectDescendant = document.querySelector('body li')
   // const selectDescendant = document.querySelectorAll('body li')
   const selectChild = document.querySelector('body > li')
   // console.log(selectDescendant) 
   // console.log(selectChild) 
   
   //1-6. getElementById, getElementByClassName 
   const selectSepcialLang = document.getElementById('specialLang')
   const selectAllLangs = document.getElementsByClassName('framework')
   const selectAllLiTags = document.getElementsByTagName('li')
   // console.log(selectSepcialLang, selectAllLangs, selectAllLiTags)
   ```

   - 단일 노드 

     `document.getElementById(id)`

     ​	id만 활용하여 선택할 수 있다. (querySelector에 비해 유연성이 떨어진다.)

     **`document.querySelector(selector)`**

     ​	id, class, tag, 복합 선택자(자손, 자식 선택자) 등을 모두 활용하여 선택할 수 있다.

   - 여러 개의 요소

     - HTMLCollection (live)

       `document.getElementsByTagName(tagName)`

       `document.getElementsByClassName(class)`

     - NodeList (non-live)

       **`document.querySelectorAll(selector)`**

2. 변경, 조작 (Manipulation)

   ```javascript
   //2. Manipulation
   //2-1. Creation
   const browserHeader = document.createElement('h2')
   const ul = document.createElement('ul')
   const li1 = document.createElement('li')
   const li2 = document.createElement('li')
   const li3 = document.createElement('li')
   // console.log(browserHeader, li1, li2, li3) 
   
   
   //2-2. innerText & innerHTML / append & appendChild
   browserHeader.innerText = 'Browsers'
   li1.innerText = 'IE'
   li2.innerText = '<strong>FireFox</strong>'
   li3.innerHTML = '<strong>Chrome</strong>'
   // console.log(browserHeader, li1, li2, li3)
   
   
   //2-3. append DOM 
   const body = document.querySelector('body')
   body.appendChild(browserHeader)
   body.appendChild(ul)
   
   // append와 appendChild의 차이점도 같이 확인해보세요!
   ul.append(li1, li2, li3) 
   // ul.appendChild(li1, li2, li3) 
   
   
   //2-4. Delete
   // removeChild
   // ul.removeChild(li1) 
   // ul.removeChild(li2)
   
   // remove
   // ul.remove()
   // body.remove()
   
   
   //2-5. Element Styling
   li1.style.cursor = 'pointer'
   li2.style.color = 'blue'
   li3.style.background = 'red'
   
   // setAttribute
   li3.setAttribute('id', 'king')
   
   // getAttribute
   const getAttr = li1.getAttribute('style')
   const getAttr2 = li3.getAttribute('style')
   console.log(getAttr)
   console.log(getAttr2)
   ```

   - 속성 변경

     `innerText` & `innerHTML`

     `element.style.color` & `element.style.textDecoration`

     `setAttribute(attribute, value)` & `getAttribute(attribute)`

   - 생성해서 DOM에 부착

     `document.createElement(tagName)`

     `appendChild` & `append`

     `removeChild` & `remove`



## 참고 사항

1. 주석 (comment)

   ```javascript
   // 한 줄 주석
   
   /*
   여러 줄로 
   표현하는 주석
   */
   ```

2. JavaScript 코드 사용법

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>example</title>
   </head>
   <body>
     <script>
       // 1. html에 script 태그 만들어서 작성
     </script>
   </body>
   </html>
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>example</title>
   </head>
   <body>
     <!-- 2. html에 script 태그 내부에 src 속성으로 파일을 불러와서 작성 -->
     <script src="index.js"></script>
   </body>
   </html>
   ```

   ```javascript
   // 2. index.js
   console.log(window)
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>example</title>
   </head>
   <body>
      <!-- 3. onclick -> inline으로 사용할 수 있지만 권장하지 않음 inline styling을 권장하지 않는 것과 동일 -->
    <button onclick="alertMessage()">나를 눌러봐!</button>
       
    <script>
      //3. onlclick
      const alertMessage = function () {
         alert('안녕하세요!')
      }
    </script>
   </body>
   ```
   
   

## 참고 링크

아래의 수업 시간에 참고한 자료거나 추가적인 학습에 도움이 될만한 링크입니다.

| 문서 제목                                                    | 비고                      |
| ------------------------------------------------------------ | ------------------------- |
| [ParentNode.append()](https://developer.mozilla.org/ko/docs/Web/API/ParentNode/append) | append vs appendChild     |
| [Node.textContent](https://developer.mozilla.org/ko/docs/Web/API/Node/textContent) | innerText vs textContent  |
| [JavaScript](https://developer.mozilla.org/ko/docs/Web/JavaScript) | MDN 문서                  |
| [JavaScript가 뭔가요?](https://developer.mozilla.org/ko/docs/Learn/JavaScript/First_steps/What_is_JavaScript) | MDN 문서                  |
| [Front-end web development](https://en.wikipedia.org/wiki/Front-end_web_development) | 프론트엔드 웹 개발의 요소 |