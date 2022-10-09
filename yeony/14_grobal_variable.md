## 14.1 변수의 생명 주기

### 14.1.1 지역 변수의 생명 주기

- 변수에도 생명 주기가 있다.
    - 없으면 메모리 공간을 계속 점유한다.
- 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸한다.
    - 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다. (스코프 내부에서만 사용 가능)
- 지역 변수가 함수보다 오래 생존하는 경우도 있다.
    - 누군가가 해당 메모리 공간을 참조하고 있으면 메모리 공간이 해제되지 않는다.
    - 스코프 또한 다른 곳에서 참조 중이면 소멸하지 않는다.

```jsx
var x = 'global';

function foo() { // 함수가 실행되며 x가 undifined로 초기화됨
	console.log(x); // undifined
	var x = 'local';
}

foo();
console.log(x); // grobal
```

- 호이스팅은 스코프 단위로 동작한다.

### 14.1.2 전역 변수의 생명 주기

- 진입점 없이 코드가 실행되자마자 해석된다.
    - 반환문이 따로 없으므로 마지막 문이 실행되어 더 이상 실행될 문이 없다면 종료한다.
- var 로 선언한 전역 변수는 전역 객체의 프로퍼티가 된다.
    - 전역 변수의 생명 주기가 `전역 객체`의 생명 주기와 일치한다.

> **전역객체**
- 코드 실행 이전 단계, 자바스크립트 엔진에 의해 가장 먼저 생성되는 객체
- 브라우저에서는 window, 노드에서는 global
- ES11에서는 globalThis로 통일
- 프로퍼티 = 표준 빌트인 객체 + 호스트 객체 + var 키워드로 선언한 전역 변수와 전역 함수
> 

## 14.2 전역 변수의 문제점

### 암묵적 결합

- 전역에서 참조하고 변경이 가능하다.
    - 암묵적 결합을 허용하는 것

### 긴 생명 주기

- 생명 주기가 전역 객체와 동일하기 때문에 메모리 리소스를 오랫동안 소비한다.

### 스코프 체인 상에서 종점에 존재

- 변수 검색시 가장 마지막에 검색된다.
    - 검색 속도가 느리다.

### 네임스페이스 오염

- 파일이 분리되어 있어도 하나의 전역 스코프를 공유한다.

## 14.3 전역 변수의 사용을 억제하는 방법

- 변수의 스코프는 좁으면 좁을수록 좋다.
- 무분별한 전역 변수의 사용을 막기 위해서라면?

### 14.3.1 즉시 실행 함수

- 함수 정의와 동시에 단 한번만 호출되는 점을 이용한다.
- 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.
- 라이브러리에 많이 사용된다!

### 14.3.2 네임스페이스 객체

- 전역에 네임스페이스 역할을 담당할 객체를 생성 후 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가한다.
- 네임스페이스 자체가 전역 변수에 할당되어서 유용하게 쓰이진 않는다.

### 14.3.3 모듈 패턴

- 클래스를 모방해서 관련 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈로 만드는 것
- 클로저 기반으로 동작
- 전역 변수의 억제와 캡슐화까지 구현 가능하다.
    - 캡슐화
        - 객체의 상태를 나타내는 프로퍼티와 메서드를 하나로 묶는 것

> 객체지향 프로그래밍 언어는 클래스를 구성하는 멤버에 대해 **접근 제한자**를 제공한다.
> 
> 
> `public` : 외부에서 접근 가능
> `private` : 외부에서 접근 불가, 내부에서만 사용됨
> 
- 자바스크립트는 접근 제한자를 제공하지 않는다.
    - 즉시 실행  함수에 선언한 변수는 `private` 멤버가 된다.
    - 즉시 실행 함수에서 `return` 되는 객체의 프로퍼티는 `public` 멤버가 된다.

### 14.3.4 ES6 모듈

- ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.
- script 태그에 `type="module"` 을 추가하면 파일은 모듈로써 동작한다.
    - 권장 확장자는 `mjs`

```html
<script type="module" src="lib.mjs" />
```