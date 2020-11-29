---
title: [ES2020]Optional Chaining ?.
layout: post
author: Creatijin
category:
  - javascript
thumbnail: "/assets/img/posts/javascript-posting-optionalchaining.jpg"
date: "2020-11-28 06:06:00"
summary: Optional Chaining ?.

---

# [ES2020]Optional Chaining ?. 

ES2020에 추가된 문법 Optional Chaining을 공부해보자

## optional chaining이 필요해!

```javascript
let user = {
  name: {
    first: "jo",
    last: "seungjin"
  }
}
user.name.first // "jo"
user.address.street //Uncaught TypeError: Cannot read property 'street' of undefined

//&& 연산자로 해결
user.address && user.address.street;

//lodash 유틸리티 라이브러리 사용
import { get } from "lodash";
get(user, "address.street");
```

위와 같이 자바스크립트에서 `.` 는 참조가 nullish (null 또는 undefined)이라면, 에러가 발생한다. 여러 사례들이 있지만 보통 이런 문제들은 `&&`연산자 또는 `lodash` 같은 유틸리티 라이브러리를 사용하여 문제를 해결했다. `?.` 는 위와 같이 지저분한 코드를 안전하고 깔끔하게 정리하기 위해서 나왔다.

---



## optional chaining의 등장 `?.` 

optional chaining `?.` 연산자는 체인의 참조 속성 값을 읽어 nullish 로 인해 에러가 발생하는 것 대신 표현식의 리턴 값은 `undefined`가 출력된다.

```javascript
let user = {};
user.address?.street //undefined
```

`?.` 로 에러가 발생하지 않고 undefined를 출력한다.  `user.address.street`에 접근하기 전에 `user.address`가 `null` 또는 `undefined` 가 아니라는 것을 암묵적으로 확인 하는 것을 알 수 있다.

**`?.` 연산자는 함수,배열에도 사용 가능하다.**

~~~javascript
let someInterface = () => {}
let result = someInterface.customMethod?.(); // undefined
let arr = [];
arr?.[0] // undefined
~~~

존재하지 않을 수 있는 메서드를 호출할 때 optional chaining을 사용할 수 있다.

함수 호출과 optional chaining을 사용함으로 메서드를 찾을 수 없는 경우에 에러를 발생시키는 것 대신에 `undefined` 를 반환한다.

**`?.`은 `delete`와 함께 사용할 수 있다.**

~~~javascript
let user = {name:{}}
delete user?.name; // user가 존재한다면 user.name을 삭제
user // {}
~~~



---

## 주의

- `?.`은 연산자가 아니며, `?.`은 함수나 대괄호와 함께 동작하는 특별한 문법 구조체(syntax construct)다.

- `?.` 는 존재하지 않아도 상관 없는 대상에게만 사용을 권장한다. 남용하다보면 디버깅에 상당히 어려워진다.

- `?.` 사용시에 앞에 대상은 변수로 선언되어 있어야한다 그렇지 않으면 에러가 발생한다.

- `?.`은 읽기나 삭제하기에는 사용할 수 있지만 쓰기에는 사용할 수 없다.

- `?.`는 왼쪽 평가대상에 값이 없다면 즉시 평가를 멈춘다. 이런 평가 방법을 단락 평가라 하며 함수 호출을 비롯한 `?.` 오른쪽에 있는 부가 동작은 `?.`의 평가가 멈췄을 때 더 진행되지 않는다.

  ```javascript
  let user = null;
  let x = 0;
  
  user?.bell(x++);
  alert(x); // 0
  
  //x는 증가하지 않습니다.
  ```



## 마무리

`?.`  연산자와 `.` 연산자는 유사하게 작동하지만 `?.` 는 그 속성이 존재하는 경우에만 chaining을 계속하고 존재하지 않는다면 `undefined`를 반환한다.

꼭 있어야 하는 값이지만 없는 경우에 `?.`을 사용하면 프로그래밍 에러를 쉽게 찾을 수 없으므로 이런 상황을 만들지 말도록 한다.

`?.` 연산자는 2020년 기준 최신 브라우저에서 대부분 지원하기 때문에 사용가능하다. 그 이전 버전의 경우 Babel 같은 트랜스파일러를 통해 사용 가능하다.

### 참고

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://www.daleseo.com/js-optional-chaining/
- https://ko.javascript.info/optional-chaining