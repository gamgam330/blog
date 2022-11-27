---
title: "Spring 객체지향과 SOLID"
date: 2022-11-27
draft: false
category: [study]
subcategories: [spring]
tags: [Back-end, Spring]
---

자바를 기반으로한 spring 프레임워크는 `객체지향이라`는 가장 큰 장점을 가지고 있다.
<!--more-->

객체지향의 특징인 다형성을 가장 중요하게 생각한다. 다형성을 가진 spring은 `유연하고 변경에 용이`하다는 장점을 가지고 있다.

예를 들어 살펴보겠다.

ex) 운전면허가 있는 내가, 메르세데스 벤츠 E클래스를 운전하다가 제네시스 X 컨버터블로 차를 바꿧다. 이때 나는 운전을 처음부터 다시 배워야 하는가?

라는 질문을 봤을때의 답은 `아니요.` 일것이다. 이것처럼 차가 바뀐다고 내가 다시 운전을 배우고 운전면허증을 따야하는것은 아니다. 이렇게 모든 기능들을 인터페이스로 개발해 부품만 갈아끼우는 방식으로 개발 할 수 있는것이 spring의 가장 큰 장점이다.

SOLID 5원칙에 대해 살펴보겠다.

1. SRP : 단일 책임 원칙
2. OCP : 개방 폐쇄 원칙
3. LSP : 리스코프 치환 원칙
4. ISP : 인터페이스 분리 원칙
5. DIP : 의존관계 역전 원칙


1. SPR
   SPR은 `단일 책임 원칙`으로 한 클래스는 하나의 책임만 가져야 한다. 라는 특징을 가지고있다. 수정시에 하나의 클래스만을 수정할 수 있도록 설계해야 한다는 의미이다.

2. OCP
   SOLID의 5원칙 중 중요한 개념에 해당한다. 이는 소프트웨어 요소는 확장에는 열려있으나, 변경에는 닫혀 있어야 한다. 라는 의미를 가지고 있다. 기존 코드에 대한 변경은 없어야 하고 새로운 블록을 갈아 끼워야 한다는 소리이다.

   하지만, 구현객체에 대한 변경이 있다.
   ex) `Memorymember member = new memoryMemberrepository();` 와 `Memorymember member = new JDBCMemberrepository();` 처럼 기능에 따라 구현객체의 수정이 일어난다. 이에 대한 해결책으로는 연관관계를 맺어주는 별도의 조립이나 설정자가 필요하다.

3. LSP
   프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위타입의 인스턴스로 바꿀 수 있어야 한다. 즉, 자동차라는 객체를 봤을때, 악셀은 앞으로 가야한다 라는 약속을 깨면 안된다는 것이다. 컴파일이 성공해도 악셀을 밟았을때 뒤로가면 이것은 LSP원칙을 위반한 것이된다.

4. ISP
   특정 클라이언트를 위한 인터페이스 여러개가 범용 인터페이스 하나보다 낫다. 즉, 자동차 인터페이스에서 운전과 정비로 나누면 운전자, 정비사 클라이언트로 분기가 가능하고 이는 즉 정비관련 문제는 정비사 인터페이스에서만 수정을 하면 된다는 소리이다.

5. DIP
   이역시 SOLID 5원칙 중 중요한 개념에 해당한다. 의존성 주입은 이 원칙을 따라는 방법 중 하나이다. 즉, 구연코드가 클라이언트 말고 인터페이스를 바라봐야 한다. 즉 장동건이 김태희랑만 극중 역할을 연습했다고, 송혜교와는 촬영하지 못하면 안된다는 것이다. 그 극중의 역할만을 생각해야 한다는 것이다.

이렇게 다형성이라는 장점만으로는 `OCP`,`DIP`의 위반을 막지 못한다.

기본적으로 인터페이스와 구현체를 가진 코드를 살펴보겠다.

```java
package hello.core.order;

public interface OrderSerice {
    Order createOrder(Long memberId, String itemName, int itemPrice);

}

```

```java
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;
import hello.core.member.MemoryMemberRepository;

public class OrderServiceImpl implements OrderSerice{

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);

        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

```java
package hello.core.discount;

import hello.core.member.Member;

public interface DiscountPolicy {
    /**
     * @return 할인 대상 금액
     */
    int discount(Member member, int price);
}
```

```java
package hello.core.discount;

import hello.core.member.Grade;
import hello.core.member.Member;

public class FixDiscountPolicy implements DiscountPolicy{
    private int discountFixAmount = 1000;

    @Override
    public int discount(Member member, int price) {
        if(member.getGrade() == Grade.VIP){
            return discountFixAmount;
        }else {
            return 0;
        }
    }
}

```

인터페이스를 이용해 조립하는 다형성을 잘 활용하긴 했지만, OCP와 DIP에 대한 위반을 처리하진 못한다. 이방법은 차차 알아보도록 하겠다.