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
- 브라우저 환경에서만 정상동작한다.

### HTTP 요청 전송

- default로 비동기 요청 여부는 true이다.

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpReauest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 헤더 설정
// 서버로 전송할 데이터의 MIME 타입 지정
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

### send() 메서드

- 요청 몸체에 담아 전송할 데이터를 인수로 전달한다.
    - 객체면 JSON 형식으로 변환해야 한다.
- 요청 메서드 GET과 POST는 전송 방식의 차이가 있다.
    - GET
        - 데이터의 URL의 일부분인 쿼리 문자열로 서버에 전송
        - 페이로드 인수는 무시되고 null로 보낸다.
    - POST
        - 데이터(페이로드,  payload)를 요청 몸체에 담아 전송

### HTTP 요청 Header

- MIME 타입
    - 클라이언트에게 전송된 문서의 다양성을 알려주기 위한 메커니즘
- Content-type
    - 데이터의 MIME 타입
        - text: plain, html, css, javascript
        - application: json, x-www-form-unlencode
        - multipart: formed-data
- Accept
    - 서버가 응답할 MIME 타입
    - 미설정시 `*/*` 로 전송 → 모든 MIME 타입

### 응답 처리

- XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야한다.
- 이벤트 핸들러를 통해 `readyState` 프로퍼티 값이 변경되면 응답을 처리한다.
- readystatechange 이벤트를 통해 readyState를 감지하여 서버의 응답이 완료되었는지 확인한다.
    - load 이벤트를 사용해도 좋다. 이것은 HTTP 요청이 성공적으로 완료되면 발생한다.
- 응답이 완료되고, xhr.status가 200 xhr.response에서 서버에서 받은 데이터를 취득한다.
    - 에러면 유감..