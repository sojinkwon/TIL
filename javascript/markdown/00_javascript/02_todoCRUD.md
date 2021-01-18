# 02_todo CRUD

> 2020.10.12 오후 라이브

[강의 코드](https://lab.ssafy.com/ssafy4/javascript)



## 핵심 개념

**Todo CRUD Operation**

- Create
  - todo 요소 생성
- Read
  - todo 요소가 DOM에 부착
- Update
  - todo 요소를 클릭하면 취소선이 토글
- Delete
  - `X` 버튼을 누르면 todo 삭제



## TODO CURD Operation

1. **기본 마크업**

   ```html
   <input type="text">
   <button>Todo</button>
   <ul></ul>
   ```

2. **Create** & **Read**

   ```javascript
   // Chrome Console
   
   //1. input 선택 및 value 값 선택
   const input = document.querySelector('input')
   const content = input.value
   
   //2. li 태그 생성
   const li = document.createElement('li')
   
   //3. 생성한 li의 내부의 text를 content 안에 들어있는 값으로 추가
   li.innerText = content
   
   //4. ul 태그를 잡아서 li 태그를 붙이자
   const ul = document.querySelector('ul')
   
   ul.appendChild(li)
   ```

   위의 로직 그대로 script로 옮기면 동작하지 않는다. input에 요소를 쓰기 전에 이미 코드가 실행되기 때문이다. 함수로 만들어 우리가 원하는 시점에 실행시켜보자

   ```javascript
   function addTodo () {
     const input = document.querySelector('input')
     const content = input.value
   
     const li = document.createElement('li')
     li.innerText = content
     
     const ul = document.querySelector('ul')
   
     ul.append(li)
   }
   ```

   이제는 함수를 우리가 직접 호출하는 것이 아니라 '클릭 이벤트가 발생 했을 때'라는 조건아래 실행되도록 설정하자. 이때 `addEventListener`를 활용할 수 있다.

   ```javascript
   const button = document.querySelector('button')
   
   button.addEventListener('click', function () {
     const input = document.querySelector('input')
     const content = input.value
   
     const li = document.createElement('li')
     li.innerText = content
   
     const ul = document.querySelector('ul')
     ul.appendChild(li)
       
     // input 초기화
     input.value = ''
   })
   ```

   버튼 뿐만 아니라 Enter를 눌렀을 때만 값이 추가되도록 설정

   ```javascript
   const button = document.querySelector('button')
   const input = document.querySelector('input') 
   
   function addTodo () {
     ...
   }
   
   button.addEventListener('click', addTodo)
   input.addEventListener('keydown', function (event) {
     // console.log(event) // code -> Enter 
     // Enter 키를 눌렀을 때만 addTodo 함수 실행
     if (event.code === 'Enter') {
       addTodo()
     }
   })
   ```

   값이 비어있는 경우에 대한 예외처리 (빈 값을 넣을 경우 경고창까지 같이 보여주자)

   ```javascript
   ...
   
   function addTodo () {
     const content = input.value
     // if (content !== '') {}
     if (content) {
       const li = document.createElement('li')
       li.innerText = content 
   
       const ul = document.querySelector('ul')
       ul.appendChild(li)
   
       input.value = ''
     } else {
   		alert('할 일을 입력해주세요!')
   	}
   }
   ```

3. **Update**

   >  "li 태그를 **누르면** todo에 취소선을 토글한다."

   1. `.length` 프로퍼티 활용

      ```javascript
      // if (event.target.clssList.length === 0) 
      if (event.target.classList.length) {
          event.target.classList.remove('done')
      } else {
          event.target.classList.add('done')
      }
      })
      ```

   2. `.contains` 메서드 활용

      ```javascript
      if (event.target.classList.contains('done')) {
        event.target.classList.remove('done')
      } else {
        event.target.classList.add('done')
      }
      ```

   3. `toggle` 메서드 활용

      ```
      event.target.classList.toggle('done')
      ```

4. **Delete**

   >  "X 버튼을 **누르면** todo를 삭제**한다.**"

   ```javascript
   li.addEventListener('click', function (event) {
     event.target.classList.toggle('done')
   })
   
   // 버튼 생성
   const deleteButton = document.createElement('button')
   deleteButton.innerText = 'X'
   deleteButton.style.marginLeft = '10px'
   
   // 이벤트리스너
   deleteButton.addEventListener('click', function () {
     li.remove()
   })
   ```



## 참고 링크

아래의 수업 시간에 참고한 자료거나 추가적인 학습에 도움이 될만한 링크입니다.

| 문서 제목                                                    | 비고                         |
| ------------------------------------------------------------ | ---------------------------- |
| [이벤트 참조](https://developer.mozilla.org/ko/docs/Web/Events) | 다양한 이벤트 목록 확인      |
| [GlobalEventHandlers](https://developer.mozilla.org/ko/docs/Web/API/GlobalEventHandlers) | 다양한 이벤트 핸들러(처리기) |