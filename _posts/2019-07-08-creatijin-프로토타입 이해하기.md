---
layout: post
title: "Basic - prototype"
author: Creatijin
category: ["javascript"]
thumbnail: /assets/img/posts/javascript-posting-prototype.jpg
date: "2019-07-08T03:05:00.000Z"
summary: 자바스크립트 프로토타입
---
# 프로토타입 이해하기

자바스크립트느 프로토타입 기반언어


~~~javascript
function Person() {
    this.eyes = 2;
  	this.nose = 1;
}

var Seungjin = new person();
var jo = new Person();


console.log(Seungjin.eyes); //2
console.log(jo.eyes); //1

console.log(Seungjin.nose); //2
console.log(jo.nose); //1
~~~

위와 같이 seung, ho에 eyes, nose를 공통적으로 가지고 있지만, 메모리는 4개를 할당하게 된다.

~~~javascript
function Person() {}
Person.prototype.eyes = 2;
Person.prototype.nose = 1;

var kim  = new Person();
var park = new Person():

console.log(kim.eyes); // => 2
...
~~~

프로토타입은 이러한 문제를 해결할 수 있다.



## Prototype Link와 Prototype Object

자바스크립트에는 Prototype Link와 Prototype Object 가 존재한다.

이 둘을 **Prototype**이라고 부른다.



### Prototype Object

모든 객체(Object)의 조상은 함수(Function)이다

~~~javascript
function Person() {} // => 함수

var personObj = new Person(); // => 함수로 객체를 생성
~~~

personObj 객체는 Person함수로 부터 파생된 객체이다.

많이 쓰이는 객체생성도 이와 같다

~~~Javascript
var obj = {} 

var obj = new Object();  // 각 obj는 같은 의미를 뜻한다
~~~

위 코드에서 **Object**가 자바스크립트에서 기본적으로 제공하는 함수

**Object**와 마찬가지로 Function, Array도 모두 함수로 정의되어 있다.

함수를 정의될때 2가지 일이 동시에 이루어진다.

1. 해당 함수에 Constructor(생산자) 자격 부여

   **Constructor 자격**이 부여되면 new를 통해 객체를 만들어 낼 수 있게 된다.

   이것이 함수만 new 키워드를 사용할 수 있는 이유다.

   ​

2. 해당 함수의 Prototype Object 생성 및 연결

   함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성이 된다.



그리고 생성된 함수는 Prototype 이라는 속성을 통해 Prototype Object에 접근할 수 있다. Prototy Object는 일반적인 객체와 같으며 기본적인 속성으로 constructor과 \__proto__를 가지고 있다.



constructor은 Prototype Object와 같은 생성되었던 함수를 가르킨다.

\__proto__는 Prototype Link다.





### Prototype Link

Prototype 속성은 함수만 가지고 있던 것과는 달리 (Person.prototype)

\__proto__속성은 모든 객체가 빠짐없이 가지고 있는 속성이다.

**\__proto__는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가르킨다.**



객체가 속성을 직접 가지고 있지 않기 때문에 속성을 찾을때 까지 상위 프로토타입을 탐색한다.

최상위인 Object의 Prototype Object까지 도달했는데도 못찾았을 경우 undefined를 리턴한다.

\__proto__속성을 통해 상위 프로토타입과 연결되어있는 형태를 프로토타입 체인(chain)이라고한다.



프로토타입 체인 구조때문에 모든 객체는 Object의 자식이라고 불리고, Object Prototype Object에 있는 모든 속성을 사용할 수 있다.

~~~Javascript
Object.prototype
{__defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, __lookupSetter__: ƒ, …}
constructor:ƒ Object()
hasOwnProperty:ƒ hasOwnProperty()
isPrototypeOf:ƒ isPrototypeOf()
propertyIsEnumerable:ƒ propertyIsEnumerable()
toLocaleString:ƒ toLocaleString()
toString:ƒ toString()
arguments:null
caller:null
length:0
name:"toString"
__proto__:ƒ ()
valueOf:ƒ valueOf()
__defineGetter__:ƒ __defineGetter__()
__defineSetter__:ƒ __defineSetter__()
__lookupGetter__:ƒ __lookupGetter__()
__lookupSetter__:ƒ __lookupSetter__()
get __proto__:ƒ __proto__()
set __proto__:ƒ __proto__()

jin.toString();
~~~





