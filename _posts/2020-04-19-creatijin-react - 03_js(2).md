---
layout: post
author: Creatijin
title: 리액트 프로젝트 - 03 (2) javascript
date: "2020-04-19T01:56:00.000Z"
thumbnail: /assets/img/posts/react-posting-bg.jpg
category: [React, javascript]
summary: 객체와 배열
---


# 리액트 프로젝트 - 03 (2) javascript

##1.객체와 배열의 사용성 개선

ES6 이전까지는 var를 이용해서 변수를 정의했고 유일한 방법이었다. 하지만 ES6부터는 const, let을 사용할 수 있게 되었고 const, let이 나온 이유는 var가 많은 문제들이 있기 때문이다.

### 객체와 배열을 생성과 수정

- 단축 속성명 (shorthand property names)

- 계산된 속성명 (computed property names)

- 전개 연산자 (Spread Operator)

- 비구조화할당 (destructuring assignment)

  

#### 단축 속성명(shorthand property names)

단축 속성명은 객체 리터럴 코드를 간편하게 작성할 목적으로 만들어진 문법이다.

~~~javascript
const name = 'creatijin';
const obj = {
  age: 29,
  name,//(1)
  getName(){return this.name}//(2)
};
~~~

1. 새로 만들려는 객체의 속성값 일부가 **이미 변수로 존재한다면** 변수 이름만 적어 주면 된다. 속성명은 변수 이름과 같아진다.
2. 속성값이 함수면 function 키워드 없이 함수명만 적어도 된다. 속성명은 함수명과 같아진다.

~~~javascript
//단축 속성명을 사용하지 않는 경우
function programer1(age, name) {
  return {age: age, name: name};
}

//단축 속성명을 사용한 경우
function programer2(age, name) {
  return {age, name};
}
~~~



#### 계산된 속성명(computed property names)

객체의 속성명을 동적으로 결정하기 위해 나온 문법

~~~javascript
//계산된 속성명 사용하지 않는 경우
function programer1(key, value) {
  const obj = {};
  obj[key] = value;
  return obj;
}
//계산된 속성명 사용한 경우
function programer1(key, value) {
  return {[key]: value};
}
//계산된 속성명을 사용하면 함수를 간결하게 작성할 수 있다.

//react에서 컴포넌트 상탯값 변경하기
class myCom extends Component {
  state = {
    count1:0,
    count2:0,
    count3:0,
  };
	onClick = index => {
    const key = `count${index}`;
    const value = this.state[key];
    this.setState({ [key]: value + 1 });
  }
}
//setState 호출 시 계산된 속성명을 사용할 수 있다.
~~~



#### 속성값 편하게 가져오기

전개 연산자(Spread Operator)

~~~javascript
const languages = ['Python', 'Javascript', 'Kotlin', 'Go', 'Scala'];
console.log(...languages); // Python Javascript Kotlin Go Scala
~~~

위 예제 코드를 보면 알듯 '...'는 배열이나 객체의 모든 속성을 풀어 놓을 때 사용 가능하다.

전개 연산자는 배열이나 객체를 복사할 때도 편리하다.

~~~javascript
const arr1 = [1,2,3];
const obj1 = {age:23, name:'mike'};
const arr2 = [...arr1];
const obj2 = {...obj1};
~~~

전개 연산자는 복사할때 새로운 객체가 생성되기 때문에 **속성을 추가하거나 변경해도 원래의 객체에 영향을 주지 않는다.** 배열의 경우 전개 연산자를 사용하면 순서도 유지된다. 전개 연산자는 다른 두 배열이나 객체를 합칠때도 편리한데 만약 같은 이름의 속성명을 가지고 있다 하더라도 에러가 발생하지 않는다. (ES6부터 발생하지 않고 중복된 속성명은 마지막 속성명의 값이 된다.)



#### 배열 비구조화(Array Destructuring)

배열의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법이다.

~~~javascript
const arr = [1,2];
const [a,b] = arr;
console.log(a); //1
console.log(b); //2
//배열의 속성값이 왼쪽부터 순서대로 들어간다.

let c,d;
[c, d] = [1,2];
//이미 존재하는 변수에 할당할 수도 있다.

const arr = [1];
const [a=10, b=20] = arr;
console.log(a); //1
console.log(b); //20
//배열의 속성값이 undefined라면 정의된 기본값이 할당되고, 그렇지 않다면 원래의 속성값이 할당된다.

let a = 1;
let b = 2;
[a,b] = [b,a];
console.log(a) //2
console.log(b) //1
//두 변수의 값을 쉽게 교환할 수 있다. 
//*두 변수가 값을 교환하기 위해서는 제 3의 변수를 이용하는게 일반적이지만 비구조화를 사용하면 짧은 코드로 구현할 수 있다.

const arr = [1,2,3];
const [a, ,c] = arr;
console.log(a); //1
console.log(c); //3
//일부 속성값을 무시하고 진행하고자 한다면 건너뛰는 개수만큼 쉼표를 입력하면 된다.

const arr = [1,2,3];
const [first, ...rest1] = arr;//(1)
console.log(rest1); //[2,3]
const [a,b,c,...rest2] = arr;//(2)
console.log(rest2) // []
//(1)배열 비구조화시 마지막에 전개 연산자(...)를 변수명으로 입력하면 나머지 모든 속성값이 새로운 배열로 만들어진다.
//(2)나머지 속성값이 존재하지 않으면 빈 배열이 만들어진다.
~~~



#### 객체 비구조화

객체의 여러 속성값을 변수로 쉽게 할당할 수 있는 문법이다.

~~~javascript
const obj = {age:29, name: 'creatijin'};
const {age, name} = obj;
console.log(age); //29
console.log(name); //creatijin

const obj = {age:29, name: 'creatijin'};
const {age, name} = obj;//age:29, name:creatijin
const {name, age} = obj;//age:29, name:creatijin
const {a,b} = obj; // undefined
//배열 비구조화에서는 배열의 순서가 중점이였지만 객체 비구조화에서는 순서는 의미가 없고 기존의 속성명을 그대로 사영해야 한다는 점이 있다.

//별칭(Alias)사용하기
//별칭은 중복된 변수명을 피하거나, 좀 더 구체적인 변수명을 만들때 사용한다
const obj = {age:29, name: 'creatijin'};
const {age:theAge, name} = obj;
console.log(theAge); //29
console.log(age) // Reffrence Error
//theAge라는 이름의 변수만 할당되고 age변수는 할당되지 않는다.

//객체 비구조화에서의 기본값
const obj = {age: undefined, name: null, grade: 'A'};
const {age = 0, name = 'noName', grade='F'} = obj;
console.log(age); // 0; undefined인 경우에 기본값이 들어간다.
console.log(name); // null null은 기본값에 들어가지 않는다.
console.log(grade) // A

const obj = {age: undefined, name: 'creatijin'};
const {age: theAge = 0, name} = obj;
console.log(theAge); // 0
//이런식으로 기본값과 별칭과 동시에 사용 가능하다.

//객체 비구조화에서 나머지 속성들을 별도의 객체로 생성
//Spread Operator(전개 연산자)를 사용하여 별도의 객체로 분리 할 수 있다.
const obj = {age: 29, name: 'creatijin', grade: 'A'};
const{age, ...rest} = obj;
console.log(rest); // {name: 'creatijin', grade: 'A'}

//for문을 이용한 객체 비구조화
const people = [{age: 29, name: 'creatijin'}, {age:29, name: 'seungjin'}];
for (const {age, name} of people) {
    // ...
}
~~~



























