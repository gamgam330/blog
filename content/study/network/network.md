---
title: "network(1)"
date: 2023-03-18
draft: false
category: [study]
subcategories: [network]
tags: [network]
---

이번글에서 다뤄볼 내용은 `host`와 `core`등 이다.

<!--more-->

먼저 네트워크에 대해 무지한 내가 이번학기가 끝남과 동시에 네트워크란 무엇이고 네트워크에 대해 설명이 가능한 사람이 되고자 기원한다.

첫 시작은 `edge`, `access network`, `core`에 대해 알아보도록 하겠다.

1. edge
   - 네트워크의 가장자리로 우리가 사용하는 디바이스에 해당한다. 여기에 end system이라는 개념이 등장하는데, 이것은 host라고 생각하면 된다. host는 `서비스의 주인`으로 IP Address가 있으면 host라고 부른다. 예를들어 스마트폰, PC 또는 회사의 서버이다.

2. access network
   - access network란 네트워크에 접근하기 위한 네트워크로, end system들이 인터넷을 사용할 수 있도록 길을 열어주는 네트워크이다. 와이파이에 접속하거나 유선랜을 꼽는 것 모두 access network에 접속하는 것 이다. 하지만 end system을 사이 경로상에 이쓴 첫번째 라우터에 연결하는 네트워크를 access network라고 부른다. 집은 `residential access networks`를 설치하고 학교, 회사, 기관은 `institutional access networks`를 설치한다 그리고 기지국은 `mobile access networks`를 설치해 edge router에 end system이 연결된다. 대부분은 통신사에서 엑세스 네트워크를 제공해준다.

3. core
   - core는 전체 네트워크 시스템의 중앙에 위치하여 데이터를 전송하는 핵심적인 역할을 수행한다. core의 구조는 `Mesh Of interconnected routers`로 수많은 라우터들이 그물처럼 얽혀있는 구조이다. 네트워크 코어에서 패킷을 교환하는 것을 `Packet switching` 이라고 부르는데, 이는 어느 지정된 디바이스에 패킷을 보내는 장치라고 보면된다. 예를들어 우리가 교실에서 세개의 스위치는 모두 다른 전등을 킬 수 있도록 만들어져있다. 이렇게 첫 번째 스위치는 맨앞 전등을 두 번째 스위치는 중간 세번째 스위치는 마지막 전등을 키는것과 같이 각각의 디바이스에 패킷을 보내주는 장치이다. 예를들어 WiFi 안테나나 송신기가 존재한다.

다음 내용을 살펴보기에 앞서 `protocol`과 `infrastructure`에 대해 짧게 살펴보겠다.

protocol은 프로토콜 또는 통신규약이라고 불리며, 컴퓨터나 원거리 통신 장비 사이에서 메세지를 주고 받는 양식과 규칙의 체계이다. 즉, 사람과 사람이 통신할 때 서로 이해할 수 있는 언어, 공용된 언어를 사용해 전세계 모든 사람과 대화 할 수 있다라고하면, 컴퓨터와 컴퓨터도 서로 이해 할 수 있는 언어, 공용된 언어를 사용해야 한다는것인데, 이것이 바로 프로토콜이다.

infrastructure이란 사회적 생산 기반. 또는, 경제 활동의 기반을 형성하는 기초적인 시설. 댐·도로·항만·발전소·통신 시설 등의 산업 기반 및 학교·병원·공원 등의 사회 복지·환경 시설이 이에 해당한다. 인터넷 또한 이러한 인프라이다.
  
이렇게 edge와 access network그리고 core에 대해 알아보았고, 바로 `DSL`과 `cable network`에 대해 알아보도록 하겠다.

1. DSL(digital subscriber line1)
   - Access network의 한 예이다. 통신사에 연결된 link가 집 안에서 전화기와 인터넷으로 나뉘어진다. 이 모든것이 모두 같은 네트워크에 물리게 된다. 하지만 전화는 dedicate이고 인터넷은 share가 된다. 먼저 음성과 데이터를 분류해 다른 주파수에 실어서 보내도록 한다. 전화는 음성 품질이 보장되어애 하므로 bandwidth가 일정하게 보장받도록 분리를 해준다. 이 분리된 것은 telephone network에 가도록 하며, 인터넷과 따로 이동하게 된다.
   - home DSL modem: 컴퓨터의 디지털신호를 아날로그 신호로 변환해준다.
   - home splitter: DSL modem을 거쳐 온 컴퓨터의 데이터와 전화의 음성을 서로 다른 주파수로 central office DSLAM에게 보낸다. 그리고 central office의 DSLAM으로부터 받은 데이터를 음성과 데이터로 분리하여 보낸다.
   - DSL은 dedicated line을 이용해 각 집마다 central office의 DSLAM으로 따로 연결이 되어 있다. 그래서 다른집과 충돌이 일어나지 않는다.

2. cable network
   - cable network는 DSL과 다르게 음성이 아니라 TV 데이터가 보내진다. bits은 파형의 형태로 날아간다. 높은 주파수와 낮은 주파수를 섞어서 보내면, 수신 딴에서 분리가 가능하다. 이것을 `frequenct division multiplexing`이라고 한다. 때문에 텔레비전, 인터넷, 전화 등의 신호들을 섞어서 보낼 수 있다. 
   - 여기에서 `HFC(Hybrid fiber coax)` 모델이 등장한다. 이는 각 집들은 cable headend로 가는 access network를 공유한다. DSL과 다르게 dedicated access가 아니고 `shared access`한다. cable에는 두 종류가 있는데 fiber cable과 coaxial cable이 존재한다. fiber은 bandwidth가 높아 여러 가정집을 연결한 경우에 사용되며 coaxial cable은 bandwidth가 낮다.

3. home network
   - 과거에는 DSL을 사용했으나 현재는 cable을 사용한다.

4. enterprise access networks(Ethernet)
   - 이더넷은 과거에 유선랜이라 불렸던 것이다. 학교나 군대PC에는 유선랜이 존재하는데 이것이 이더넷 케이블이다. 

5. wireless access networks
   - 무선랜은 WiFi기술이다. `WLANs(wireless local area networks)`는 싸고 거리가 짧은 무선랜이다.

6. wide-area wireless access
   - 기지국 통해서 서비스하는 cellular 네트워크이다. 비싸고 거리가 길다. 과거에 음성중심이었고, 지금은 스마트폰의 증가로 인터넷 까지 가능하다.

마지막으로 packet에 대해 살펴보도록 하겠다.

`packet`이란 Host는 데이터를 전송하는 기능이 있는데, 전송하려는 데이터를 chunk라는 것으로 자른다. 이것을 packet이라고 한다. 다시말해 전송하는 데이터를 일정한 크기의 데이터로 자른것을 packet이라고 한다.

`Packet transmission delay`로 패킷을 전송할 때 걸리는 시간에 대해 알아보도록 하겠다.

각 패킷의 길이를 L이라 하자. 어떤 host에서 데이터를 보내는데 link에서 보낼 수 있는 전송률인 link transmission rate를 R이라고 하겠다. R은 capacity, link bandwidth라고도 말할 수 있다. L bit짜리 데이터를 R이라는 capacity를 가지고 있는 구간에 전송할 때 전송 delay는 어떻게 될까?

바로 `L/R`이다. 패킷의 길이 L을 link의 전송률 R로 나누어 주면된다. 쉽게 풀어 link의 능력이 R인데 L을 보낸다는 뜻이다.