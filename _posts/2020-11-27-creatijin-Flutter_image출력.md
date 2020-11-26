---
title: Flutter 이미지 출력하기
layout: post
author: Creatijin
category: [Flutter]
thumbnail: "/assets/img/posts/flutter-logo-sharing.png"
date: "2020-11-27 02:53:00"
summary: Flutter 이미지 출력
---

# Flutter image 출력

Card Widget을 사용해서 이미지를 출력해보려고 한다.

플루터 프로젝트에서 이미지 파일을 담을 폴더를 하나 생성

![flutter_img(1)](/assets/img/posts/flutter_img1.png)

그러고 난 후에 pubspec.yaml 파일에 assets 부분을 주석을 풀어주고 경로를 입력

![flutter_img(1)](/assets/img/posts/flutter_img2.png)

위 이미지 처럼 assets에 경로를 입력해 줍니다.

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('EasyList'),
        ),
        body: Card(
            child: Column(
          children: <Widget>[
            Image.asset('images/PIHDG.png'),
            Text('kakao roomImg'),
          ],
        )),
      ),
    );
  }
}

```

body부분에 Card Widget을 사용해서 이미지와 이미지 설명을 넣어줍니다.

![flutter_img(1)](/assets/img/posts/flutter_img4.png)

시뮬레이터 아이폰 11에서 확인한 이미지

### 참고 자료

- https://fenderist.tistory.com/112?category=730848
