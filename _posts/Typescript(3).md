---
title: Typescript(3)
layout: post
author: Creatijin
category: [Typescript]
thumbnail: "/assets/img/posts/typescript_Logo.png"
date: '2020-07-31 08:00:00'
summary: 타입스크립트의 제어문
---


# Typescript(3)

## 타입스크립트 제어문



#### if문 사용시 타입 제약

if문에 사용할 변수도 다음과 같이 명시적으로 불리언 타입으로 선언해야 한다.

~~~typescript
let isBoolean = true;

// 타입스크립트는 이중 if문을 중첩할 경우 괄호 {}를 생략할 수 있다.
if (2 > 1)
  if (1 < 2) {
    
  }

// if 문의 조건으로 사용가능한 타입
let txt: string = "";
let num: number = 0;
let bool: boolean = true;

//1번 if 문
if (num || txt) {
	console.log("123");
}

//2번 if문
if (bool && 2 > 1) {
    console.log("2");
}

//결과 - 2
~~~

if문의 동작 원리는 javascript와 같다. 1번의 경우 2개의 조건이며 number에서 0은 false이고 문자열의 경우도 빈 값은 false이다. 둘다 false이기 때문에 1번 if문은 실행 되지 않는다. 2번 if문은 ture/false가 명확한 boolean 타입으로 지정한 변수가 조건이기 때문에 직관적이다. 

#### switch 문과 폴스루

~~~typescript
let com: number = 0;

switch (com) {
  case 0:
    // 명령문
    break
  case 1:
    // 명령문
    break
}
~~~

switch 문의 조건은 number 타입이므로 case절의 값은 모두 number 타입이여야 한다. 만약 타입을 정하지 않았다면 타입을 제한하지 않을 수 있다.

~~~typescript
let com2: any = "hello";

switch (com2) {
  case "hello":
    // 명령문
    break
  case 2:
    // 명령문
    break
}
~~~

조건 타입이 any기 때문에 case 절의 값은 여러 타입을 사용할 수 있다. **switch 문을 사용할 때 case 절에 사용할 타입이 고정적이라면 반드시 입력 변수에 타입을 선언해야 한다 그렇지 않다면 any 타입을 지정해야 한다.**

##### 폴스루 이해와 폴스루의 사용 여부 설정

case절에서는 break 문을 사용해 switch 문을 벗어난다. 그런데 상황에 따라서 break 문을 생략할 수 있다. 만약 **break 문을 생략하면 다음 case절이 실행**된다. 이런 상태를 **폴스루(full-through)**라고 한다. 

~~~typescript
let input = 2;

switch (input % 2) {
  case 0:
  	console.log("숫자 0"); // <- full-throughs 발생
  case 1:
    console.log("숫자 1");
    break;
}
// 숫자 0, 숫자 1
~~~

폴스루를 개발자가 고의로 발생시키는 경우가 있다. case 절을 처리할 때 여러 case절에서 처리하는 로직이 같은 경우, 편의상 break 문을 생략하는 것입니다. 반대로 개발자의 실수로 break 문을 생략해서 폴스루가 발생할 수 있다. 발생 원인이 무엇이든 타입스크립트는 폴스루의 사용 여부를 설정할 수 있다. Tsc 컴파일러 옵션인 **"noFallthroughCasesInSwitch"** 를 수정하면 된다. 

~~~json
//폴스루를 허용하지 않을경우 tsconfig.json에 설정
{
  "compilerOption": {
    "noFallthroughCasesInSwitch": true
  }
}
// 기본은 false이며 허용할 경우 false 지정하거나 옵션을 추가하지 않고 생략하면 된다.
// 옵션을 true로 설정하더라도 case절에 아무런 명령문도 없으면 폴스루를 허용한다.
~~~

