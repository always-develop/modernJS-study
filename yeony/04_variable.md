## 4.1 변수란 무엇인가? 왜 필요한가?

---

- 컴퓨터의 연산은 `CPU` 에서, 기억은 `메모리`에서 수행한다.

### 예시 : 컴퓨터가 연산하는 과정

- 아래의 식을 자바스크립트로 작성한 식을 자바스크립트 엔진이 계산한다면?
    
    > 10 + 20
    > 
    1. 10, +, 20 에 대한 의미를 알고 있어야 한다. → `메모리`
    2. 의미를 알고 적절히 해석할 줄 알아야 한다. → `CPU`

- 컴퓨터가 연산을 완료하면 결과 값(30)을 메모리에 저장할 것이다.
- 하지만 해당 결과 값을 재사용하는 것은 불가능하다.
    - 메모리 주소에 직접 접근하는 행위는 치명적인 오류를 발생시킬 수 있기 때문에 자바스크립트로는 메모리 제어를 애초에 허용하지 않는다.
    - 메모리 제어를 허용한다 하더라도 그때그때의 상황마다 저장될 메모리 주소는 변경될 것이기 때문에 이전에 값이 저장된 메모리 주소를 알 수 없다.
- 그렇기 때문에 우리는 변수를 사용한다.
- `변수`는 하나의 값을 저장하기 위해 확보한 **메모리 공간**이며 해당 **메모리 공간을 식별**하여 사용하기 위해 붙인 임의의 이름이다. 또한 값의 위치이다. 즉 변수는 해당 메모리의 메모리 주소를 담는 것이다.
- `할당` : 변수에 값을 저장하는 것
- `참조` : 변수에 할당된 값을 읽는 것

> **메모리 : 컴퓨터가 데이터를 저장하는 방법**
> 
> - 메모리 `cell`의 집합체
> - 1cell = 1bite = 8bit
> - 메모리는 1cell 단위로 데이터를 저장하고, 읽는다.
> - 각 cell은 고유의 주소를 가지고 있으며 주소는 0부터 메모리의 크기만큼 정수로 표현한다.

## 4.2 식별자

---

- 변수 이름 = 식별자
    - 어떤 값을 구별해서 식별할 수 있는 고유한 이름
- 변수, 함수, 클래스 이름들을 모두 식별자라고 말할 수 있다.
- `선언` : 자바스크립트 엔진에 식별자의 존재를 알리는 것

## 4.3 변수 선언

---

- 변수를 생성하는 첫 단계이며 메모리 공간을 확보하고, 메모리 공간에 값을 저장(할당)하기 위한 준비
- 변수가 선언되는 순간부터 확보된 메모리 공간은 확보가 해제되기 전까지 사용할 수 없다.

### 변수를 선언하는 방법 : 키워드

- `var`, `let`, `const`
- 사용 문법

```jsx
var score;
```

- 선언한 변수에 할당을 하기 전까지는 자바스크립트 엔진에 의해 `undefined` 라는 값을 가지게 된다.

> **자바스크립트 엔진이 변수 선언 시 하는 것**
> 
> 1. 선언 단계 : 변수 이름을 등록해 자바스크립트 엔진에 변수의 존재를 알린다.
> 2. 초기화 단계 : 값을 저장하기 위한 메모리 공간을 확보하고 암무적으로 `undefined` 를 할당해 값을 초기화한다.
- `초기화` : 변수가 선언된 이후 최초로 값을 할당하는 것

- 값을 초기화하지 않으면 확보된 메모리 공간에 이전 다른 어플리케이션이 사용했던 값이 남아있을 수 있고 우리는 그 값들을 쓰레기 값이라고 한다.
- 또한 선언하지 않은 식별자에 참조를 하려한다면 `ReferenceError`가 발생한다.

## 4.4 변수 선언의 실행 시점과 변수 호이스팅

---

```jsx
console.log(score);
var score;
```

- 위의 코드를 자바스크립트 엔진에서 실행하면 `ReferenceError`가 발생할 것 같지만 아니다.
- 자바스크립트 코드는 인터프리터에 의해 한 줄씩 순차적으로 실행된다.
- 그러나 변수 선언은 소스 코드가 순차적으로 실행되기 전, 즉 런타임 이전에 먼저 실행된다.
- 이런 현상을 **변수 호이스팅**이라고 부른다.

## 4.5 값의 할당

---

- 할당은 `=` 연산자를 이용한다.
- 선언과 할당은 한 줄로 표현할 수 있다.

```jsx
var score = 80;
```

- 표현은 하나지만 실제 실행은 선언과 할당 2단계로 나눠서 진행된다.
- 변수 호이스팅 현상은 변수 선언에 제한된다.
    - 변수 선언 자체는 런타임 이전 실행되지만 변수 할당은 런타임 때 실행된다. 즉 할당은 전체 코드에서 순차적으로 이루어진다.
- 예시

```jsx
// var score; 변수 선언 후 엔진에 의해 undefined로 자동 값 초기화
console.log(score); // undefined
var score = 80;

// score = 80; 런타임 진행시 undefined -> 80 으로 재할당
console.log(score); // 80
```

- 여기서 유의할 점은 재할당을 한다는 것은 처음 초기화된 메모리 공간을 재사용한다는 것이 아닌 초기화 값 메모리 공간 외 새로운 메모리 공간을 확보하여 새로운 값을 저장하는 것이다.

## 4.6 값의 재할당

---

- `var` 키워드로 선언한 변수는 값을 재할당할 수 있고, 애초에 `var` 을 이용해 선언을 하는 것은 `undefined`로 값을 초기화시킨 것이고, 그 이후 할당은 재할당이라고 볼 수 있다.
- 변수에 재할당을 한다는 것은 항상 변경될 수 있다는 것이고, 만약 값을 재할당할 수 없어 변수에 저장된 값을 변경할 수 없다면 그것은 **상수(constant)**이다.
- 변수 중 한 번만 할당할 수 있는 변수를 상수라고 부른다.

### 메모리 누수 : 가비지 콜렉터

- 변수를 재할당하는 행위를 계속하게된다면 위에서 언급했듯 처음 선언된 메모리 공간을 사용하는 것이 아닌 계속해서 새로운 공간을 확보하게된다.
- 그렇다면 재할당 이전의 공간들은 어떻게 될까?
- 식별자가 없는 메모리 공간은 더이상 참조할 수 없게된다.
- 더이상 사용할 수 없는 메모리 공간들은 가비지 콜렉터에 의해 주기적으로 검사되고 메모리 해제가 된다.
    - 자바스크립트는 매니지드 언어로서 가비지 콜렉터를 통해 메모리 누수를 방지한다.
    

> 언매니지드 언어와 매니지드 언어
> 
> - 메모리 관리 방식에 따라 프로그래밍 언어를 분류
> - 언매니지드 언어 : 메모리 제어를 개발자가 주도할 수 있음. 생산성은 떨어질 수 있으나 최적화하기 쉬움.
> - 매니지드 언어 : 메모리 제어를 개발자에게 허용하지 않기 때문에 메모리 해제는 가비지 콜렉터가 수행하게 됨. 생산성은 올라가나 최적화하기 어려움.

## 4.7 식별자 네이밍 규칙

---

- 식별자는 고유한 이름이기 때문에 규칙이 존재한다.
    - 특수문자를 제외한 문자, 숫자, 언더스코어(_), 달러 기호($) 포함 가능
    - 특수문자를 제외한 문자, 언더스코어(_), 달러 기호($)로 시작 해야하며 숫자로 시작할 수 없음
    - 예약어는 식별자로 사용할 수 없음
- 또한 식별자는 대소문자를 구별하기 때문에 같은 의미의 단어라도 대소문자가 다르면 다른 식별자로 인식한다.

<aside>
❗ **예약어?**
프로그래밍 언어에서 이미 사용되고 있고나 사용될 예정인 단어

</aside>

### 식별자 이름으로 권장하지 않는 형식

```jsx
// 1. 한 줄에 여러 변수를 선언하는 것 : 가독성 저하
var person, $elem, _name, first_name, val1;

// 2. 알파벳 외 유니코드 문자로 변수를 선언하는 것
var 한국어, (일본어);

// 3. 주석이 필요한, 의미없는 변수 이름
var dd = 3;
```

### 자주  사용되는 네이밍 컨벤션

```jsx
// camelCase : 변수 / 함수
var firstName;

// snake_case
var first_name;

// PascalCase : 생성자 함수, 클래스 이름
var FirstName;

// typeHungarianCase
var strFirstName; // type + identifier
var $elem = document.getElementById('myId'); // DOM Node
var observable$ = fromEvent(document, 'click'); // RxJS Observable
```