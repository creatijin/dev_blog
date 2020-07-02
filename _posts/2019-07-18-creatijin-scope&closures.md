---
layout: post
title: 스코프와 클로저
author: Creatijin
category: [javascript]
thumbnail: /assets/img/posts/javascript-posting1-bg.jpg
date: "2019-07-18T01:47:00.000Z"
summary: 스코프와 클로저
---


# 스코프와 클로저

자바스크립트에서 스코프와 클로저는 빠질 수 없는 부분이다.(물론 어떤것도 뺄 순 없지만…) 다른 언어를 접하던 프로그래머들이 다르게 작동하는 자바스크립트의 스코프와 접하기 어려운 클로저 때문에 고생이다.

자바스크립트에서 클로저는 상당히 어려운 부분에 속한다 하지만 이해만하고 익숙해진다면 객체지향 언어보다 유연함을 느낄 수 있을것이다.



## 스코프(scope)

스코프란 현재 접근할 수 있는 변수들의 범위를 뜻한다 어떠한 변수가 스코프 안에 선언되었으면 해당 스코프 안에서는 변수에 접근해서 읽거나 쓸 수 있고, 스코프 밖에서는 해당 변수에 접근할 수 없다.

스코프에는 크게 두 종류의 종류가 있다.

- 전역 스코프(global scope)
- 지역 스코프(local scope)



### 전역 스코프 (global scope)

전역 스코프는 변수가 함수 바깥이나 중괄호 **{}** 바깥에 선언되었다면, 전역 스코프에 정의 된다.

#### 1) 전역 변수

전역 변수를 선언하면 코드 모든 곳에서 해당 변수를 사용할 수 있다 함수에서도 가능하다.

~~~javascript
//1) 전역변수
var globalVar = "global";

function hello () {
  console.log(globalVar);
}

console.log(globalVar);
hello();

//global
//global
~~~



#### 2) const,let

~~~javascript
let globalLet = "globar";
let globalLet = "globarLet";

/*
에러
Uncaught SyntaxError: Identifier 'globalLet' has already been declared
*/

var globalVar = "globarVar";
var globalVar = "globarVariable";

console.log(globalVar);
// globarVariable
~~~

왠만하면 전역 스코프에 변수를 선언 안하는것이 좋으며, **const, let**을 사용하여 두개 이상의 변수의 이름이 같은 경우 이름 충돌이 발생하여 에러가 발생된다 만약 **var** 의 경우 두개 이상의 변수를 선언할 경우 두번째 변수가 첫번째 변수를 덮어쓰게 된다. 이러한 문제는 프로그래머들에게 디버깅의 어려움을 발생 시킨다.

##### 언제나 전역 변수보다는 지역 변수로 변수를 선언해야 한다.

**const,let,var은 다음에 더 자세히 다루도록 하자**



### 지역 스코프 (local scope)

코드의 특정 부분에만 사용이 가능한 변수이며 자바스크립트에서는 두 가지의 지역 스코프가 있다.

- **함수 스코프**
  - 함수 내부에서 변수를 선언한다면, 그 변수는 선언한 함수 내부에서만 사용이 가능하다.

~~~javascript
function localFunc () {
  var localF = "Hello local scope";
  console.log(localF);
}
localFunc();
console.log(localF);
//"Hello local scope"
//에러 - Uncaught ReferenceError: localF is not defined
~~~



- **블록 스코프**

  - 중괄호 **{}** 내부에서 **const,let** 으로 변수를 선언하면, 그 변수들은 중괄호 블록 내부에서만 접근이 가능하다.
  - 함수를 선언 할 때 중괄호를 사용해야 하므로 블록 스코프는 함수 스코프의 서브셋(subset)이다.**(화살표 함수를 사용하여 반환하는 경우는 제외)**

  ~~~javascript
  {
    const blockHello = "block scope Hello"
    console.log(blockHello);
  }
  console.log(blockHello);
  //"block scope Hello"
  //Uncaught ReferenceError: blockHello is not defined
  ~~~

####함수 호이스팅(Function hoisting)

함수는 **함수 선언식(Function declaration)**으로 선언될 경우, 현재 스코프의 **최상단**으로 호이스팅(hoisting) 된다.

~~~javascript
//호이스팅으로 같은 결과를 볼 수 있다.

hoistingFunc();
function hoistingFunc () {
  console.log('function hoisting');
}

function hoistingFunc () {
  console.log('function hoisting');
}
hoistingFunc();
//'function hoisting'
//'function hoisting'
~~~



하지만 **함수 표현식**으로 선언할 경우, 함수는 스코프의 **최상단으로 호이스팅 되지 않는다.**

~~~javascript
hoistingFunc();
var hoistingFunc = function () {
  console.log('function hoisting');
}
//Uncaught SyntaxError: Identifier 'hoistingFunc' has already been declaredat
~~~



두 방식이 결과가 다르기 때문에 **함수 호이스팅은 사용하면 안된다** 이유는 함수 호이스팅은 프로그래머에게 혼란을 줄 수 있기 때문이다. **함수를 호출 하기전에 선언해 두어야한다.**



####함수는 서로의 스코프에 접근할 수 없다

함수들이 각각 선언 되었을 때, 서로의 스코프에는 접근할 수 없다. 어떤 함수가 다른 함수에서 사용더라도 불가하다.

~~~javascript
function firstFunc () {
  var firstVar = "First Variable";
}

function secondFunc () {
  firstFunc();
  console.log(firstVar)
}
secondFunc();

//Uncaught ReferenceError: firstVar is not definedat secondFunc
~~~

**secondFunc** 함수는 **firstFunc** 함수안에 변수 firstVar에 접근 할 수 없다.



#### 렉시컬 스코프

렉시컬 스코핑은 함수가 다른 함수 내부에서 정의 된다면, **내부 함수는 외부 함수의 변수에 접근**할 수 있다

~~~javascript
function lexicalFunc () {
  var lexicalVar = "렉시컬 스코핑 outer";
  
  function lexicalInner () {
    var lexicalin = "렉시컬 스코핑 inner";
    console.log(lexicalVar);
  }
  lexicalInner();
  console.log(lexicalin);
}
lexicalFunc();
//렉시컬 스코핑 outer
//Uncaught ReferenceError: lexicalin is not definedat lexicalFunc
~~~

**위 결과 처럼 외부 함수가 내부 함수의 변수에는 접근 할 수 없다**



스코프의 동작을 예를 들자면 영화에 나오는 경찰들의 취조실을 보면 이해가 쉽다. 취조하는 방을 안쪽에서는 볼 수 있지만 취조하는 방에서는 안쪽을 볼 수 없는 구조이다.



## 클로저(Closures)

클로저란 ~~함수 내부에 함수를 작성할 때 마다 클로저를 생성하는 것이다 내부에 작성된 함수가~~[오류로 인한 수정] 외부 함수에서 내부 함수로는 닫혀 있지만, 내부 함수에서 외부 함수로는 열려있는 구조를 바로 클로저라 한다. 클로저는 외부 함수의 변수를 사용할 수 있기 때문에 대개 반환하여 사용한다.

#####특정 함수가 참조하는 변수들이 선언된 렉시컬 스코프는 계속 유지되는데, 그 함수와 스코프를 묶어서 클로저라고 한다. (더 쉽게 말하면 이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 함수)

~~~javascript
function outer () {
	var counter = 0;
  return function inner () {
    return ++counter;
  };
}
var increase = outer();

console.log(increase());
console.log(increase());

// 1
// 2
~~~

**가장 기본적인 환경은 스코프 안에 스코프가 있을때, 즉 function 안에 function이 선언되었을 때이다.** 

클로저는 외부 함수의 변수에 접근할 수 있기 때문에 일반적으로 두가지 목적을 위해 사용한다.

1. 사이드 이펙트 제어
2. Private 변수 생성



###사이드 이펙트(side effects) 제어

함수에서 값을 반환할 때를 제외하고 무언가를 행할 때 사이드 이펙트[^1]가 발생한다. 여러가지 있지만 예를 들면 Ajax 요청, timeout을 생성할 때, console.log를 선언할때 조차 사이드 이펙트가 발생한다.

[^1]: '부작용'이라고 해석할 수 있지만 프로그래밍에서는 '실행 중에 어떤 객체를 접근해서 변화가 일어나는 행위(라이브러리 I/O, 객체 변경 등)' 라고 정의할 수 있다.

보통 Ajax나 timeout과 같이 코드 흐름을 방해하는 것을 위해 클로저를 활용하여 사이드 이펙트를 제어한다.

예를 들어 친구들에게 파스타를 만들어준다고 생각해보자

~~~javascript
function makePasta () {
	setTimeout(function(){
		console.log('파스타를 만들자!');
	},1000)
}
makePasta();
//파스타를 만들자!
~~~

위 함수를 실행하면 1초 뒤 '파스타를 만들자'를 볼 수 있다.

하지만 사람마다 좋아하는 파스타가 다르기 때문에 어떤 파스타를 좋아하는지 알 수 있게 변경해보자

~~~javascript
function makePasta (flavor) {
	setTimeout(function(){
		console.log((flavor) + ' 파스타가 좋아!');
	},1000)
}
makePasta('미트볼');
//미트볼 파스타가 좋아!
~~~

하지만 위 코드와 같이 진행 한다면 맛을 알자마자 파스타를 만들어야 한다. 요리하는 사람이 원하는 시점에 파스타를 만들 수 있어야한다.

~~~javascript
function preparePasta (flavor) {
  return function () {
    setTimeout(function(){
			console.log((flavor) + ' 파스타를 만들자!');
		},1000)
  }
}
var makepasta = preparePasta('봉골레');
makepasta();
//봉골레 파스타를 만들자!
~~~

위 코드와 같이 클로저를 사용하여 사이드 이펙트를 줄일 수 있다. 원할 때 내부 클로저를 호출할 수 있는 함수를 만들었다.



### Private 변수 와 클로저

함수 내의 변수는 함수 바깥에서 접근할 수 없다 접근 할 수 없기 때문에 private(은밀한) 변수라고 불린다.

하지만, private 변수에 접근해야할 때가 있다, 이것 또한 클로저를 사용할 수 있다.

~~~javascript
function secretFunc (secretCode) {
  return {
    saySecretCode () {
      console.log(secretCode)
    }
  }
}
const theSecret = secret('CSS Tricks is amazing')
theSecret.saySecretCode()
~~~

위 코드는 saySecretCode는 유일하게 secret 함수 바깥에서 secretCode를 노출하는 함수(클로저)이다 따라서, 이런 함수를 특권함수(Privileged function)라고 부른다.



**스코프와 클로저의 개념은 꼭 알아두어야하며 아직 모르는 부분도 많다 나중에 더 자세히 심도 깊게 알아볼 생각이다.**



####참고자료

- 인사이드 자바스크립트
- 속 깊은 JavaScript
- [번역]https://css-tricks.com/javascript-scope-closures/
