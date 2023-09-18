---
title:  "Chapter 4. 팩토리 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern]

toc: true
toc_sticky: true

date: 2023-09-15
---


## 📌 팩토리패턴

> 💡 <b>디자인 원칙</b>
> - 의존성 뒤집기 원칙(Dependency Inversion Principle)
> - 추상화된 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만듬
{: .notice--info}

<br>

- 팩토리 메서드 패턴
- 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정
- 팩토리 메서드를 사용하면 인스턴스 만드는 일을 서브클래스에 맡길 수 있음
- 사용하는 서브클래스에 따라 생산되는 객체 인스턴스가 결정됨

<br>

<img src="/assets/images/04004.png" witdh="600" height="400">

- Creator 추상 클래스에서 객체를 만드는 메소드, 즉 팩토리 메서드용 인터페이스를 제공
- Creator 추상 클래스에 구현되어 있는 다른 메소드는 팩토리 메소드에 의해 생산된 제품으로 필요한 작업을 처리
- 실제 팩토리 메소드를 구현하고 제품(객체 인스턴스)을 만드는 일은 서브클래스에서만 할 수 있음