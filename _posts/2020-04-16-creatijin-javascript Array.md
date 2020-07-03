---
layout: post
title: "Basic - Array"
author: Creatijin
category: ["javascript"]
image: ../img/javascript-posting-array.jpg
date: "2019-07-08T03:05:00.000Z"
draft: false
---
# 자바스크립트 배열



## 1.배열생성

자바스크립트에서 배열의 생성하는 방식은 2가지

1. 리터럴 방식

   타이핑 양이 적고 가독성에서 우월하게 성능에서까지 좋다.

2. 생성자 방식

~~~Javascript
var arr1 = [];
var arr2 = new Array();
~~~



## 2.배열 생성자

생성자는 리터럴의 우수함때문에 사용할 이유가 없지만 그 외에도 사용하지 말아야할 이유가 있다.

배열 생성자의 결함은 인자 개수에 따라 다르게 작동하기 때문이다.



~~~javascript
var arr1 = new Array(5);
var arr2 = new Array(5, 6, 7);

console.log(arr1.length);    // 5
console.log(arr2.length);    // 3
~~~



결과를 확인해보면 알겠지만 인자가 1개일때는 배열의 크기로 사용하고 인자가 여러개일땐 배열의 요소로 사용하기 때문에 두 결과의 일관성이 없다.

한가지 더 큰 문제는 인자가 1개지만 타입이 숫자가 아닌 경우다.

~~~Javascript
var arr1 = new Array("5");

console.log(arr1.length);    // 1
~~~

이 경우에는 또 배열의 초기 크기가 아니라 배열의 요소로 사용한다.

이런 문제들을 방지하기위해선 자바스크립트 스펙에 대해 상세히 알아야하지만 잠깐 깜박하는 순간 프로그램은 의도대로 작동하지 않을 것이다.

더군다나 자바스크립트의 배열은 C 나 Java의 배열처럼 크기가 고정되어있는게 아니라 얼마든지 가변적으로 크기를 변경할 수 있기때문에 초기화 시점에 배열의 크기를 정하는게 큰 의미도 없다



## 3.빈 배열 생성

자바스크립트 배열은 생성하는 방식에 따라 빈 배열의 출력 내용이 다르다.

~~~javascript
var arr1 = [];   
arr1.length = 3;   
   
var arr2 = [undefined, undefined, undefined];   
   
var arr3 = new Array(3);   
   
var arr4 = new Array(undefined, undefined, undefined);   

//크롬
console.log(arr1);   //(3) [empty x 3]
console.log(arr2);   //(3) [undefined, undefined, undefined]
console.log(arr3);   //(3) [empty x 3]
console.log(arr4);   //(3) [undefined, undefined, undefined]

//익스
console.log(arr1);   //[object Array]    [undefined, undefined, undefined]
console.log(arr2);   //[object Array]    [undefined, undefined, undefined]
console.log(arr3);   //[object Array]    [undefined, undefined, undefined]
console.log(arr4);   //[object Array]    [undefined, undefined, undefined]
~~~

두 브라우저의 출력이 다르다.

크롬과 같은 크로니움 엔진을 사용하는 웨일 브라우저는 크롬과 같은 결과가 나온다. 모든 배열에 대해 동일한 결과를 보여주는 IE는 오히려 신경쓸게 없을거 같은데 크롬은 왜 결과가 다를까?

아마도 명시적으로 undefined 초기화를 해준 요소와 명시적인 초기화가 없는 요소 (undeclared)를 내부적으로 다르게 표현하는것 같다.



~~~javascript
var arr1 = Array.apply(null, {length:3});
console.log(arr1); //(3) [undefined, undefined, undefined]
~~~

이렇게 테스트를 해보면 명확하다. 



~~~Javascript
var arr = [];   
arr.length = 3;   
   
arr[0] === undefined;
true
~~~

위 연산은 브라우저 상관없이 true를 반환한다.

즉 크롬에서 undefined로 초기화되지 않은 요소의 '브라우저 출력' 결과를 좀 다르게 출력할뿐 실제 개발에서 사용될 비교문에서는 전혀 문제가 발생하지 않는다.



## 4.정리

자바스크립트는 배열의 리터럴을 제공한다. 또한 객체의 리터럴도 제공한다. 기본적으로 대다수의 언어에서 리터럴을 제공하는 표현에 대해서는 성능과 가독성 면에서 우수하다. 특히나 자바스크립트 배열 생성자는 인자의 타입과 개수에 따라 다르게 작동되도록 구현되어있으니 괜한 문제를 일으키고 싶은게 아니라면 애초에 싹을 없애버리도록 리터럴을 사용하는게 좋다.



빈 요소들(undefined)로 배열의 초기 크기를 지정하는 경우 지정 방법에 따라 브라우저별로 출력되는 방식이 좀 다른 문제가 있으나 출력에서만 다를뿐 내부 연산에서는 차이가 없다.

또한 동적으로 크기가 바뀌는 자바스크립트 배열의 초기 크기를 지정하는게 큰 의미가 없고, 더욱이 그걸 그대로 브라우저 콘솔창에 출력하는 경우는 거의 없으니 이것 때문에 무언가 코드를 변경하는건 무의미 하다.













