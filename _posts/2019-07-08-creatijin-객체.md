---
layout: post
title: "Basic - Object"
author: Creatijin
category: [javascript]
thumbnail: /assets/img/posts/javascript-posting-object.jpg
date: "2019-07-08T03:01:00.000Z"
summary: 자바스크립트 객체
---

# 객체

자바스크립트에서 단순한 데이터 타입은 **(숫자, 문자열, 블리언(true/false), null, undefined)**가 있다.

이들을 제외한 다른값은 **모두 객체**이다. 하지만 이들은 값이 정해지면 **변경**할 수 없다.

자바스크립트의 객체는 **변형** 가능한 속성들의 집합이다. 

자바스크립트에서는 배열, 함수, 정규 표현식, 객체 등 모두 객체이다.



객체는 이름과 값이 있는 속성들을 포함하는 컨테이너 라고 할 수 있다.

속성의 이름은 문자열이면 모두 가능하다. **(빈 문자열도 포함)**

속성의 값은 undefined를 제외한 자바스크립트의 모든 값이 사용 될 수 있다.



자바스크립트의 객체는 클래스가 필요 없고, 새로운 속성의 이름이나 값이 어떠한 제약 사항이 없다.

객체는 데이터를 한 곳에 모으고 구조화 하는데 유용하다.

객체 하나는 다른 객체를 포함할 수 있기 때문에 그래프나 트리 같은 자료구조를 쉽게 표현할 수 있다.



객체 하나에 있는 속성들을 다른 객체에 상속하게 해주는 프로토타입(prototype) 연결 특성이 있다.

**프로토타입 체이닝**

이 특성을 잘 활용하면, 객체를 초기화하는 시간과 메모리 사용을 줄일 수 있다.



### 01)객체 리터럴

새로운 객체를 생성할때 매우 편리한 표기법을 제공

~~~~javascript
var foo = {   } ;

var foo2 = {
	"first-name" : "jo",
	"last-name" :  "seungjin"
}
~~~~



속성(property)의 이름은 어떤 문자열이라도 가능하다. **(빈 문자열도 포함)**

속성 이름에 사용한 따옴표는 속성 이름이 자바스크립트에서 사용할 수 있는 유효한 이름이고, 예약어가 아닐 경우에는 생략할 수 있다.

쉼표는 , "속성 이름" : "값" 쌍 들을 구분하는데 사용한다.

속성의 값은 어떠한 표현식도 가능하다. 여기에는 객체 리터럴도 가능하다.

~~~~javascript
Var flight = {

	airline : "Oceanic",
	number : 815,

	departure : {
		IATA : "SYD",
		time : "2004-09-22 14:55",
		city : "Sydney",
	},

	arrival : {
		IATA : "LAX",
		time : "2004-09-23 10:42",
		city : "Los angeles"
	}
};
~~~~



### 02)속성값 읽기

객체에 속한 속성의 값은 속성 이름을 대괄호 [ ] 로 둘러싼 형태로 읽을 수 있다.

속성 이름이 유효한 자바스크립트 이름이고 예약어가 아닐 경우에는 마침표(.) 표기법을 대신 사용할 수 있다.

마침표 표기법은 보다 간단하고 읽기가 편하기 때문에 보통 더 선호한다.

~~~javascript
foo2["first-name"] //"jo"
flight.departure.IATA //"SYD" 마침표 표기법
~~~

**객체에 존재하지 않는 속성을 읽으려고 하면 undefined**

~~~javascript
foo2["middle-name"] //undefined
~~~



||연산자를 사용하여 다음과 같이 기본값을 지정할 수 있다.

~~~javascript
var middle = stooga["middle-name"] || "(none)";
var status = flight.status || "unknown";
~~~

존재하지 않는 속성, 즉 undefined의 속성을 참조하려 할 때 typeError 예외가 발생합니다.

이런 상황을 방지하기 위해서 다음과 같이 && 연산자를 사용할 수 있다.

~~~javascript
flight.equipment 								//undefined
flight.equipment.model 							//throw "TypeError"
flight.equipment && flight.equipment.model		//undefined
~~~



### 03) 속성값의 갱신

객체의 값은 할당에 의해 갱신합니다. 

만약 할당하는 표현식에서 속성 이름이 이미 객체 안에 존재하면 해당 속성의 값만 교체합니다.

~~~javascript
stooge['first-name'] = 'Jerome'
~~~

이와 반대로 속성이 객체 내에 존재하지 않는 경우에는 해당 속상을 객체에 추가한다.

~~~Javascript
stooge['middle-name'] = 'Lester';
stooge.nickname = 'Curly';
flight.equipment = {
    model : 'Boeing 777'
};
flight.status = 'overdue';
~~~



### 04)참조

객체는 참조 방식으로 전달  —  **결코 복사되지 않는다.**

~~~Javascript
var x = stooge;
x.nickname = 'Curly';
var nick = stooge.nickname;
// x와 stooge가 모두 같은 객체를 참조하기 때문에,
//변수 nick의 값은 'Curly'

var a = {}, b = {}, c = {};
// a,b,c는 각각 다른 빈 객체를 참조

a = b = c = {};
//a, b, c 는 모두 같은 빈 객체를 참조
~~~



### 5)프로토타입(prototype)

**모든 객체는 속성을 상속하는 프로토타입 객체에 연결돼 있다.**

* 객체 리터럴로 생성되는 모든 객체는 자바스크립트 표준 객체인 Object의 속성인  Prototype(Object Prototype) 객체에 연결 된다.

객체를 생성할 때는 해당 객체의 프로토타입이 될 객체를 선택할 수 있다.

create는 넘겨 받은 객체를 프로토타입으로 하는 새로운 객체를 생성하는 메소드이다.

~~~~javascript
if (typeof Object.create !== 'function') {
    Object.create = function (o) {
        var F = function () {};
      	F.prototype = o
      	return new F();
    }
}
var another_stooge = Object.create(stooge);
~~~~

프로토타입 연결은 **값의 갱신에 영향을 받지 않는다.**

즉, 객체를 변경하더라도 객체의 프로토타입에는 영향을 미치지 않는다.

~~~Javascript
another_stooge["first-name"] = 'Harry';
another_stooge['middle-name'] = 'Moses';
another_stooge.nickname = 'moe';
~~~

프로토타입 연결은 오로지 객체의 속성을 읽을 때만 사용합니다.

객체에 있는 특정 속성의 값을 읽으려고 하는데 해당 속성이 객체에 없는 경우 자바스크립트는 

이 속성을 프로토타입 객체에서 찾으려고 한다.

이러한 시도는 프로토타입 체인(prototype chain)의 가장 마지막에 있는 Object.prototype까지 계속해서 이어진다.

**존재하지 않는 경우 undefined 값을 반환한다.** 

이러한 동작을 **위임(delegation)**



프로토타입 관계는 동적 관계다.

프로토타입에 새로윤 속성이 추가되면, 해당 프로토타입을 근간으로 하는 객체들에는 즉각적으로 속성이 나타남



### 06)리플렉션(reflection)

객체에 어떤 속성들이 있는지는 특정 속성을 접근해서 반환하는 값을 보면 쉽게 알 수 있다.

이때 typeofㅇ연산자는 속성의 타입을 살펴보는데 매우 유용

~~~javascript
typeof flight.number						//'number'
typeof flight.status						//'string'
typeof flight.arrival						//'arrival'
typeof flight.manifest						//'undefined'
~~~

떄때로 해당 객체의 속성이 아니라 프로토타입 체인 상에 있는 속성을 반환할 수 있기 때문에 주의할 필요가 있다.



~~~Javascript
typeof flight.toString 		//'function'
typeof flight.constructor   //'function'
~~~

리플렉션을 할 때 원하지 않는 속성을 배제하기 위한 두가지 방법

1. 함수값을 배제하는 방법

   - 일반적으로 리플렉션을 할 때는 데이터에 관심이 있기 때문에 함수가 반환되는 경우를 염두에 두고 있다가 배제시키면 원하지 않는 속성을 배제할 수 있다.

     ​

2. 갹체에 특정 속성이 있는지를 확인하여 true/false 값을 반환하는 메소드를 사용(hasOwnProperty)

   - hasOwnProperty메소드는 프로토타입 체인을 바라보지 않는다.

     ~~~javascript
     flight.hasOwnProperty('number');		//true
     Flight.hasOwnproperty('constructor')	//false
     ~~~



### 07)열거(Enumeration)

for in 구문을 사용하면 객체에 있는 모든 속성의 이름을 열거 할 수 있다.

함수나 프로토타입에 있는 속성 등 모든 속성이 포함 되기 때문에 원하지 않는 것들을 걸러내기 위해 열거가 필요하다.

**일반적인 필터링 방법**

~~~Javascript
var name;
for (name in another_stooge ) {
  if ( typeof another_stooge[name] !== 'function' ){
    document.writeln( name + " : " + another_stooge[name]);
  }
}
~~~

- hasOwnProeprty 메소드와 함수를 배제하기 위한 typeof를 사용한다.



for in 구문을 사용하면 속성들이 이름순으로 나온다는 보장이 없다.

그러므로 만약 특정 순으로 속성 이름들이 열거되기를 원한다면 for in 구문을 사용하지말고,

속성이 열거되기 원하는 순서를 특정 배열로 지정하고 이 배열을 이용하여 객체의 속성을 열거 할 수 있다.

~~~javascript
var properties = [
  'first-name',
  'middle-name',
  'last-name'
]

for(var i = 0; i < properties.length; i++) {
    document.writeln(properties[i] + ':' + another_stooge[properties[i]]);
}
~~~

이렇게 하면 프로토타입 체인에 있는 속성들이 나오지 않을까 염려할 필요도 없으며 원하는 순서대로 속성들을 리플렉션 할 수 있다.



### 08)삭제

**delete 연산자를 사용하면 객체의 속성을 삭제할 수 있다.**

delete 연산자는 해당 속성이 객체에 있을 경우에 삭제를 하며 프로토타입 연결 상에 있는 객체들은 접근하지 않는다.



객체에서 특정 속성을 삭제했는데 같은 이름의 속성이 프로토타입 체인에 있는 경우 프로토타입의 속성이 나타난다.

~~~javascript
another_stooge.nickname				//'Moe'

//another_stooge에서 nickname을 제거하면 프로토타입에 있는 nickname이 나타남

delete another_stooge.nickname;

another_stooge.nickname				//'Curly'
~~~



### 09)최소한의 전역변수 사용

**자바스크립트는 전역변수(Global)사용이 매우 쉽다.**

(프로그램 유연성을 약화시키기 때문에 가능한 피하는것이 좋다.)



**방법**

- 애플리케이션에서 전역변수 사용을 위해 다음과 같이 전역변수 하나를 만든다.

  ~~~Javascript
  var MYAPP = {};
  //이제 이 변수를 다른 전역변수를 위한 컨테이너로 사용

  MYAPP.stooge = {
      'first-name': 'Jo',
    	'last-name' : 'seungjin'
  };

  MYAPP.flight = {
    	airline : 'Oceanic',
    	number : 815,
    	departure : {
          IATA : 'SYD'
      },
    	arrival : {
          IATA : 'LAX'
      }
  };
  ~~~

  이러한 방법으로 애플리케이션에 필요한 전역변수를 이름 하나로 관리하면 다른 애플리케이션이나 위젯 또는 라이브러리들과 연동할 때 발생하는 문제점을 최소화할 수 있다.

  MYAPP.stooge가 명시적으로 전역변수라는 것을 나타내기 때문에 프로그램 가독성이 높아진다.



- 클로저 또한 전역변수의 사용을 줄이는 효과적인 방법 중 하나다.

