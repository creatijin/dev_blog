The red

#Back to Basics: 프레임워크보다 기본기

## 정확한 용어

1. 정확한 용어와 영문 발음도 확실히 알아두자, 신입,초급 개발자들은 평가 받는 자리가 많다보니 정확한 용어를 사용하지 못하면 상대방에게 안좋은 이미지를 줄 수 있다.
2. 번역에서도 중요한 영향이 있다.
3. 자료검색의 측면도 있다. 자료의 양과 질적인 측면에서도 차이가 많이 난다. 영어와 한국어의 자료량의 차이는 민망할 정도로 차이가 많이 난다.
4. 해외 취업의 기회 

## 명확한 근거

모든 결정에는 구체적인 이유를 주자 (어떤 프레임워크를 쓰고 어떤걸 결정할때 마다 명확한 이유가 있어야한다.)

- 왜 이런 기술 스택을 선택했는지에 대한 이유는 빠지지 않는 부분이다.(신경을 많이 쓰고있다.)

## 프론트엔드 기술 스택

웹브라우저 + HTML + CSS + JS

![스크린샷 2020-11-07 오후 1.39.27](/Users/joseungjin/Library/Application Support/typora-user-images/스크린샷 2020-11-07 오후 1.39.27.png)

- Prompt for unload - 다른 페이지로 이동할때 발생 / 해당 페이지에서 벗어날때 신호를 주는 것
  - beforeunload에 이벤트를걸면 이 페이지를 벗어나겠습니까? 라는 알림이 뜬다.
- Redirect - 요청한 URL에서 각각 서버쪽에서 Redirection 신호를 보낼때 발생 /옵셔널하다
  - 그래프의 노랑부분은 웹 페이지에서 벗어난 후 읽어드리기 전이기때문에 javascript 부분은 없다. 전부 네트워크 부분에 해당
- AppCache - 실제로 서버에서 데이터로 읽어오기전에 브라우저 캐시에 저장된 데이터가 있는지 확인
- DNS~Response 부분은 전부 네트워크 단계(패스) 필요한 파일들을 받아옴
- Processing - 필요한 파일들을 받아온걸 파싱, 렌더링 하는 단계
- DOMContentLoaded - HTML 읽고,파싱했고 DOM 까지 읽었는데 화면에 그리기 전에 JS로 기능을 추가할 수 있게 된다.(서브리소스(이미지나 동영상)은 아직 읽기전)
- Load - 필요한 리소스를 모두 읽고,다운로드받고 한 단계
  - Window.load 이벤트 발생

## 브라우저 주요 구성 요소

![스크린샷 2020-11-07 오후 1.54.22](/Users/joseungjin/Library/Application Support/typora-user-images/스크린샷 2020-11-07 오후 1.54.22.png)

- 사용자 인터페이스 - 북마크, 주소, 앞으로 뒤로, 탭 등등
- 브라우저 엔진 - 사용자 인터페이스와 렌더링 엔진의 다리역활
- 렌더링 엔진 - 실질적으로 웹 페이지를 표현하는 부분(가장 중요한 부분)
- 데이터 저장소 - 쿠키, 파일 데이터같은걸 저장하는 장소

브라우저에 대해 읽어보면 좋을 글

- 브라우저는 어떻게 작동하는가 - D2
- (추천) 모던 웹 브라우저 들여다보기 - 구글 developers

### 렌더링 엔진의 동작 과정

![스크린샷 2020-11-07 오후 3.26.21](/Users/joseungjin/Library/Application Support/typora-user-images/스크린샷 2020-11-07 오후 3.26.21.png)



## HTML5의 의미

### Semantic HTML

HTML 엘리먼트의 속성, 속성값은 특정한 의미를 지니도록 정의되었다.

html태그에는 각 내용에 맞는 태그들이 있다. 좀 더 신경을 쓰면서 작성을 해야한다.

header, nav, article, footer 등을 잘 사용해서 작성을 해야한다.

#### HTML변화

1. 버전이 없는 Living Standard로 변화

2. 의미를 가진 태그 대거 추가

   section, header, footer, main, article, nav 등

3. 의미를 기술하기 위해 Microdata도 포함

## CSS의 변화

1. CSS3는 없다
   - 각 기능별 모듈만 존재
   - "There will never be a CSS4" - Tab Atkins
2. CSS 명세가 방대해졌다.
   - 2020년 9월 현재 전체 CSS 프로퍼티 533개
   - 주요 개념과 주요 사용하는 속성 먼저 공부

### 레이아웃

1. 기본 개념 두 가지
   - 크기와 위치
2. CSS Box Model
   - CSS 레이아웃의 기본이 되는 원리
   - 엘리먼트의 크기를 정하는 표준
   - Box-sizing 속성으로 크기 계산 방식 변경 가능
3. 