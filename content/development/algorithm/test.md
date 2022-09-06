---
title: "테스트당"
date: 2022-09-05
draft: false
category: [development]
subcategories: [algorithm]
tags: [Jenkins, Github]
---

위키백과에 따르면, `Jenkins`란 `CI Tool(Continuous Integration Tool)`로 다수의 개발자들이 하나의 프로그램을 개발할 때 버전 충돌을 방지하기 위해 각자 작업한 내용을 공유 영역에 있는 Git 등의 저장소에 빈번히 업로드 함으로써 지속적 통합이 가능하도록 해준다고 명시되어 있다. 

<!--more-->

[위키백과 링크](https://ko.wikipedia.org/wiki/%EC%A0%A0%ED%82%A8%EC%8A%A4_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4))

뭔가 굉장히 어려워 보이지만 단순히 한 예를 이야기 해 보자면, Github 등에 개발자가 소스코드를 수정하여 업로드 했을 경우 Jenkins를 사용하여 프로그램이 자동으로 빌드되도록 할 수 있다.
한 명의 개발자가 한 프로그램을 빌드하는 것은 그렇게 어렵지 않겠지만, 회사에서 여러 명의 개발자가 빌드를 해야 하는 경우에는 이야기가 조금 달라진다.