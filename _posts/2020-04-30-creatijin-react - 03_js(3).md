---
layout: post
title: 리액트 프로젝트 - 03 (3) javascript
author: Creatijin
category: ['React', 'javascript']
thumbnail: /assets/img/posts/react-posting-bg.jpg
date: '2020-04-30T17:41:00.000Z'
summary: 함수, 화살표 함수
---

# 리액트 프로젝트 - 03 (3) javascript

## 1.강화된 함수의 기능

### 매개변수에 추가된 기능

```javascript
//매개변수 기본값

function printing(a = 1) {
  console.log(a);
}
printing(); //1
//기본값이 정의된 매개변수에 undefined를 입력하면 정의된 기본값이 나온다.

//기본값으로 함수 호출
function getDefault() {
  return 1;
}
function printLog(a = getDefault()) {
  console.log(a);
}
printLog(); // 1
//입력값이 undefined인 경우에만 호출된다는 특징을 이용하면 매개변수에서 필숫값을 표현할 수 있다.

//기본값을 이용해서 필숫값 표현
function error() {
  return console.log('error');
}
function printLog(a = error()) {
  console.log(a);
}
printLog(10); //10
printLog(); //error

//나머지 매개변수(rest parameter)
//인수중에서 정의된 매개변수 개수만큼을 제외한 나머지를 배열로 만들어준다.
function printing(a, ...rest) {
  console.log(a, rest);
}
printing(1, 2, 3); // 1 [2, 3]

//arguments 키워드
//arguments의 존재가 명시적으로 드러나지 않기 때문에 가독성이 안좋다.
function printing(a) {
  const rest = Array.from(arguments).splice(1);
  console.log(a, rest);
}
printing(1, 2, 3); // 1 [2, 3]
//나머지 매개변수를 사용하는게 더 가독성에서 좋다.

//명명된 매개변수(named parameter)
const numbers = [1, 2, 3, 4];
const result1 = getValues(number, 5, 25);
const result2 = getValues(number, (greaterThan: 5), (lessThan: 25));
const result3 = getValues(number, (lessThan: 25));
//result1 매개변수의 이름이 보이지 않아서 의미하는 바를 알기 어렵지만 result2는 명명된 매개변수를 이용하여 매개변수의 이름을 노출시킬 수 있다. result3의 경우 명명된 매개변수를 사용하면 필요한 인수만 넣어주어 선택적 매개변수가 늘어나도 별문제 없이 사용할 수 있다.
//명명된 매개변수를 사용하면 함수를 호출할 때마다 객체가 생성되기 때문에 비효율적일 것이라고 생각할 수 있지만 자바스크립트 엔진이 최적화를 통해 새로운 객체를 생성하지 않는다.
```

### 화살표 함수(arrow function)

화살표 함수를 이용하면 함수를 간결하게 이용할 수 있다.

```javascript
const add (a,b) => a + b; //(1)
console.log(add(1,2)); //3
const add1 = a => a+1; //(2)
console.log(add1(5)) //6
const addObject = (a,b) => ({result: a+b}); //(3)
console.log(addObject(1,2).result); //3
const printing = (...rest) => console.log(rest); //(4)
printing(1,2) // [1,2]
```

(1) 화살표 함수를 중괄호{}로 감싸지 않으면 오른쪽의 계산 결과가 반영된다. Return 키워드를 사용하지 않아도 되기 때문에 간결해진다.

(2) 매개변수가 하나라면 소괄호()를 생략해도 된다.

(3) 객체를 반환해야 한다면 소괄호()로 감싸야 한다.

(4) 화살표 함수는 this,arguments가 바인딩되지 않는다 arguments를 사용해야 한다면 **나머지 매개변수** 를 이용해야 한다.

```javascript
//일반 함수에서 this 바인딩 때문에 버그가 발생하는 경우
const obj = {
  value: 1,
  increase: function () {
    this.value++; //(1)
  },
};
obj.increase(); //(2)
console.log(obj.value); //2
increase(); //(3)
console.log(obj.value); //2
```

(1) 일반 함수이므로 호출시 사용된 객체가 this로 바인딩 된다.

(2) obj객체가 this에 바인딩 되므로 obj.value가 증가

(3) 객체 없이 호출될 경우에 전역 객체가 바인딩 된다(window 객체 바인딩), obj.value는 증가하지 않는다. 만약 화살표 함수로 작성 하더라도 화살표 함수 안에서 사용된 this와 arguments는 자신을 감싸고 있는 가장 가까운 일반 함수의 것을 참조하기 때문에 window객체를 가리키기 때문에 결과는 변하지 않는다.
