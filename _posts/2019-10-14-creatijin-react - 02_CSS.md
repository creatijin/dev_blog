---
layout: post
author: Creatijin
title: 리액트 프로젝트 - 02 (1) CSS
date: "2019-10-14T11:43:00.000Z"
thumbnail: /assets/img/posts/react-posting-bg.jpg
category: [React]
summary: React CSS
---

# 리액트 프로젝트 - 02 (1) CSS

## Autoprefixer

최신 CSS를 사용하려면 벤더 접두사(vender prefix)[^1]가 붙은 이름을 사용해야 한다. 벤더 접두사를 수동으로 작성하는 일은 매우 고된 일인데 **create-react-app은 autoprefixer 패키지를 통해서 자동으로 벤더 접두사를 붙여준다.**



## CSS 작성 방법 결정

웹 애플리케이션을 개발할때 스타일을 담당하는 CSS파일이 전통적인 방법인데  CSS는 재사용성이 상당히 떨어진다. 그래서 재사용성을 늘릴 수 있는 **Sass,Less 등 변수와 믹스인(mixin) 개념이 있는 스타일시트 언어를 사용함으로써 중복 코드를 많이 줄일 수 있다.**

리액트 프로그래밍을 할 때도 **컴포넌트를 중심으로 생각하며 작성하면 컴포넌트 하나를 잘 만들어서 여러 곳에 재사용 하기 좋다**. 하지만 **재사용을 위해서는 각 컴포넌트는 서로 간의 의존성을 최소화하고 내부적으로 응집도를 높여야한다.** 

응집도기 높은 컴포넌트를 작성하려면 CSS 코드를 컴포넌트 내부에서 관리하는게 좋다. CSS 코드를 컴포넌트 내부에서 관리하는 방법은 css-module[^2], css-in-js[^3] 두가지가 있다. 하지만 일반적으로는 CSS, Sass를 쓴다.

### css-module 작성

CSS파일에서는 **클래스명이 겹치는 경우가 많은데 css-module에서는 간결한 클래스명을 이용해서 컴포넌트 단위로 스타일을 적용**할 때 좋다. Create-react-app에서 CSS 파일 이름은 `{이름}.module.css` 으로 작성하면 된다.

아래 코드는 css-module을 사용해서 작성한것으로 `classnames`패키지를 이용한 모습이다.

~~~javascript
//일반적인 사용
<div className={`${style.name} ${style.small}`}>creatijin</div>;

//classnames 패키지
import cn from 'classnames';

<div className={cn(style.name, style.small)}>creatijin</div>;
~~~

~~~html
<!--HTML에서 확인한 코드-->
<div class="name_small__jsJYe">creatijin</div>
~~~

Css-module 을 사용해서 확인해보면 HTML에서 class가 **[파일이름]\_[클래스이름]\__[해쉬값] 형식**으로 입력된다.


### Sass로 작성

Sass 문법으로 작성한 파일을 별도의 빌드 단계를 거쳐서 CSS 파일로 변환해야하는데 create-react-app 에서는 `node-sass` 패키지를 설치해야 한다. Sass를 CSS로 빌드할 때 사용된다.



### css-in-js로 작성

css-in-js는 최근에 떠오르고 있는 방법이며 **CSS 코드를 자바스크립트 파일 안에서 작성**하는 방법이다. CSS 코드가 자바스크립트 안에서 관리되기 때문에 **공통되는 CSS 코드를 변수로 관리**할 수 있으며, **동적으로 CSS 코드를 작성**하기 쉽다. 

Css-in-js를 지원하는 패키지도 다양하며 문법도 다양하다 하지만 개발자 개개인이 자바스크립트 와 CSS 모두를 작성할 줄 안다면 css-in-js는 좋은 선택이 될 수 있지만 CSS만 담당하는 마크업 개발팀이 별도에 있다면 도입이 힘들 것이다.

Css-in-js를 지원하는 패키지중에 하나인 styled-components의 사용법을 한번 보자.

~~~javascript
import styled from 'styled-componnents';

const BoxCom = style.div`
	height: 50px;
	bacground-color: #000000;
`;
const BoxSmall = BoxCom.extend`
	width:200px;
`;

function Box() {
  return <BoxSmall>작은 상자</BoxSmall>;
}

//동적

const BoxDynamic = style.div`
	width: ${props => (props.isSmall ? 100 : 200)}px;
`;

function Box({size}) {
  const isSmall = size === 'small';
  return <BoxDynamic isSamll={isSmall}>작은 상자</BoxDynamic>;
}
~~~

위와 같이 js파일 안에서 css를 관리 할 수 있기 때문에 개발자 개개인이 자바스크립트와 CSS 모두를 작성 할 주 안다면 css-in-js는 괜찮은 선택이다. (물론 팀원과 의논이 필요하다...) css-in-js는 **동적 스타일을 적용**할 수 있기 때문에 다른거 필요 없이 사용할 수 있다.



[^1]:브라우저별로 CSS속성을 잡아주기 위해 `-moz-`,`-webket`,`-o-`,`-ms-` 라는 각 브라우저에서 판독이 가능한 접두어를 붙여서 해당 브라우저에서 인식 할 수 있게 하는 것
[^2]:CSS 클래스를 불러와서 사용 할 때 [파일이름]\_[클래스이름]\__[해쉬값] 형태로 클래스네임을 자동으로 고유한 값으로 만들어줘서 컴포넌트 스타일 중첩현상을 방지한다. 사용은 [파일이름].module.css 형식으로 저장해야한다.
[^3]:리액트 css-in-js 관련 라이브러리중 **styled-components** 가 가장 잘나가고 있다. 이름에서 알 수 있듯 자바스크립트 파일 안에서 css를 작성하는 형태다.



### 참고자료

- https://velog.io/@velopert/react-component-styling#css-module [react-css]
- https://d0gf00t.tistory.com/22 [css-in-js]