---
title: Typescript(2)
layout: post
author: Creatijin
category: [Typescript]
thumbnail: "/assets/img/posts/typescript_Logo.png"
date: '2020-07-25 01:00:00'
summary: 타입스크립트 내장 타입
---

# Typescript(2)

## 타입스크립트 내장 타입

##any타입

Any 타입은 제약이 없는 타입이다. 어떤 타입의 값도 받아들일 수 있다. any 타입은 최소한의 타입 검사만 수행한다. (자바스크립트의 최소한의 정적 타입 검사를 수행하는 것처럼)
any는 확정된 타입이 아니기 때문에 어떠한 값이든 할당 가능하다. 이러한 점 때문에 유연한 대처가 가능하며 배열의 타입으로 사용하면 어떤 타입의 요소도 받아들일 수 있다.

~~~typescript
let fruit:any[] = ['Banana', 20, false];
console.log(fruit[0]); // 'Banana'
~~~

배열 값의 타입이 다양해서 한 가지 타입으로 고정할 수 없을 때 사용할 수 있다.

### 타입을 이용해 컴파일 시간에 타입 검사 생략

any 타입은 최상위 타입이기에 자바스크립트의 모든 값을 할당받을 수 있다. 자바스크립트 변수 선언과 동일한 동작을 수행한다. ( var a; <= a: any 와 동일 )

하지만 any를 선언한것과 안한것은 차이가 있다.
선언하면 명시적으로 타입을 선언한 것이고 선언하지 않으면 타입이 없는 것이다.
명시적으로 타입을 선언한 변수는 최소한의 정적 타입 검사(static type checking)를 수행한다.

any 타입을 선언하면 **선언돼 있지 않은 속성에 접근하더라도 컴파일 오류가 발생하지 않는다.**

~~~typescript
let number = 20;
let number2: any = 20;

number.toFixed(1); //20.0
number2.toFixed(1); //20.0
//선언된적 없는 임의 메소드 notMethod
number.notMethod(); // 컴파일 오류
number2.notMethod(); // 오류가 발생하지 않는다.
~~~



### any 타입과 유사하지만 다른 동작방식 object 타입

Object 타입은 타입 구분 없이 값을 할당할 수 있는 특성이 있어 any 타입과 비슷하다. 그런데 속성 유무 검사 시점이 다르다.

- Any 타입 - 속성 유무를 런타임 시에 검사

  ~~~typescript
  let num3: any = 30;
  console.log(typeof num3, num3.toFixed(2));
  // number 30.00
  ~~~

  Any 타입으로 선언했기 때문에 컴파일과 실행에 오류가 없다. 

- object 타입 - 컴파일 시간에 속성 유무 검사

  - Object 타입은 변수에 숫자를 할당하더라도 컴파일 시에 숫자 메서드를 인식하지 못해 컴파일 할때 에러가 발생한다.

    ~~~typescript
    let num2: Object = 50;
    num2.toFixed(1); 
    // 컴파일 에러 property 'toFixed' does not exist on type 'Object'
    ~~~

    - object 타입은 컴파일 과정에서만 유효한 타임이며, 실제 자바스크립트로 컴파일된 뒤에는 타입이 사라진다. 컴파일 후에는 타입을 지정하지 않은 것과 같다.



### noImplicitAny 옵션

매개변수에 타입을 생략하면 암시적으로 any 타입이지만, 타입을 생략했기 때문에 도무지 무슨 타입의 값을 전달해야 할지 개발자가 코드를 보며 예측할 수 없다. (타입스크립트 2.1 버전부터는 any 타입에 대한 추론 능력이 강화되었고 명시적으로 any 타입을 선언하지 않아도 암시적으로 any 타입이다.)

~~~typescript
let c; // 암시적 any 타입
let d = []; // 암시적 any[] 타입
~~~

명시적으로 any 타입이라고 하나 선언을 강제할 필요성이 있다. any 타입의 사용을 강제하려면 컴파일러 옵션 중 nolmplicitAny를 true로 설정하면 된다. **(tsconfig.json 파일에서 설정 기본값은 false)**

~~~typescript
//tsconfig.json 파일 설정
{
  "compilerOptions": {
    "noImplicitAny": true // false 기본값
  }
}
// 명령어
$ tsc --noImplicitAny true

// 매개변수에 any타입 선언하지 않을 경우 컴파일 오류 발생
// 아래와 같이 매개변수에 타입 선언
function add(a:any, b:any) => {}
~~~

#### 배열 타입과 제네릭 배열 타입

배열은 여러 개의 값을 하나의 변수에 담아 관리하는 자료구조이다. 타입스크립트는 두 가지 배열 형태를 지니고 있다.

- 배열 타입(array type)
  - 요소 타입으로 string, number, boolean과 같은 내장 타입뿐 아니라 클래스, 인터페이스도 가능하다.
  - 배열 요소의 타입이 정해져 있지 않다면 any 타입으로 지정할 수 있다.
  - 타입이 느슨하므로 타입을 제약하려면 유니언 타입을 이용해 선언한다.
  
- 제네릭 배열 타입 (generic array type)

  - Array<T> 형태로 선언한다. (<T>는 타입을 의미한다)
  - 타입을 숫자나 문자열 타입으로 제약하려면 유니언 타입으로 선언한다,
  - 타입을 참조할떄는 타입 쿼리를 사용 가능하다.
  - 내장 타입 외에 객체 타입도 받을 수 있다.

  ~~~typescript
  // Array<T>형태
  let number:Array<number> = [1,2,3];
  let numString:Array<number | string> = [1, "string"];
  // 타입 쿼리로 numString 변수의 타입을 참조
  let num2: typeof numString = [2, "number"];
  // 객체 타입
  let nums: Array< () => string > = [ () => "one", () => "two"  ];
  console.log(nums[0]) // "one"
  ~~~


**타입스크립트에서 선언된 배열타입, 제네릭 배열 타입은 컴파일 시 타입 검사를위해 필요하며, 컴파일 후(ES5)에는 타입이 제거된 배열만 남는다.**



####  튜플 타입(tuple type)

n개의 요소로 이뤄진 배열에 대응하는 타입, 튜플은 배열과 비슷하지만, 배열은 배열 요소의 개수에 제한이 없고 string[ ]처럼 특정 타입으로 배열 요소의 타입을 강제할 수 있다. 하지만 튜플의 경우 배열 요소에 대응하는 n개에 대한 타입이다.

~~~typescript
// 배열 요소에 대응 하는 n개의 타입
let y : [string, number] = ["tuple", 200];
// string => "tuple", number => 200
~~~

**튜플에 선언된 타입 수와 할당될 배열의 요소 수가 정확히 일치돼야 할당이 가능해진다.**
타입스크립트 2.7 이후부터 튜플 타입에 따라 할당 배열의 요소 수가 고정되었다. 그전 버전에서는 개수를 초과하면 유니언 타입으로 적용받았다.



#### void, null, undefined

- void - 함수의 반환값이 없을 때 지정하는 타입, null이나 undefined만 할당할 수 있다.(void 타입이 null과 undefined의 상위 타입이기 때문)

~~~typescript
function world(): void {
  console.log("Hello, world");
}
// 반환값이 없는 함수
let myWorld = world();
// myWorld 변수는 void 타입으로 지정, void, undefined를 할당 할 수 있음
// 반환 타입은 void 지만 반환값이 없으므로 undefined가 할당됨
console.log(myWorld, typeof myWorld);
// undefined, 'undefined'
// void 타입은 null 또는 undefined만 할당할 수 있기 때문에 유영한 타입은 아니다.

let empty;
// empty에 할당된 값이 없기 때문에 undefined가 된다.
~~~

**변수를 선언할 때 값을 할당하지 않았음을 나타내기 위해 선언한 변수에 null을 할당하는 것은 권장하지 않는다.**
Null과 undefined는 불필요한 선언이 되거나 초기화하지 않았을 때 불안정한 연산을 초래할 수 있다.
그래서 컴파일러 옵션중에 사용하지 못하게 막는것이 가능하다.

~~~typescript
//tsconfig.json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
// 기본적으로 변수에 할당되던 null과 undefined는 더 이상 할당되지 못하고 컴파일 오류가 발생한다.
~~~

