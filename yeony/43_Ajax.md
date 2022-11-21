## 43.1 Ajax란?

- Asynchronous JavaScript an XML
- 자바스크립트를 사용하여
    - 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고
    - 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체 기반으로 동작한다.
- 전통적인 브라우저 동작 방식을 전환한 프로그래밍 방식이다.
    - 비동기 방식을 이용해 변경이 필요한 부분만 한정적으로 렌더링한다.
        - 비동기 방식이기 때문에 서버와의 통신에서 블로킹이 사라진다.

## 43.2 JSON

- JavaScript Object Notation
- 클라이언트와 서버간의 HTTP 통신을 위한 텍스트 포맷
    - 자바스크립트만의 데이터 포맷은 아니다.

### 표기 방식

- 객체 리터럴과 유사한 키와 값으로 구성된 텍스트
    - 키는  `“”` 로 묶어야 한다.
    - 값은 객체 리터럴과 같은 방식이다.

```jsx
{
	"name": "Jiyeon",
	"age": 20,
	"alive": true,
	"hobby": ["game", "study"]
}
```

### JSON.stringify

- 객체와 배열을 JSON 포맷의 문자열로 변환한다.
- 클라이언트에서 서버로 전송하려면 문자열화 해야한다. → `직렬화`

### JSON.parse

- JSON 포맷을 다시 객체와 배열 형태로 변환한다.
- 서버에서 전송받은 데이터를 클라이언트에서 사용하려면 객체화 해야한다. → `역직렬화`

## 43.3 XMLHttpRequest

- 브라우저 주소창이나 form 태그, a 태그를 이용해 HTTP 요청 전송 기능을 기본 제공한다.
- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpReauest객체를 사용한다.

### XMLHttpRequest 객체 생성

- 브라우저 환경에서만 정상동작한다.

```jsx
const xhr = new XMLHttpReauest();
```