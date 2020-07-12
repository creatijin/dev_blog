---
title: Mob X & MST
layout: post
author: Creatijin
category: [Conference]
thumbnail: "/assets/img/posts/MobX_Logo.png"
date: '2020-07-03 08:32:00'
summary: MobX & MST - 편안한 State Management

---

## Mob X & MST

발표자 - 이현섭
발표일 - 2018.08
참고 - https://youtu.be/4yUgM7SaYUU


### MobX 소개 및 장점
- 어플리케이션 상태 관리 라이브러리
- 반응형 프로그래밍 패러다임
- "파생될 수 있는 모든 상태는 파생되어야 한다. 자동으로."
- 상태를 관찰가능(Observable)하게 관리

- 단순하고 이해하기 쉬움
- 필수 API가 적음
- 자유도가 높음
- OOP, 도메인 클래스 써서 데이터 모델링 하기가 적절
- 비동기 처리 간단
- TypeScript
  - 타입스크립트로 제작
  - API 타이핑을 라이브러리에서 자체 제공
  - 타입스크립트 사용자를 가정하고 개발
  - Typescript와 궁합 좋음 ( 데코레이터 등)


Serialize(직렬화) / Deserialize(역직렬화) 문제

- 서버에서 받아온 JSON 데이터를 observable하게 만들어야함
- 당시 모든 필드를 constructor에서 일일이 assign 했다.
- 스토어 구조가 커질수록 Deserialize 지옥 (Server Side Rendering)할때

### Redux VS MobX

#### Redux

- Immutable (Snapshot)
- State 업데이트 문법이 불편
- Pure Object (Snapshot
- Serialize 비용 적음
- 정규화된 트리 구조를 강제
- Single Root Tree
- 트리 순회 가능
- 모델링 / 타이핑 불편
- Time Traveling 지원

#### Mob X

- Mutable
- State 업데이트 편리함
- 보통은 인스턴스 결합
- 데이터 변경 시 Serialize 비용 큼
- 정규화 강제 안함
- 그래프 구조를 가질 가능성이 높음
- 순회가 불가할 수도 있음
- 모델링 / 타이핑 적합
- Time Traveling 안됨

### MobX State Tree

- MobX 베이스에 Redux의 장점들을 결합
- 정규화된 Single Tree 구성
- Mutable, But Immutable Snapshot 보장
- MobX 공식 지원 프로젝트
- 완전히 자유로웠던 MobX와는 달리 데이터 구조가 제한됨
- Oponionated 제약이 많이 걸려있다. (넌 그냥 개발만 해)



#### 첫 인상

- 문법이 맘에 들지 않음 (클래스 안씀)
- typeScript 타입이 아니라 라이브러리내 타입 시스템을 써야함
- 타입스크립트 쓰면서 propTypes 쓰는 거 같아서 좀 싫음
- 이것까지 쓰면 번들이 좀 클 것 같음
- 근데 Serialize / Deserialize가 공짜
- MobX 관련 모델링 라이브러리 중에서는 별이 가장 많다
- 모델링은 편함

### 요약

- 정규화된 State Tree를 구성
- 런타임 타입 체크, 타입 안정성 보장
- 모든 데이터는 observable
- 사용하는 API는 편리하게 가공된 인스턴스
- Referense로 다른 모델 참조 가능 (그래프)
- SnapShot으로 정규화된 Immutable 데이터를 보장

#### 그 외의 장점

- Memoization (without Reselect)
- 좋은 개발자 도구 (Redux-devtools, mobx-devtools, wiretap...)
- 미들웨어 지원 (로깅 및 LocalStorage 캐싱)



#### 단점

- Magic이 너무 많다 (투명하지 않음) [Redux에 비해서]
- Mobx에 비해 느린 성능 [MST 대상]
- 추론되는 타입이 너무 복잡하고 김 (그리고 느리다)
- 비동기 처리를 위해 yield를 사용해야함
- 귀찮은 모킹
- 보기 힘든 타입 에러 메시지
- 래퍼런스 거의 없음
- 안타까운 문서화 수준

#### 트리 분리하기

- 상태를 저장하는 트리가 꼭 한 개일 필요는 없다
- 모든 페이지에서 모든 상태를 들고 있어야 되므로 비효율적
- 특정 페이지에서만 쓰는 스토어의 경우 다른 루트로 분리해도 무방
- 이 경우 정합성은 보장 안됨

#### 작은 스토어 많이 만들기

- 도메인 스토어만 스토어는 아니다
- 실제로 프론트에서 다루는 상태 중 많은 것들이 View 상태
- 굳이 Global로 다룰 필요는 없다
- 페이지 단위의 상태 공유가 필요한 경우
- 그 외엔 웬만하면 React State 사용

#### 인스턴스 모킹

- 다른 스토어에 의존성이 있는 노드의 경우
- 트리 루트를 통해서 다른 스토어를 참조
- 레퍼런스 타입의 경우 같은 트리 안에 해당 id 를 가진 노드가 있어야함
- 전체 트리를 초기화하기 힘드므로 필요한 부분만 모킹



## 느낀점
회사 프로잭트에서 Redux에 대한 자료를 찾다가 MobX에도 눈길이 가서 자료를 찾아봤는데
2년전 자료지만 Redux와 MobX의 비교나 MobX의 기본적 원리에 대해서 잘 정리해주셔서 MobX를 이해하는데
상당한 도움이 되었다. 아직 사용해보진 않았지만 여러 개발자들의 말을 들어보면 프로젝트별로 Redux 나 MobX를 선택해서 사용하면 될꺼같다.