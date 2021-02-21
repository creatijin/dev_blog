---
title: 리펙토링
layout: post
author: Creatijin
category:
  - javascript
thumbnail: ""
date: "2021-02-22 01:13:00"
summary: 리펙토링 - 어떤걸 리펙토링 해야하는가?
---

# 리펙토링

마틴파울러 - 소프트웨어에 **겉보기 동작은 그대로 유지**한채 코드를 이해하고 수정하기 쉽도록 내부구조를 변경하는 기법

## 3의 법칙

- 처음에는 그냥 실행해본다

- 비슷한일을 두번째로 한다면 (중복) 그래도 일단 진행한다.

- 비슷한일을 세번째 할때 그때가 바로 리펙토링을 할때다.

## 어떤걸 리펙토링 해야하는가?[code smell]

1. 너무 큰 함수나 클래스
2. 이름이 명확하지 않는 함수나 변수명
   - 지나치게 짧은것보단 명확하게 조금 긴게 더 좋다
3. 중복코드(같은 일을 하는 코드가 여저기저 있으면)
4. 전역변수
5. 과도한 콜백과 중첩된 조건문
6. 과도하게 긴 식별자(너무 긴것도 문제다.)

## 리펙토링의 방법

1. 함수를 추출하거나 혹은 옮기는 방법

   ```javascript
   const logPost = (post) => {
     console.log(post);
   };

   const processPost = () => {
     const posts = getPosts();

     //변경전
     posts.forEach((post) => {
       console.log(post);
     });
     //변경후
     posts.forEach(logPost);
   };
   ```

2. 중간변수를 도입

   ```javascript
   const user = getUser();
   //변경전
   if (user.authKey) {
      	...
   }
   //변경후
   const isLoggedIn = Boolean(user.authKey);
   if (isLoggedIn) {
       ...
   }
   ```

3. 함수로 만드는 방법

   ```javascript
   //변경전
   const user = getUser();
   const purchases = getPurchaseHistory(user);
   if (user.authKey && user.locale === "kr" && purchases.length === 0) {
     showEventBanner();
   }
   //변경후
   const isEligibleForEventBanner = () => {
     const user = getUser();
     const purchases = getPurchaseHistory(user);
     const isLoggedIn = Boolean(user.authKey);
     const isKorean = user.locale === "kr";
     const hasPurchaesHistory = purchases.length === 0;
     return isLoggedIn && isKorean && !hasPurchaesHistory;
   };
   if (isEligibleForEventBanner()) {
     showEventBanner();
   }
   ```

4. var => let,const 로 변경(let보단 const를 사용하고 let을 사용해야한다면 꼭 써야하는지 고려한다.)

   - 함수의 경우 사이드 이펙트 최소화, 순수함수(똑같은 입력값이 주워졌을때 똑같은 출력을 하는 함수)로 작성(명확해지고 테스트에도 편리)

5. 조건식 통합

6. 페스트 페일

7. 반복문 보단 파이프 (이해하기 쉬워서)

   ```javascript
   //변경전
   const names = [];
   for (const i of input) {
     if (i.job === "programmer") {
       names.push(i.name);
     }
   }
   //변경후
   const names = input
     .filter((item) => item.job === "programmer")
     .map((item) => item.name);
   //함수를 계속 호출하는거기 때문에 성능면에서는 손해다 하지만 수만건을 다루는것이 아니면 큰 차이가 없다.
   ```

8. 조건문이나 반복문에 중괄호는 사용해주는게 가독성이 좋다.

9. 스위치 대신 오브젝트 리터럴 사용

   ```javascript
   //변경전
   const sayJob = (job) => {
     switch() {
       case 'engineer':
         console.log('엔지니어');
       	break;
       case 'developer':
         console.log('개발자');
       	break;
       case 'designer':
       	console.log('디자이너');
       	break;
     }
   }
   //변경후
   const sayJob = (job) => {
     const translated = {
       designer: '디자이너'
       developer: '개발자',
       engineer: '엔지니어',
     }
     console.log(translated[job]);
   }
   ```

10. 배열이나 객체는 불변객체처럼 Immutable하게 다루는게 좋다. ( 배열이나 객체에 그냥 추가하게되면 변화가 추적되지 않아서 문제가 생길 수 있다. )

    ```javascript
    //변경전
    const ohMyGirl = ['아린', '비니', '미미', '유아', '지호', '진이', '승희', '효정'];
    ohMyGirl.forEach((value, index)=>{
      blackPink[index] += '♥';
    }):
    console.log(ohMyGirl);

    //변경후
    const ohMyGirl = ['아린', '비니', '미미', '유아', '지호', '진이', '승희', '효정'];
    const ohMyGirlkWithLove = ohMyGirl.map((value, index)=>{
      blackPink[index] += '♥';
    }):
    console.log(ohMyGirlkWithLove);
    ```

11. 템플릿 문자열을 사용 `${}`

### 참고

- The Red(프론트엔드 Back to the Basics)
