# [javascript] ☕ 모던 자바스크립트

---

## _14장. 전역 변수의 문제점-Setter 를 쓰지 말아야 하는 이유

### Setter 란

- 특정 객체의 프로퍼티를 재할당하기 위해 프로퍼티와 일대일로 대응되는 set 메서드

### Setter 가 남용되는 이유

- 만들기 쉽다
- 편하다
- 객체를 참조할 수 있다면 객체의 상태(== 프로퍼티)도 쉽게 바꿀 수 있다
- 많이 사용되기 때문에 IDE 단에서 자동완성을 해주거나 아예 컴파일 단계에서 자동완성을 해줘버리는 라이브러리도 존재함
    - 자바 진영의 Lombok 이 그렇다

### Setter 의 단점

- 전역 변수의 문제점과 상당히 비슷하다
    - 객체의 상태를 어디에서든지 바꿀 수 있음
    - 그렇기 때문에 예기치 못한 부수 효과(side-effect) 를 불러일으킬 수 있음
- 객체가 어떤 상태를 가지는지 알야아함
    - 클라이언트(객체를 사용하는 입장)에서 너무 많은 정보를 알아야함
    - e.g. 가령, 프론트엔드에서 API 를 호출할 때.. 실제 백엔드단의 DB 의 구성까지 알아야 하는 상황
    - 요약하자면 TMI
    - 클라이언트가 필요한 정보만 제공할 수 있도록 하는 것이 Best
- 의도를 파악하기 힘든 이름
    - `손을 잡는다` 라는 메서드와 `수지 관절의 관절 각도를 n 도 움직여서 근육의 수축 운동을 수행한다` 라는 메서드는 똑같이 `손` 이라는 객체의 행동을 똑같이 정의한 것
    - 전자가 훨씬 알아보기 쉽듯, 객체의 행동의 이름은 간결하고 한 눈에 알아볼 수 있어야 함
        - `무엇을` 수행하는지
        - 사람에게 친화적인지(사람이 코드를 읽으니까요)
        - 내가 만든 소프트웨어가 해결해야할 영역의 용어를 사용 중인지(짧게 말해 도메인 용어)
    - 마찬가지로 get… 와 같은 Getter 도 지양해야하나, Getter 는 예측 불가능한 부수 효과를 일으키지는 않기 때문에 비교적 단점이 많이 부각되지 않는듯 하다

### Setter 쓰지 말기

- “그렇다면 Setter 메서드만 안만들면 되겠군” 이 아니라, 객체의 상태를 누구나 변경하게 두지 말아야 하는 것이 핵심
- 그렇다고해서 상태를 아예 바꾸지 말라는건 아님
    - 물론, 불변 상태의 객체가 훨씬 안정성이 뛰어나기 때문에, 특히 함수형은 불변성을 지향하기 때문에 상태를 수정하지 않는 것이 제일 좋음
- 객체의 상태를 바꾸는, 좀 더 `의미 있는` 이름을 붙인 메서드를 만드는 것을 목표해야 함

### 캡슐화

- 캡슐화란, 객체가 가지는 상태를 외부로부터 숨기고 상태에 접근하는 행위들을 의미 있는 메서드로 만들어 외부에 공개하는 것

```java
// Getter, Setter 가 남용되어 캡슐화가 되지 않은 코드 예시
class User {
    // private 으로 선언한 식별자는 자신이 선언된 클래스 바깥에선 접근할 수 없음
    private String name;
    private String address;
    private String phone;

    // public 으로 선언한 식별자는 모든 클래스에서 접근할 수 있음
    public String getName() {
        return name;
    }

    public String getAddress() {
        return address;
    }

    public String getPhone() {
        return phone;
    }

    public void setName(String name) {
				// 자바에서의 this 는 new User() 를 통해 생성된 클래스의 인스턴스를 가리킵니다
        this.name = name;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }
}

class UpdateMyInformation {
		// setter 가 사용될 경우 객체를 사용하는 쪽(클라이언트)에서 객체 내부 상태를 알아야 한다
		// user 를 참조하는 곳이 많아질 수록 아래와 같은 코드가 중복된다
    static void editPrivacy(User user, String address, String phone) {
        user.setAddress(address);
        user.setPhone(phone);
    }

		// getter 도 클린코드 측면에서는 setter 와 단점을 거의 공유한다
    static void checkPrivacy(User user) {
        System.out.println("my name: " + user.getName());
        System.out.println("my address: " + user.getName());
        System.out.println("my phone: " + user.getName());
    }
}
```

```java
// Getter, Setter 를 사용하지 않고 객체의 상태를 의미 있는 이름의 행동으로 캡슐화한 코드 예시
class User {
    private String name;
    private String address;
    private String phone;

		// getter, setter 대신 의미 있는 이름을 부여함
    public void editMyPrivacy(String address, String phone) {
        this.address = address;
        this.phone = phone;
    }

    public void checkMyPrivacy() {
        System.out.println("my name: " + name);
        System.out.println("my address: " + address);
        System.out.println("my phone: " + phone);
    }
}

class UpdateMyInformation {
		// 객체를 사용하는 쪽에서도 간단한 코드를 만들 수 있음
		// 객체가 실제로 어떤 속성을 가졌는지는 알 수 없음
    static void editPrivacy(User user, String address, String phone) {
        user.editMyPrivacy(address, phone);
    }

    static void checkPrivacy(User user) {
        user.checkMyPrivacy();
    }
}
```

### 참고

- [https://velog.io/@backfox/setter-쓰지-말라고만-하고-가버리면-어떡해요](https://velog.io/@backfox/setter-%EC%93%B0%EC%A7%80-%EB%A7%90%EB%9D%BC%EA%B3%A0%EB%A7%8C-%ED%95%98%EA%B3%A0-%EA%B0%80%EB%B2%84%EB%A6%AC%EB%A9%B4-%EC%96%B4%EB%96%A1%ED%95%B4%EC%9A%94)