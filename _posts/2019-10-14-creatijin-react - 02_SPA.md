---
layout: post
title: 리액트 프로젝트 - 02 (1) CSS
author: Creatijin
category: [React]
thumbnail: /assets/img/posts/react-posting-bg.jpg
date: "2019-10-14T11:43:00.000Z"
summary: 리액트 프로젝트 - 02 (1) CSS
---


# 리액트 프로젝트 - 02 (2) SPA

## 단일 페이지 애플리케이션(Single page application)

**단일 페이지 애플리케이션(Single page application)[^1] 방식**? 리엑트는 SPA으로 개발하는 것이 정석이라는데 왜 그런지 SPA가 뭔지 알아보자.

위키를 찾아보니 SPA는 '**서버로부터 완전한 새로운 페이지를 불러오지 않고 현재의 페이지를 동적으로 다시 작성**함으로써 사용자와 소통하는 웹 애플리케이션이나 웹 사이트를 말한다.' 고 나와 있는데 쉽게 설명하자면 현재(2019-10-15) 기준 네이버 들어가서 새로고침을 누르면 화면이 깜빡이는 걸 볼 수 있을것이다. 전통적인 방식의 웹 페이지는 페이지를 전환할 때마다 렌더링 결과를 서버에서 받기 때문에 깜박이 생긴다.

하지만 SPA에서는 페이지 전환의 의한 렌더링을 클라이언트에서 처리하기 때문에 네이티브 애플리케이션처럼 자연스럽게 동작한다.

### 브라우저 히스토리 API

SPA를 구현하려면 두 가지 조건이 있다.

- 자바스크립트에서 **브라우저로 페이지 전환 요청**을 보낼 수 있지만, **브라우저는 서버로 요청을 보내지 않아야 한다**.
- 브라우저의 뒤로 가기와 같은 사용자의 페이지 전환 요청을 **자바스크립트에서 처리**할 수 있지만, **브라우저는 서버로 요청을 보내지 않아야한다.**

두 조건 모두 '브라우저가 서버로 요청을 보내지 않아야한다' '는 것이다. 두 조건을 만족 시키는 브라우저 API는 `pushState`[^1], `replaceState`[^2] 함수와 `popstate`[^3] 이벤트가 있다. 브라우저에는 히스토리에 state를 저장하는 스택(stack)이 있다.

```html
<div id="state"></div>
<button id="pushState">pushState</button>
<button id="replaceState">replaceState</button>
```

```javascript
//pushState
document.querySelector('#pushState').addEventListener('click', function () {
  history.pushState({ data: 'push' }, 'title - pushState', '/push')
});

//replaceState
document.querySelector('#replaceState').addEventListener('click', function () {
  history.replaceState({ data: 'replace' }, 'title - replaceState', '/replace');
});

//popstate
window.addEventListener('popstate', function () {
  console.log('popstate', history.state);
  document.querySelector('#state').innerHTML = JSON.stringify(history.state);
});
```

#### pushState

`pushState`, `replaceState` 두 메소드는 **(state, title, URL)**로 주소 이동과 함께 데이터도 저장할 수 있어 큰 장점이 있으며, 데이터 정보는 `history.state` 로 접근 가능하다.

`pushState` 로 **주소를 이동하면 브라우저의 뒤로가기가 활성화 됩니다.** 원래 주소로 돌아가던지 다시 앞으로 가기를 눌러 주소를 다시 이동 할 수 있다. 이유는 **이전 주소에 새로운 주소()`/push`) 를 추가하는 방식**입니다. 그렇기 때문에 뒤로가기도 가능합니다.

####replaceState

`replaceState` 를 눌러보면 **주소는 이동하지만 뒤로가기가 활성화되지 않는다.** `pushState` 와는 다르게 이전 주소에 새로 주소를 추가하는 방식이 아닌 **이전 주소를 없애고 바꿀 주소를 넣어주는 방식**입니다. 그렇기 때문에 이전 주소로 돌아갈 수 없습니다.

#### popstate

브라우저의 뒤로가기, 앞으로가기를 했을때 발생하는 이벤트다. `pushState`, `replaceState` 는 이벤트가 따로 발생하지 않기고 뒤로가기, 앞으로가기를 눌렀을때만  `popstate` 이벤트가 발생합니다.  `history.state` 에 접근해서 state를 가져올 수 있기 때문에 그 정보를 활용할 수 있습니다.



### react-router-dom 사용

브라우저 히스토리 API를 사용하면 직접 구현할 수 있지만 신경을 많이 써야한다. 그래서 리엑트에서는 `react-router-dom` 패키지로 페이지 라우팅 처리를 쉽게 구현할 수 있다.

```
npm install react-router-dom
```

`react-router-dom` 은 웹뿜만 아니라 리엑트 네이티브도 지원한다. 



우선 `React-router-dom`을 사용하기 위해서는 전체를 `<BrowserRouter>` 컴포넌트로 감싸야 한다. 그리고 페이지 전환을 하기 위해서는 `React-router-dom`에서 제공하는 Link 컴포넌트를 사용한다. (to 속성값은 이동할 주소) Roure 컴포넌트를 이용하여 각 페이지를 정의하고 현 주소가 path 속성값으로 시작하면 component 속성값이 가리키는 컴포넌트를 렌더링 한다.

~~~react
import Rooms from './Dress';

<BrowserRouter>
	<div>
  	<Link to="/">집</Link>
    <Link to="/photo">사진관</Link>
    <Link to="/dress">옷장</Link>
    <Route exact path="/" component={Home}/>
    <Route path="/photo" component={Photo}/>
    <Route path="/dress" component={Dress}/>
  </div>
</BrowserRouter>
~~~

Exact 속성값을 입력하면 그 값이 완전히 일치해야 해당 컴포넌트가 랜더링된다. 만약 Home 컴포넌트 부분에서 exact 속성값을 입력하지 않았다면 Home 컴포넌트는 항상 랜더링 된다. 혹시 같은 path 속성을 갖는 Route 컴포넌트를 여러 번 작성해도 상관 없다.

`<Route path="/dress" component={DressRoom}/>`,`<Route path="/dress" component={Dressy}/>`

위 경우에는 두 컴포넌트 모두 렌더링 된다.



~~~react
function Dress({ match }) {
  return (
    <div>
      <h2>여기는 옷을 소개하는 페이지입니다.</h2>
      <Link to={`${match.url}/bludDress`}>파란옷</Link>
      <br />
      <Link to={`${match.url}/greenDress`}>초록옷</Link>
      <br />
      <Route path={`${match.url}/:roomId`} component={Dress} />
      <Route
        exact
        path={match.url}
        render={() => <h3>옷을 선택해 주세요.</h3>}
      />
    </div>
  );
}
~~~

Dress 컴포넌트 내부에 다시 라우팅 처리하는 코드를 사용한다. Route를 통해서 렌더링된 컴포넌트는 match라는 속성값을 사용할 수 있다. `match.url` 은 **Route 컴포넌트의 path 속성값과 같다.** 따라서 Rooms 컴포넌트의 match.url은 /dress와 같다. Route 컴포넌트의 path 속성값에서 콜론을 사용하면 파라미터를 나타낼 수 있다. 추출된 파라미터는 **match.params.{파라미터이름} 형식**으로 사용될 수 있다.

[^1]:`pushState()` 함수 실행은 `window.location = "#foo"`를 실행하는 것과 비슷한 측면이 있습니다. 두 실행 모두 현재 다큐멘트에 연관되어 있는 또다른 히스토리 엔트리를 생성하고 활성화합니다. 
[^2]:`history.replaceState()`는 `history.pushState()`와 동일하게 동작합니다. 다만, `replaceState()`는 새로운 히스토리를 하나 생성하는 대신에 현재의 히스토리 엔트리를 변경합니다. 하지만 전역 브라우저 히스토리에 새로운 항목을 추가하는 것을 막지는 않습니다. `replaceState()` 는 state 객체나 사용자의 동작에 따라 현재 히스토리 엔트리의 URL을 업데이트 하려고 할 때 매우 유용합니다.
[^3]:`popstate` 이벤트는 현재 활성화된 히스토리 엔트리에 변화가 있을 때 마다 실행됩니다. 만약 `pushState` 함수나 `replaceState` 함수에 의해 현재 활성화되어 있는 히스토리 엔트리가 조작 및 변경된다면, `popstate` 이벤트의 `state` 속성은 히스토리 항의 state 객체의 사본이 됩니다.

### 참고자료

- https://www.zerocho.com/category/HTML&DOM/post/599d2fb635814200189fe1a7 [브라우저 히스토리 API]
