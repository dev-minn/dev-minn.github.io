---
title:  "Chapter 0. 디자인 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern]

toc: true
toc_sticky: true

date: 2023-09-12
---


## 📌 객체지향 기초

- <b>추상화</b> : 공통 속성 및 기능을 묶어 이름을 붙임
- <b>캡슐화</b> : 데이터 구조 및 다루는 방법들을 결합시켜 묶는 것
- <b>상속</b> : 상위 개념의 특징을 하위 개념이 물려받는 것
- <b>다형성</b> : 물려받은 상위 개념의 특징을 재정하는 것

<br>

## 📌 객체지향 원칙

> 💡 <b>디자인 원칙</b>
> - 상속보다는 구성을 활용

<br>

- <b>달라지는 부분을 찾아서 나머지 코드에 영향을 주지 않도록 '캡슐화'</b>
    - 캡슐화함으로써 나중에 바뀌지 않는 부분에는 영향을 미치지 않고 그 부분만 고치거나 확장할 수 있음
- <b>구현보다는 인터페이스에 맞춰서 프로그래밍</b>
    - 반드시 자바의 인터페이스를 사용하라는 뜻이 아니라
    - 실제 실행 시에 쓰이는 객체가 코드에 고정되지 않도록 상위 형식에 맞춰서 프로그래밍해서 다형성을 활용해야 한다는 것

<br>

> 💡 <b>상위 형식에 맞춰서 프로그래밍?</b>
> - 변수를 선언할 때 보통 추상 클래스나 인터페이스 같은 상위 형식으로 선언해야 함
> - 객체를 변수에 대입할 때 상위 형식을 구체적으로 구현한 형식이라면 어떤 객체든 넣을 수 있기 때문
> - 그러면 변수를 선언하는 클래스에서 실제 객체의 형식        

<br>

## 📌 디자인 패턴

- 객체지향 기초 지식만 가지고는 훌륭한 객체지향 디자이너가 될 수 없음
- 훌륭한 객체지향 디자인이라면 재사용성, 확장성, 관리의 용이성을 갖출 줄 알아야 함
- 패턴은 훌륭한 객체지향 디자인 품질을 갖추고 있는 시스템을 만드는 방법을 제공
- 패턴은 검증받은 객체지향 경험의 산물
- 패턴이 코드를 바로 제공하는 것은 아니고 디자인 문제의 보편적인 해법을 제공
- 패턴은 발명되는 것이 아니라 발견되는 것
- 대부분의 패턴과 원칙은 소프트웨어의 변경 문제와 연관되어 있음
- 대부분의 패턴은 시스템의 일부분을 나머지 부분과 무관하게 변경하는 방법을 제공
- 많은 경우에 시스템에서 바뀌는 부분을 골라내서 캡슐화해야 함
- 패턴은 다른 개발자와의 의사소통을 극대화하는 전문 용어 역할을 함

<br>

## 📌 정리

✅ 디자인 패턴? 설계 문제에 대한 해답을 문서화하기 위해 고안된 형식 방법<br>
✅ 훌륭한 객체지향 디자인이라면 재사용성, 확장성, 관리의 용이성을 갖출 줄 알아야 함<br>
✅ 패턴은 검증받은 객체지향 경험의 산물<br>
✅ 패턴은 다른 개발자와의 의사소통을 극대화하는 전문 용어 역할을 함<br>


<br><br><br>

## 📌 참고

>  출처
- 헤드퍼스트 디자인 패턴
