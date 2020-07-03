---
layout: post
title: "Basic - Function"
author: Creatijin
category: ["javascript"]
thumbnail: /assets/img/posts/javascript-posting-function.jpg
date: "2019-07-08T03:01:00.000Z"
summary: 자바스크립트 함수
---
# 함수

자바스크립트의 가장 좋은 점은 함수의 구현 부분이다.



함수는 실행문장들의 집합을 감싸고 있다.

**함수는 자바스크립트에서 모듈화의 근간이다.**

함수는 코드의 재사용, 정보의 구성 및 은닉 등에 사용하고 객체의 행위를 지정하는데도 사용한다.



*프로그래밍 기술은 요구사항의 집합을 함수와 자료구조의 집합으로 변환한 것이다*



### 01)함수 객체

**함수는 객체이다.**

객체는 프로토타입 객체로 숨겨진 연결을 갖는 이름/값 쌍들의 집합체이다.

객체 중에서도 객체 리터럴로 생성되는 객체는 Object.prototype 에 연결된다.

**반면 함수 객체는 Function.prototype 에 연결이 된다.**

(Function은 Object.prototype에 연결된다.)



또한 모든 함수는 숨겨져 있는 두 개의 추가 속성이 있다.

- 문맥(context)
- 함수의 행위를 구현하는 코드(code)



**모든 함수 객체는 prototype 이라는 속성이 있다.**

(이 속성의 값은 함수 자체를 값으로 갖는 [^constructor] 라는 속성이 있는 객체이다.)

Function.prototype으로 숨겨진 연결과는 구분된다. **뒷쪽에서 더 자세히 알아보자**



> 함수는 객체이기 때문에 다른 값들처럼 사용할 수 있다.
>
> 함수는 변수나 객체, 배열 등에 저장되며, 다른 함수에 전달하는 인수로도 사용하고, 함수의 반환값으로 사용한다.
>
> 함수를 다른 객체와 구분 짓는 특징은 호출할 수 있다.



[^constructor]: 생성자 메소드는 클레스가 오브젝트로 생성되고 초기되기 위한 특별한 메소드 입니다. 



### 02)함수 리터럴

- 함수 객체는 함수 리터럴로 생성할 수 있다.

  ​

~~~Javascript
//add라는 변수를 생성하고 두 수를 더하는 함수를 이 변수에 저장
var add = function (a, b) {
	return a + b;
};
~~~

#### 함수 리터럴

1. function 예약어


2. 선택사항으로 함수의 이름 함수의 이름은 함수를 재귀적으로 호출할 때 사용한다. 디버거나 개발 툴에서 함수를 구분할때 사용 **함수의 이름이 주어지지 않은 경우 익명함수(anonymous)**
3. 괄호로 둘러싸인 함수의 매개변수 집합, 괄호 안에 아예 없거나 하나 이상의 매개변수를 쉼표로 분리해서 열거 한다. **매개변수들은 함수내에서 변수로 정의한다.** 일반적인 변수들은 undefined로 초기화하는 것과는 달리 매개변수는 함수를 호출할 때 넘겨진 인수(argument)로 초기화한다.
4. 중괄호로 둘러싸인 문장들의 집합, 이러한 문장들은 함수의 몸체(body)이며 함수를 호출했을 때 실행
   - 함수 리터럴은 표현식이 나올 수 있는 곳이면 어디든 위치할수 있다.
   - 함수는 다른 함수 내에서도 정의할 수 있다.
   - 내부 함수도 매개변수와 변수를 가질 수 있으며 자신을 포함하고 있는 함수의 매개변수와 변수에도 접근할 수 있다.
   - 함수 리터럴로 생성한 함수 객체는 외부 문맥으로의 연결이 있는데 이걸 클로저(closure)라고 한다.




### 03)호출

함수를 호출하면 현재 함수의 실행을 **잠시 중단하고** 제어를 매개변수와 함께 호출한 함수로 넘긴다.

모든 함수는 명시되어 있는 매개변수에 더해서 this와 arguments라는 추가적인 매개변수 두 개를 받게 된다.

**this라는 매개변수는 객체지향 프로그래밍 관점에서 매우 중요**하다 이 매개 변수의 값은 호출하는 패턴에 의해 결정된다.



1. 메소드 호출 패턴
2. 함수 호출 패턴
3. 생성자 호출 패턴
4. apply 호출 패턴

각각의 패턴에 따라 this라는 추가적인 매개변수를 다르게 초기화



#### 메소드 호출 패턴

함수를 객체의 속성에 저장하는 경우 메소드라고 부른다.

**this는 메소드를 포함하고 있는 객체에 바인딩된다.** (즉, this는 객체 자체가 된다.)

~~~javascript
/*
value와 increment 메소드가 있는 myObject 생성.
increment 메소드의 매개변수는 선택적
인수가 숫자가 아니면 1이 기본값으로 사용됨
*/

var myObject = {
  value: 0,
  increment: function (inc) {
      this.value += typeof inc === 'number' ? inc : 1;
  }//this.value는 myObject에value를 가르킨다.
};

myObject.increment();
document.writeln(myObject.value); //1

myObject.increment(2);
document.writeln(myObject.value); //3
~~~

**자신의 객체 문맥을 this로 얻는 메소드를 퍼블릭(public) 메소드 라고한다.**

- 퍼블릭은 공개된
- 프라이빗은 숨겨진



#### 함수 호출 패턴

함수가 객체의 속성이 아닌 경우에는 함수로서 호출한다.

~~~javascript
var sum = add(3, 4); // 3 + 4 = 7
~~~

**함수를 이 패턴으로 호출할 때 this는 전역객체에 바인딩된다.** (strict mode -  undefined?)



~~~javascript
//myObject에 double 메소드를 추가

myObject.double = function () {
  var that = this;
  
  var helper = function () {
    that.value = add(that.value, that.value);
  };
  helper(); //helper를 함수로 호출
  
};
//double을 메소드로 호출

myObject.double();
document.writeln(myObject.getValue()); //6
~~~

**대안으로 메소드에서 변수를 정의한 후 여기에 this를 항당하고 내부 함수는 이 변수를 통해서 메소드의 this에 접근하는 방법**



이런 특성은 언어 설계 단계에서의 실수이다.

만약 바르게 설계했다면 내부 함수를 호출할때 이 함수의 this는 외부 함수의 this 변수에 바인딩 되어야한다.

메소드가 내부 함수를 사용하여ㅛ 자신의 작업을 돕지 못한다는 것이다.

(내부 함수는 메소드가 객체 접근을 위해 사용하는 this에, 자신의 this를 바인딩 하지 않고 전역객체에 연결하기 때문)



#### 생성자(constructor) 호출 패턴

함수를 new라는 전치 연산자와 함께 호출하면, 호출한 함수의 prototype 속성의 값에 연결된는 (숨겨진) 링크를 찾는 객체가 생성되고, 이 새로운 객체는 this에 바인딩 된다.

~~~javascript
//Quo라는 생성자 함수를 생성
//이 함수는 status라는 속성을 가진 객체를 생성함

var Quo = function (string) {
    this.status = string;
}

//Quo의 모든 인스턴스에 get_status라는 public 메소드를 줌
Quo.prototype.get_status = function () {
    return this.status;
}

//Quo의 인스턴스 생성

var myQuo = new Quo("confused");
document.writeln(myQuo.get_status()); //confused
~~~

**일반적으로 생성자는 이니셜을 대문자로 표기하여 이름을 지정한다.**

생성자를 new 없이 호출하면 컴파일 시간이나 실행시간에 어떠한 경고도 없어서 알 수 없는 결과를 초래한다. 그러므로 대문자 표기법을 사용하여 해당 삼수 생성자라고 구분하는것이 중요하다.

생성자 함수를 사용하는 스타일은 권장 사항이 아니다.



#### apply 호출 패턴

함수형 객체지향 언어이기 때문에, 함수는 메소드를 가질 수 있다.

Apply 메소드는 함수를 호츨할 때 사용하는 인수를 배열로 받아드린다.

this의 값을 선택할 수 있게 해준다.



~~~Javascript
var array = [3, 4];
var sum = add.apply(null, array); //7

// status라는 속성을 가진 객체를 만든다

var statusObject = {
    status: 'A-OK'
};

/*
statusObject는 Quo.prototype을 상속받지 않지만,
Quo에 있는 get_status 메소드가 statusObject를 대상으로
실행되도록 호출할 수 있다.
*/

var status = Quo.prototype.get_status.apply(statusObject);
// status는 'A-OK'

~~~



### 04)인수 배열(argument)

호출할 때 추가적인 매개변수로 arguments라는 배열을 사용할 수 있습니다.

함수를 호출할 때 전달된 모든 인수를 접근할 수 있게 한다.

매개변수 개수보다 더 많이 전달된 인수들도 모두 포함한다.

~~~javascript
var sum = function () {
    var i, sum = 0;
    for(i = 0; i < arguments.lengthl i += 1) {
        sum += arguments[i]
    }
  	return sum;
}
console.log(sum(2, 8, 15, 16, 23, 42));
~~~

**설계상 문제로 arguments는 실제 배열은 아닙니다. arguments는 배열 같은 객체**

arguments는 length라는 속성이 있지만 모든 배열이 가지는 메소드들은 없다.



### 05)반환

함수를 호출하면 첫번째 문장부터 실행해서 함수를 닫는 } 를 만나면 끝난다.

함수가 끝나면 호출한 부분으로 반환 된다.



Return 문은 함수의 끝에 도달하기 전에 제어를 반환할 수 있다.

return문을 실행하면 함수는 나머지 부분을 실행하지 않고 그 즉시 반환

**함수는 항상 값을 반환 반환값이 지정되지 않은 경우에는 undefined가 반환**



### 06)예외

자바스크립트는 예외를 다룰 수 있는 메커니즘을 제공

예외는 정상적인 프로그램의 흐름을 방해하는 비정상적인 사고

~~~javascript
// 새로운 add 함수를 잘못된 방법으로 호출하는
// try_it 함수 작성

var try_it = function () {
    try {
        add("seven");
    }  catch (e) {
      document.writeln(e.name + ':' + e.messsage );
    }
}

try_it();
~~~

