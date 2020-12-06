---
title: ES2020 Nullish coalescing operator ??
layout: post
author: Creatijin
category:
  - javascript
thumbnail: "/assets/img/posts/javascript-posting-null.jpg"
date: "2020-12-06 06:06:00"
summary: ES2020 Nullish coalescing operator ??
---

# ES2020 Nullish coalescing operator ??

Optional Chaining에 이어서 ES2020에 추가된 ?? 연산자를 알아보자

## Null 병합 연산자 ??

MDN에서는 '왼쪽 피연산자가 [`null`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/null) 또는 [`undefined`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자이다.' 라고 나와있다.

즉, `null` 과 `undefined`일 때, 오른쪽 피연산자를 `return` 한다.

`a ?? b` 의 경우

- `a` 가 `null`,`undefined`가 아니라면 `a` 를 리턴
- 그 외의 경우는 `b`

만약 `??` 연산자를 사용하지 않고 위와 같은 경우를 작성한다면

```javascript
x = a !== null && a !== undefined ? a : b; //이 처럼 코드가 길어진다.
```

## `??` 와 `||` 의 차이점

`||` 와 `??`는 `null`, `undefined`, 숫자`0` 을 다뤄야할 때 차이점이 두드러 진다.

```javascript
let width = 0;

console.log(width || 1040); // 1040
console.log(width ?? 1040); // 0
```

`||` 같은 경우에는 javascript에서 `0`을 `falsy`값으로 취급하기 때문에 1040의 값이 출력되는걸 볼 수 있다.

`??`은 width 할당 값이 `null`, `undefined` 경우에만 1040을 출력할 수 있다.

위의 경우처럼 `0` 을 다뤄야할 경우 `||` 보다 `??`가 적합하다.

## 우선순위

연산자의 경우 각자의 우선순위가 있는데 `??`의 경우에는 상당히 낮은 우선순위를 가지고 있다. `??` 연산자가 우선순위로 먼저인 경우는 `=`, `?`를 빼고는 우선순위가 낮다.

만약 먼저 `??` 값을 사용해야 한다면 `()` 를 사용해야한다.

```javascript
let width = null;
let height = null;

// 괄호 추가
let box1 = (height ?? 200) * (width ?? 50);
console.log(box1); // 10000
// 괄호 제거
let box2 = height ?? 200 * width ?? 50;
console.log(box2); // 0
```

## 제약사항

`??`에는 제약사항이 존재한다. **안정성 관련 이슈 때문에 `??`,`&&`,`||`는 함께 사용하지 못한다.**

```javascript
let z = 1 && 2 ?? 3;; //SyntaxError: Unexpected token '??'
```

이와 같이 제약을 둔 이유는 `||`를 `??`로 바꾸기 시작하면서 만드는 실수를 방지하고자 명세서에 제약이 추가되었다. 만약 이 제약을 피하려면 `()`를 사용하면된다.

## 마무리

- null 병합 연산자 `??`는 피연산자 중 '값이 할당된' 변수를 찾는데 유용하다.
- 변수에 기본값을 할당하는 용도로 매우 유용하다.
- 연산자 우선순위에는 `=`, `?`를 제외하고는 낮은 우선순위이다.
- `??`,`&&`,`||`를 같이 사용하는건 제약사항이며 사용하기 위해서는 `()`를 사용해야한다.

### 참고

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator
- https://ko.javascript.info/nullish-coalescing-operator
