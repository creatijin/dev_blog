---
title: Typescript(1)
layout: post
author: Creatijin
category:
- Typescript
thumbnail: "/assets/img/posts/typescript_Logo.png"
date: '2020-07-03 08:32:00'
summary: 타입스크립트 소개와 타입
---

# Typescript(1)

## 소개

자바스크립트는 동적 언어(dynamic language)로써 컴파일 하는 데 시간이 들지 않고 동적 타이핑(dynamic typing)을 수행하기 때문에 런타임(run time)시 속도가 빠르다. 안정성(type safety)을 포기하고 속도를 택했다.

하지만 타입스크립트는 정적 언어로써 컴파일 타임(compile time)에 타입을 검사하며 시간을 드려 안정성을 보장한다. 그러나 타입스크립트는 단지 컴파일 단계까지만 사용되기 때문에 실행은 자바스크립트로 이뤄진다. 이것은 타입스크립트가 자바스크립트를 완전히 대체한다기 보다 **보완하는 역활**이라 보는게 맞다. 

**타입스크립트 = 자바스크립트 + 타입** (타입스크립트의 가장 두드러지는 특징은 '타입')



## 타입 검사와 타입 선언

### 점진적 타입 검사

점진적 타입에 대해서 말하기 전에 언어에 따라서 수행하는 타입 검사의 종류는 크게 몇가지가 있다.

- 정적 타입 검사(staically type checking) [자바, C++ 등]
- 동적 타입 검사(dynamiccally type checking) [자바스크립트]
- 점진적 타입 검사 (gradually type checking) [타입스크립트, 파이썬]

점진적 타입 검사는 컴파일 시간에 타입 검사를 수행하면서 필요에 따라 타입 선언 생략을 허용한다.
타입 선언을 생략하면 암시적(implicit) 형 변환이 일어난다.

타입스크립트에서 any 타입은 모든 타입의 최상위 타입이며, 점진적 타이핑(gradual typing)을 설명할 때 적절한 예의다.
any는 동적 타입과 정적 타입의 결계선에 있는 타입으로 타입스크립트에서는 특별히 다뤄진다.
**any 타입으로 선언된 변수는 어떤 타입의 변수도 받아들이면서 심지어 타입이 없는 변수도 받아들인다.**

### 타입 계층도

![타입스크립트 타입 계층도](https://okdevtv.com/md/typescript/images/typescript.mmd.png)

타입 계층도에 가장 상위는 any 타입이며, 그 아래로는 다음과 같은 타입이 있다.

- 기본 타입
- 객체(object) 타입
- 기타 타입(유니언 타입, 인터섹션 타입)

#### 기본 타입(primitive types)

보편적으로 많이 사용되는 내장 타입으로 타입스크립트에서 지원하는 기본 타입의 종류는 다음과 같다.

- string

  string 타입은 '', ""를 사용해 문자열을 변수에 할당할 수 있지만 타입스크립트 스타일 가이드에서는 문자열 값은 ""(큰따옴표)를 이용할 것을 권장한다. 문자열 값을 표현할 때 역따옴표(backtick/backquote)를 이용할 수 있는데 역따옴표를 이용하면 줄 구분 없이 문자를 입력할 수 있다. 역따옴표 내에는 내장 표현식(embedded expressions)를 이용할 수 있는데 **${expressions}** 형태로 선언하면 된다.

- number

  Number 타입은 ES6 제안으로 10진수뿐만 아니라 16진수, 2진수, 8진수도 지원한다.

  ~~~typescript
  let decimal: number = 6; // 10진수
  let hex: number = 0xf00d; // 16진수
  let binary: number = 0b1010; // 2닌수
  let octal: number = 0o744; // 8진수
  ~~~

- boolean

  Boolean 타입에는 true, false 값을 할당할 수 있다.

- symbol (ECMA 2015 추가됨)

  symbol은 Symbol( ) 함수를 이용해 생성한 고유하고 수정 불가능한 데이터 타입으로 객체 속성의 식별자로 사용됩니다.

- enum

  enum은 number에서 확장된 타입으로 첫 번째 Enum 요소에는 숫자 0 값이 할당된다. 그다음 값은 특별히 초기화하지 않는 이상 1씩 증가한다.

  ~~~typescript
  enum WeekDay {Mon, Tue, Wed, Thu}
  let day: WeekDay = WeekDay.Mon
  ~~~

- 문자열 리터럴

  문자열 리터럴 타입은 string 타입의 확장 타입으로 사용자 정의 타입에 정의한 문자열만 할당할 수 있다.

  ~~~typescript
  type EventType = "keyup" | "mouseover";
  //type 키워드를 이용해 "keyup" 문자열 또는 "mouseover" 문자열만 허용하는 문자열 리터럴 타입을 정의
  ~~~



#### 객체(object) 타입

객체 타입은 속성을 포함하고 있으며, 호출 시그니처(call signature), 생성자 시그니처(construct signature) 등으로 구성

- Array

  배열 요소에 대응하며, 배열 안에 요소가 숫자 값이면 number[]가 타입이 된다.

  ~~~typescript
  let item:number[] = [1,2,3];
  ~~~

- Tuple

  배열 요소가 n개로 정해질 때 각 요소별로 타입을 지정하는 타입, 배열요소가 문자열과 숫자라면 [string, number]같은 형태로 타입 지정

  ~~~typescript
  let x:[string, number];
  x = ["tuple", 200];
  ~~~

- Function

  호출 시그니처에 포함되도록 정의된 타입 (6장에서 다룸)

- 생성자

  하나의 객체 (클래스로부터 생성)가 여러 생성자의 시그니처로 구성될 때 포함할 수 있는 타입, 생성자 타입 리터럴(constructor type literal) 사용하여 정의, 생성자 타입 리터럴은 생성자 시그니처를 구성하는 타입 매개변수, 매개변수 목록, 반환 타입으로 구성

  ~~~typescript
  new < 타입1,타입2, ... > (매개변수1, 매개변수2, ... ) => 타입
  ~~~

- Class,Interface

  객체 타입으로 분류되며, 객체지향 프로그래밍이나 구조 타이핑 등에 활용

### 기타 타입

그 밖에 타입스크립트에서는 다음과 같은 타입을 지원한다.

- Union

  2개 이상의 타입을 하나의 타입으로 정의한 타입

  ~~~typescript
  var x: string | number;
  ~~~

- Intersection

  두 타입을 합쳐 하나로 만들 수 있는 타입

  ~~~typescript
  interface Cat {leg: number};
  interface Dog {bone: number};
  let catDog : Cat & Dog = {leg: 4, bone: 6};
  //catDog 변수가 인터섹션 타입인 Cat & Dog로 선언돼 있으므로 할당 객체는 leg, bone 속성만 허용
  ~~~

- 특수 타입

  타입 계층도의 가장 아래쪽에 위치한 void, null, undefined 가 있다. void는 빈 값을 나타내는 타입이다. 함수에 반환값이 없을 때 void 타입을 선언할 수 있는데 undefined나 null 값을 받을 때 사용함

  ~~~typescript
  function say(): void {
  	console.log("Hi");
  }
  let un: void = undefined;
  ~~~

  Void 타입은 반환값이 없을 때 빈번하게 사용되지만 변수에 undefined 나 null 값을 할당하는 경우는 흔치 않으므로 변수에 void 타입을 사용하는 것은 유용하지 않다.

  Undefined, null 타입은 다른 모든 타입의 하위 타입(subtype)이며, undefined는 어떠한 빈 값으로도 초기화되지 않는 타입이다.
  하지만 undefined와 다르게 null 타입은 빈 객체로 초기화 된다.

**타입스크립트의 타입 계층도는 기존 자바스크립트의 타입을 확장한 형태** 비교했을때 추가된 타입은

- 객체 타입의 상위 타입으로 any추가
- Any 타입의 특수 타입으로 유니언 타입과 인터섹션 타입 추가
- 객체(Object) 타입의 하위 타입으로 Array, Interface, Tuple 추가
- Void 타입 추가

### symbol 타입

Symbol 타입은 내장 타입 중 하나이며 ES6에 추가되었다. 특징으로는 객체 속성(Object property)의 유일하게 불변적인 식별자로 사용된다.

~~~javascript
let helloSym = Symbol("helloSym");
// Symbol함수를 이용하여 선언한다.
~~~

Symbol 함수는 심벌 객체를 반환 Symbol 함수가 유일한 식별자를 생성하는 팩토리 함수의 역활을 한다. Symbol함수를 호출할때 "helloSym" 인수는 심벌의 설명(description)을 의미한다. **설명은 심벌에 접근할 때 사용할 수 있으며, 생략도 가능하다.**

만약 변수를 불변 상수로 선언하려면 const 제한자를 이용해 변수를 선언한다.

~~~javascript
const hello - Symbol();
~~~

hello 변수는 유일하면서 (Symbol() 함수 사용) 불변(const 사용)이라는 특징을 지닌다.

~~~javascript
let sym = Symbol("sym1");
let sym2 = Symbol("sym2");
console.log(sym === sym2); // false
console.log(sym, sym2); // Symbol(sym1),Symbol(sym2)
console.log(typeof sym2); // symbol
~~~

위 결과로 보듯 심벌 객체는 호출될 때마다 새심벌 객체를 만든다 즉 유일한 심벌 객체가 만들어진다. (sym, sym2는 서로 다른 심벌 객체) 그리고 심벌 객체는 symbol 타입이라는 별도의 타입을 지닌다. 심벌 객체는 객체 리터럴의 속성 키로 사용할 수 있다.

~~~javascript
const uni = Symbol();
let obj = {};
obj[uni] = 1234;
console.log(obj[uni]); // 1234
console.log(obj); // {[Symbol()]:1234}
~~~

obj의 출력 결과가 빈객체인 이유는 uni 심벌 키는 충돌을 피할 목적으로 생성했으므로 객체에서 심벌키는 무시돼서 출력되기 때문이다. Symbol() 함수의 반환값은 별다른 값을 취하지 않아도 그 자체로 식별자가 된다.

### enum 타입

ES6에 제안된 타입으로 컴파일 시간에 평가된다. 타입 계층도에서 number 타입의 하위 타입으로 자바스크립트 컴파일 된 후에는 개체 리터럴이나 배열처럼 객체 타입이 된다.(typeof 로 Object로 표시)

**명명된 숫자 상수(named numeric constants)의 집합을 정의할때 사용**

~~~typescript
// 형식
// enum Day {속성: 값, 속성: 값, 속성: 값, ... };
~~~

Day 는 바인딩 식별자, {} 자체는 enum 객체, enum 객체는 익명 객체 타입으로 (속성: 값)의 목록을 포함한다.
enum은 특별히 명시하지 않는 이상 순차적으로 인덱스가 증가합니다. **각 속성에는 초깃값을 할당할 수 있는데 기본적으로 숫자를 할당할 수 있다.**

~~~typescript
// const 제한자를 붙여 상수 enum을 선언
const enum WeekDay { Mon = 1, Thu = 2, Wed = 3, Thu = 4 }

//const 를 붙여 선언하면 읽기 전용으로 속성이나 인덱스 접근 표현식으로 속성값을 읽어야한다.
let day1 = WeekDay.Mon; // 속성으로 접근 가능
let day2 = WeekDay["The"]; // 인덱스 접근 표현식은 문자열 리터럴("Tue")을 사용해야함

//읽기 전용으로 설정하면 사용할 수 없는 상황이 있다.
let day3 = WeekDay[WeekDay.Thu]; // 오류 - "Tue" 같은 문자열 리터럴만 허용
let day4 = WeekDay; // 오류 - enum 식별자를 변수에 직접 할당할 수 없다.

// *타입스크립트 2.4부터 문자열 enum이 추가되어 초깃값으로 문자열도 할당*
enum WeekDay3 {
  Mon = "Monday",
  Tue = "Tuesday"
}
// 초깃값으로 하나의 속성을 설정했다면 다른 속성의 초깃값도 같아야 한다. (연산식은 할당할 수 없다.)
~~~