---
title: "Network(2)"
date: 2023-04-09
draft: false
category: [study]
subcategories: [Network]
tags: [Network]
---

`Physical media`

<!--more-->

`Physical media`
- 데이터 통신에서 정보를 저장하거나 전송하는데 사용되는 물리적 물질.

1. bit
   - transmitter/receiver pairs 사이에서 전파한다.

2. physical link
   - 송신기와 수신기 사이에 놓여 있다.

3. 유도 매체
   - 한 장치에서 다른 장치로 가는 통로를 제공한다
   - 구리선, 광케이블 등

4. 비유도 매체
   - 물리적인 도체를 사용하지 않고 전자기 신호로 전송한다.
   - 라디오파, 마이크로파 등

`구리선`과 `광케이블`은 속도 차이가 나게된다. 유리로 만들어진 광케이블의 속도는 무한하다. 현재 우리 인류에서 가장 속도가 빠른것은 빛으로 잘 알려져있다. 현재 광케이블은 그 빛을 광케이블망에 쏘아 보내는 기술이다. 빛보다 빠른 무언가가 발견된다면 더 빠른 속도로 광케이블을 통해 송신할 수 있다.

`Wireless radio`
- 전자파 스펙트럼으로 전달되는 신호이다.
- 양방향으로 사용 가능하다.
- 전파 환경에 영향을 받는다.
  - 반사, 물체에 의한 방해 등
- 지상파, 마이크로파, 와이파이, 셀룰러, 위성 인터넷 등이 있다.


`NetWork core`
- 서로 연결된 라우터들의 모임을 뜻한다.
- mesh of interconnected routers
  - 바둑판 모양으로 한군데가 마비가 되거나 오류가 나도 다른길을 찾아 패킷을 송신할 수 있다.

- packet switching
  - host는 애플리케이션 계층의 메세지를 패킷으로 쪼개 스위칭 한다. 

- `store and forward`
  - 라우터가 해당하는 패킷들을 다 받기 전까지 다음 라우터에 보내지 않는다. 전체 패킷이 라우터에 도착해야 다음 링크를 통해 다음 라우터로 전송하게 된다.
  - 패킷 전송에서는 `Transmission delay(전송 지연)`가 발생하는데, R bps의 속도로 L bit 패킷을 전송하는데 걸리는 시간은 L/R초이다.

- `queueing delay, loss`
  - 라우터의 메모리가 부족해 발생한다.
  - 링크 도착률이 전송률보다 높을 경우 발생한다.
  - 패킷은 출력 링크로 전송될 때 까지 대기하게 된다.
  - 만약 라우터에 있는 메모리 버퍼가 모두 찼을 경우에는 패킷 loss가 일어날 수 있다.


`Core의 두가지 기능`

1. `Forwarding`
   - 라우터의 입력 링크에 도착한 패킷을 적합한 라우터 출력 링크로 이동시킨다. 헤더 값의 목적지 주소를 읽고 목적지 링크에 따라 이동시킨다.

2. `Routing`
   - 양쪽 종단 패킷 경로를 결정하는 전역액션이다. 라우팅 알고리즘에 의해 계산된다.

Forwarding과 routing되는 과정에서 `processing delay(처리지연)`이 발생한다.


`Circuit switching`

- 서킷 스위칭은 전화망에서 주로 사용됐다. 음질을 중요시 했던 전화를 위해 사용되었다.
- 두 종단 경로를 이어두는 구조이다.
- 전화가 끊기기 전까지 전화선을 점유하기 때문에 모든 회선을 사용하고 있다면 더이상 새로운 사용자가 진입하지 못한다.
- 연결된 사용자들이 계속해서 통화하는것이 이상적이지만, 사용하지 않을때도 낭비가 된다.

- FDM(Frequency Division Multiplexing)
  - 전자기적 신호를 쪼개는 방식이다.
  - 각각의 호출은 자체 대역을 할당했고 최대 속도로 전송할 수 있다.

- TDM(Time Division Multiplexing)
  - 시간을 쪼개서 사용하는 방식이다.
  - 각 ㅊ호출은 주기적으로 슬록을 할당받고 최대 속도로 전송할 수 있다.

`packet switcing vs circuit switching`

1 Gbps로 지나갈 수 있는 링크가 있고, 각 유저는 활동 상태일 때 100Mb/s 속도로 지나간다. 활동 시간은 전체 시간의 10%이다.

- circuit switching 방식은 항상 할당되어있어야 하기때문에, 최대 10명의 사용자만 이용이 가능하다.
- packet switcing 방식은 10명을 초과하여 받을 수 있다. 만약 35명의 사용자가 존재할때 11명이상 사용할 확률을 확률질량함수를 통해 구해보면 0.0004에 불과하다. 때문에 동시간대에 사용하지 않는다면 10명 이상도 사용이 가능하다는 것이다.

`packet switching > circuit switching?!`

- 두 방법 모두 절대 우위는 아니다.
- 집중적인 데이터가 소요 될 때는 packet switching 방법이 좋은 방법이다. 데이터를 전송하지 않는 시간에는 리소스를 공유하고, 필요할 때 마다 링크를 사용할 수 있도록 해주기 때문이다.
- packet switching은 버퍼가 오버플로우가 됐을때 패킷 지연과 손실이 발생할 수 있다.

`Packet switching 장단점`

- 장점
  - 자원을 효율적으로 사용할 수 있다. 여러 사용자가 네트워크 자원을 공유하기 때문에 대역폭을 효율적으로 사용할 수 있다.
  - 유연성이 높다. 목적지까지 가는 경로를 다양하게 선택할 수 있기 때문에 네트워크의 변화에 대해 매우 유연하게 대처할 수 있다.
  - 오류 복국 빠르다. 오류가 발생한 경우 해당 패킷만 재전송하면 되므로 복구 시간이 빠르다.
- 단점
  - 전송 지연이 발생할 수 있다. 전체 데이터가 도착해야만 목적지에 전송된다.
  - 패킷 손실이 발생할 수 잇다.

`Circuit switching 장단점`

- 장점
  - 데이터가 전송되는 동안 지연이 적다. 전송 중인 데이터가 도중에 끊어지지 않기 때문에 지연이 발생하지 않는다.
  - 전송중인 데이터가 안정적이다. 전체 전송 경로가 미리 예약되기 때문에 데이터가 안정적으로 전송된다.
- 단점
  - 자원의 낭비가 발생할 수 있다.
  - 유연성이 낮다. 경로를 미리 연결해놓았기 때문에 네트워크 구조의 변경에 대한 어려움이 있다.
  - 오류 복구가 느리다. 전송중인 데이터가 끊어진 경우 전체 데이터를 재전송해야 한다.

`인터넷 구조`
- 디바이스(host)들은 인터넷 서비스 제공자(ISP)를 통해 인터넷에 연결된다.
- Access ISP들은 상호연결 되어야 한다.

`Global ISP`
- 매우 많고 각기 다른 ISP들이 존재한다. 이들 각각 직접 하나씩 연결되려면 많은 연결이 필요하다.
- 하나의 Global ISP를 만들게 되었고 소비자와 생산자 모두 경제적 합의하였다.
- IXP : Global ISP간 연결을 할 수 있도록 해 주는 것이다.
- peering link : Global ISP 간 직접 연결을 할 수 있도록 해주는 것이다.

`regional ISP`
- access net과 ISP를 이어주는 regional ISP가 등장한다.
- 통신사를 예로들을 수 있다.

`Content Provider Network`
- 구글과 같은 거대 기업을 의미한다.
- 자회사의 네트워크, 서비스를 제공한다.

`tier-1 commercial ISP`
- AT&T, NTT와 같은 곳을 의미한다.
- 국제적 범위이다.