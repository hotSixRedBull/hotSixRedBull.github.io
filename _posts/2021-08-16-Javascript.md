---
title: Update
author: Alan
layout: post
tags:
- Javascript
- Github
---

### update
I admit that i didn't post after a month of blog creation.
I'm planning to build a blog on cloud. Before the launch, let me update what i'm doing these days.

I'm reading a modern javascript tutorial [site](https://ko.javascript.info/) for better understanding of javascript.

Also, I finished a book about vim, and Reactjs.

I think leaving my understanding to blog might help preserving my memory. So, Although it's not the first chapter, let me summerize my understainding from now on.

# 9장. 클래스
## 9.1. 클래스와 기본 문법
- 기본문법
```
class MyClass {
    constructor() { ... }
    method1() { ... }
    ...
}
```
class는 기본적으로 함수이며, new 연산자를 통해 생성 시, 아래 동작을 수행한다.
1. constructor 함수를 실행, 없다면 빈 객체를 생성
2. 클래스 내부 메서드를 prototype에 저장

class 문법은 단순한 syntactic sugar, 편의문법이 아니다.
1. class로 만든 함수엔 특수 내부 프로퍼티인 \[\[FunctionKind\]\]:"classConstructor"가 이름표처럼 붙는다.
2. 클래스를 출력하면 class...로 시작하는 표현이 출력된다.
3. 클래스 메서드는 열거할 수 없다.
   1. 클래스 메서드의 enumerable 플래그는 false다.
4. 클래스는 항상 엄격 모드로 실행된다.

- 클래스 표현식
- 기본
```
let User = class {
    sayHi() {
        alert("Hello");
    }
}
```
- 기명 클래스 표현식
    - 이름은 내부에서만 사용 가능
```
let User = class MyClass {
    ...
}
alert(MyClass); // ReferenceError: MyClass is not defined, MyClass는 클래스 밖에서 사용할 수 없습니다.
```
- 동적으로 생성
```
function makeClass() {
    return class {
        ...
    };
}
```
- getter, setter
```
class User {
    get name() {
        ...
    }
    set name(value) {
        ...
    }
}
```
- 대괄호를 이용하여 계산된 메서드를 만들 수 있다.
```
class User {
    ['say' + 'hi']() {
        ...       
    }
}
```

new User().sayhi();
- 클래스 필드
    - 메서드가 아닌 속성값도 할당 가능.
    - prototype이 아닌 해당 객체에 저장된다.
    - 복잡한 표현식이나 함수 호출 결과도 사용할 수 있다.

## 9.2 클래스 상속
### `extends`
- extends 키워드를 쓰면, 해당 클래스의 prototype에 prototype이 지정된다.
  - 따라서, prototype의 prototype을 탐색하며 메서드를 가져올 수 있다.
- extends 뒤에는 표현식이 올 수도 있다.
```
function f(phrase) {
  return class {
    sayHi() { alert(phrase) }
  }
}

class User extends f("Hello") {}

new User().sayHi(); // Hello
```
### 메서드 오버라이딩
- `super` 를 사용
- super.method(...)는 부모 클래스에 정의된 메서드, method를 호출합니다.
- super(...)는 부모 생성자를 호출하는데, 자식 생성자 내부에서만 사용 할 수 있습니다.
- 화살표 함수는 super를 지원하지 않습니다.

### 생성자 오버라이딩
- 생성자를 오버라이딩 할때는 반드시 부모클래스의 생성자를 super()를 먼저 호출해야하며, 그러지 않은 경우, 에러가 발생한다.
- 상속받는 자식 클래스의 생성자는 this에 객체를 할당하지 않기 때문.
  - 상속받는 자식 클래스의 생성자는, `[[ConstructorKind]]:"derived"`라는 값을 가지며 구분되어 처리된다.

### 나중에 읽도록 권장되어 뛰어넘은 절
### 클래스 필드 오버라이딩: 까다로운 내용
### super 키워드와 `[[HomeObject]]`

## 9.3 정적 메서드와 정적 프로퍼티
### 정적 메서드
- `static` 키워드를 통해서 클래스 함수 자체에 메서드를 설정할 수 있다.
```
class User {
  static staticMethod() {
    alert(this === User);
  }
}

User.staticMethod(); // true
```
### 정적 프로퍼티
- chrome에서만 동작할 수도 있는 최신 문법.
```
class Article {
  static publisher = "Ilya Kantor";
}

alert( Article.publisher ); // Ilya Kantor
```
### 정적 프로퍼티와 정적 메서드의 상속
- extends를 쓰면, 정적 프로퍼티와 메서드도 상속된다.

## 9.4 private, protected 프로퍼티와 메서드
### protected
- 자바스크립트는 protected를 흉내낸다.
- protected 프로퍼티 양 옆엔 `_` 밑줄이 붙는다.(관례적)
```
class CoffeeMachine {
  _waterAmount = 0;

  set waterAmount(value) {
    if (value < 0) throw new Error("물의 양은 음수가 될 수 없습니다.");
    this._waterAmount = value;
  }

  get waterAmount() {
    return this._waterAmount;
  }

  constructor(power) {
    this._power = power;
  }

}

// 커피 머신 생성
let coffeeMachine = new CoffeeMachine(100);

// 물 추가
coffeeMachine.waterAmount = -10; // Error: 물의 양은 음수가 될 수 없습니다.
```

### 읽기 전용 프로퍼티
- getter만 설정함으로써 읽기 전용 프로퍼티를 만들 수도 있다.

### private 프로퍼티
- 최신 문법주의, 폴리필 구현이 필요할 수 있음.
- private 프로퍼티와 메서드는 #으로 시작합니다. #이 붙으면 클래스 안에서만 접근할 수 있습니다.
- private 필드는 `this[name]`으로 접근할 수 없습니다. 
```
class CoffeeMachine {
  #waterLimit = 200;

  #checkWater(value) {
    if (value < 0) throw new Error("물의 양은 음수가 될 수 없습니다.");
    if (value > this.#waterLimit) throw new Error("물이 용량을 초과합니다.");
  }

}

let coffeeMachine = new CoffeeMachine();

// 클래스 외부에서 private에 접근할 수 없음
coffeeMachine.#checkWater(); // Error
coffeeMachine.#waterLimit = 1000; // Error
```

## 9.5 내장 클래스 확장하기
- 배열, 맵과 같은 내장 클래스도 확장이 가능하다.
```
// 메서드 하나를 추가합니다(더 많이 추가하는 것도 가능).
class PowerArray extends Array {
  isEmpty() {
    return this.length === 0;
  }
}

let arr = new PowerArray(1, 2, 5, 10, 50);
alert(arr.isEmpty()); // false

let filteredArr = arr.filter(item => item >= 10);
alert(filteredArr); // 10, 50
alert(filteredArr.isEmpty()); // false
```
### 내장 객체와 정적 메서드 상속
- 내장 객체는 Object.keys, Array.isArray 등의 자체 정적 메서드를 갖습니다.
- 내장 클래스는 정적 메서드를 상속받지 못한다.
    - prototype끼리는 상속이 되지만, 내장 객체 사이의 상속은 없기 때문.

## 9.6 'instanceof'로 클래스 확인하기
- `instanceof` 연산자를 사용하면 객체가 특정 클래스에 속하는지 아닌지를 확인할 수 있습니다. instanceof는 상속 관계도 확인해줍니다.
### instanceof 연산자
```
obj instanceof Class
```
- obj가 Class에 속하거나 Class를 상속받는 클래스에 속하면 true가 반환됩니다.
- 클래스에 정적 메서드 Symbol.hasInstance가 구현되어 있으면, obj instanceof Class문이 실행될 때, `Class[Symbol.hasInstance](obj)`가 호출됩니다. 호출 결과는 true나 false이어야 합니다. 이런 규칙을 기반으로 instanceof의 동작을 커스터마이징 할 수 있습니다.
ㄱ
## 9.10 믹스인
- 여러 클래스를 상속받지 못하는 것을 우회하는 방법.
- `Object.assign()`을 이용한다.