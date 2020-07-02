---
title: 리액트 프로젝트 - 03 (4) javascript(Promise)
layout: post
author: Creatijin
category: [React, javascript]
thumbnail: "/assets/img/posts/react-posting-bg.jpg"
date: '2020-06-30T18:32:00.000Z'
summary: javascript(Promise)
---

# 리액트 프로젝트 - 03 (4) javascript(Promise)

##Promise

비동기 상태를 값으로 다룰 수 있는 객체이다.

Promise를 사용하면, 비동기 프로그래밍을 할때 동기 프로그래밍 방식으로 코드를 작성할 수 있는데 프로미스가 나오기 전에는 콜백 패턴을 많이 썼고 ES6에서 프로미스 등장 후 부터는 많이 사용되고 있다.

프로미스 등장전 콜백패턴의 문제점은 코드가 복잡해지는 문제가 있다.

```javascript
/* 콜백 패턴 예제 (1)~(5) 실행순서*/
function requestData1(callback) {
  //(2)
  //...
  callback(data);
}
function requestData2(callback) {
  //(4)
  //...
  callback(data);
}
function onSuccess1(data) {
  //(3)
  console.log(data);
  requestData2(onSuccess2);
}
function onSuccess2(data) {
  //(5)
  console.log(data);
  //...
}
requestData1(onSuccess1); //(1)
```

###Promise의 세가지 상태

- 대기중(pending) - 결과를 기다리는 중
- 이행됨(fulfilled) - 수행이 정상적으로 끝났고 결과값을 갖고 있음
- 거부됨(rejected) - 수행이 비정상적으로 끝났음

이행,거부 상태를 처리(settled)상태라고 부른다. 프로미스는 **처리 상태가 되면 더 이상 다른 상태로 변경되지 않는다. 대기 중 상태일 때만 이행 또는 거부 상태로 변할 수 있다.**

###Promise 생성하는 방법 3가지

####new 키워드

```javascript
const p1 = new Promise((resolve, reject) => {
  //...
  resolve(data);
  //or reject('error msg');
});
const p2 = Promise.reject('error msg'); //(1)거부
const p3 = Promise.resolve(param); //(2)이행
```

첫번째 방법은 new 키워드를 사용해서 생성한다.
이 방법으로 생성된 Promise는 대기(pending) 상태이며, 생성자에 입력되는 함수는 resolve와 reject라는 콜백 함수를 매개 변수로 갖는다.
(1) new 키워드를 사용하지 않고 Promise.reject를 호출하면 거부 상태 Promise가 생성된다.
(2) Promise.resolve를 호출해도 프로미스가 생성된다. 만약 입력값이 Promise였다면 그 객체가 그대로 반환된다. 프로미스가 아니라면 이햄된 상태인 Promise가 반환된다.

####then 사용하기

then은 처리됨 상태가 된 Promise를 처리할 때 사용되는 메소드다.
Promise가 처리됨 상태가 된다면 then메소드의 인수로 전달된 함수가 호출된다.

```javascript
/* then 메소드를 사용한 간단한 코드 */
requestData().then(onResolve, onReject);
Promise.resolve(123).then((data) => console.log(data)); // 123
Promise.reject('err').then(null, (error) => console.log(error)); // 에러발생!!
```

Promise가 처리됨 상태가 된다면 onResolve 함수가 호출된다. 거부됨 상태가 된다면 onReject함수가 호출된다.

then메소드는 항상 Promise를 반환하며, 하나의 Promise로부터 연속적으로 then 메소드를 호출할 수 있다.

```javascript
/* 연속해서 then 메소드 호출하기 */
requestData1()
  .then((data) => {
    console.log(data);
    return requestData2(); //(1)
  })
  .then((data) => {
    return data + 1; //(2)
  })
  .then((data) => {
    throw new Error('some error'); //(3)
  })
  .then(null, (error) => {
    console.log(error);
  });
```

1. onResolve 또는 onReject 함수에서 프로미스를 반환하면 then 메서드는 그 값을 그대로 반환한다.
2. 프로미스가 아닌 값을 반환하면 then 메서드는 이행됨 상태인 프로미스를 반환한다.
3. onResolve 또는 onReject 함수 내부에서 예외가 발생하면 then 메서드는 거부됨 상태인 프로미스를 반환한다.

**결과적으로 then 메소드는 항상 프로미스를 반환한다.**
프로미스가 거부됨 상태인 경우에는 onReject 함수가 존재하는 then을 만날때까지 이동한다.

```javascript
/* 거부됨 상태가 되면 onReject 함수 호출 */
Promise.reject('err')
  .then(() => console.log('then 1')) // (1)
  .then(() => console.log('then 2')) // (1)
  .then(
    () => console.log('then 3'),
    () => console.log('then 4'),
  ) //(2)
  .then(
    () => console.log('then 5'),
    () => console.log('then 6'),
  ); //(3)

// 출력값 then 4 , then 5
```

1. 거부됨 상태인 프로미스는 처음 만나는 onReject를 호출 하므로 1번 블록은 넘어간다.
2. 처음 만나는 'then 4'를 호출하게 된다. 그 후에 onReject 함수는 undefined 결과를 가지며 이행상태인 프로미스를 생성
3. 그 다음 then 메서드에서는 'then 5'가 출력된다.

**Then 메서드의 가장 중요한 특징은 항상 연결된 순서대로 호출 된다는 점**
이 특징은 **프로미스로 비동기 프로그래밍을 할 때 동기 프로그래밍 방식으로 코드를 작성**할 수 있게 해 준다.

#### catch 사용하기

catch는 프로미스 수행 중 발생한 예외를 처리하는 메서드이며 **catch 메서드는 then 메서드의 onReject 함수와 같은 역활을 한다.**

```javascript
Promise.reject(1).then(null, (error) => {
  console.log(error);
});
Promise.reject(1).catch((error) => {
  console.log(error);
});
```

예외 처리는 then 메소드의 onReject 함수보다 catch 메서드를 이용하는게 **가독성 면에서 더 효과적이다.**
그러면 가독성만 효과가 있는가? onReject 사용할때 문제점이 있다.

```javascript
Promise.resilve().then(
  () => {
    throw new Error('some error');
  },
  (error) => {
    console.log(error);
  },
);
// 거부됨 상태인 프로미스를 처리하지 않았기 때문에
// Unhandled promise rejection 에러가 발생한다.
// 수정 후
Promise.resilve()
  .then(() => {
    throw new Error('some error');
  })
  .catch((error) => {
    console.log(error);
  });
```

프로미스에서 예외 처리를 할 때는 onReject 함수보다 직관적인 catch 메서드를 이용하는 것이 좋다.
Catch 메서드 이후에도 계속해서 then 메서드를 사용할 수 있다.

#### finally 이용하기

프로미스가 이행됨, 거부됨 상태일 때 호출하는 메서드다. 2018년 자바스크립트 표준에 채택되었으며 프로미스 체인의 가장 마지막에 사용된다.
Finally 메서드는 이전에 사용된 프로미스를 그대로 반환한다는 점에서 차이점이 있다.
처리됨 상태인 프로미스의 데이터를 건드리지 않고 추가 작업을 할 때 유용하게 사용될 수 있다.

```javascript
function requestData() {
  return fetch()
    .catch((error) => {
      //...
    })
    .finally(() => {
      sendLogToServer('requestData finished');
    });
}
requestData().then((data) => console.log(data)); //(1)
//(1) requestData함수의 반환값은 finally 메서드 호출 이전의 프로미스다.
// requestData 함수를 사용하는 입장에서는 finally 메서드의 존재 여부를 신경쓰지 않아도 된다.
```