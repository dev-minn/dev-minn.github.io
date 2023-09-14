---
title:  "Chapter 1. 전략 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern]

toc: true
toc_sticky: true

date: 2023-09-13
---


## 📌 전략패턴

- 알고리즘군을 정의하고 캡슐화해서 각각의 알고리즘군을 수정해서 쓸 수 있게 해줌
- 전략패턴을 사용하면 클라이언트로부터 알고리즘을 분리해서 독립적으로 변경할 수 있음
- 일반적으로 if-else 로 구성된 코드 블록이 비슷한 기능 혹은 비슷한 알고리즘을 수행하는 경우에 전략패턴 적용함으로써 코드를 확장시킬 수 있음
- 비슷한 동작을 하지만 다르게 구현되어 있는 행위(전략)들을 공통의 인터페이스를 구현하는 각각의 클래스로 구현하고, 동적으로 바꿀 수 있도록 하는 패턴
- 전략 패턴으로 구현된 코드는 직접 행위에 대한 코드를 수정할 필요 없이 전략만 변경하여 유연하게 확장할 수 있게 됨
- 전략에 대한 인터페이스를 먼저 구현 후 각각의 전략 패턴을 클래스로 구현

<br>

## 📌 예제

### if-else 구현 

```java
public class Character {

    private final String job;

    Character(String job) {
        this.job = job;
    }

    void attack() {

        /*
         * 객체지향설계 5원칙 > 개방-폐쇄 원칙 위배
         */
        if(job.equals("warrior")) {
            System.out.println("warrior!!!");
        } else if(job.equals("thief")) {
            System.out.println("thief!!!");
        } else if(job.equals("magician")) {
            System.out.println("magician!!!");
        }
    } 
}
```

```java
public class Stage {
    public static void main(String[] args)  {
        Character warrior = new Character("warrior");
        Character thief = new Character("thief");
        Character magician = new Character("magician");

        warrior.attack();
        thief.attack();
        magician.attack();
    }
}
```


> 💡 <b>객체지향설계 5원칙 > 개방-폐쇄 원칙</b>
>
>  - 개방-폐쇄 원칙 ? 
>  - 객체의 확장은 개방적으로, 객체의 수정은 폐쇄적으로 대해야 한다는 원칙
>  - 기능이 변하거나 확장되는 것은 가능하지만 그 과정에서 기존의 코드가 수정되지 않아야 함
{: .notice--info}

<br>

### 전략패턴 리팩토링


#### STEP 1. 전략에 대한 인터페이스를 먼저 구현
```java
public interface AttackStrategy {
    String getAttackMessage();
}
```

#### STEP 2. 각 공격전략을 클래스로 구현(각각의 클래스는 모두 AttackStrategy 전략의 구현체)
```java
public class WarriorAttackStrategy implements AttackStrategy {
    public String getAttackMessage() {
        return "Warrior!!!";
    }
}
```

```java
public class ThiefAttackStrategy implements AttackStrategy {
    public String getAttackMessage() {
        return "Thief!!!";
    }
}
```

```java
public class MagicianAttackStrategy implements AttackStrategy {
    public String getAttackMessage() {
        return "Magician!!!";
    }
}
```

<br>

#### STEP 3. 전략에 대한 구현체를 작성했다면, Character 클래스는 아래와 같이 변경
```java
public class Character2 {

    private final AttackStrategy attackStrategy;

    Character2(AttackStrategy attackStrategy) {
        this.attackStrategy = attackStrategy;
    }

    void attack() {
        System.out.println(attackStrategy.getAttackMessage());
    }
    
}
```

<br>

#### STEP 4. 개선된 코드는 아래와 같이 사용
```java
Character2 warrior2 = new Character2(new WarriorAttackStrategy());
Character2 thief2 = new Character2(new ThiefAttackStrategy());
Character2 magician2 = new Character2(new MagicianAttackStrategy());

warrior2.attack();
thief2.attack();
magician2.attack();

```

<br>


## 📌 정리

✅ 알고리즘군을 정의하고 캡슐화해서 각각의 알고리즘군을 수정해서 쓸 수 있게 해줌<br>
✅ 전략패턴 STEP<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1️⃣ 비슷한 동작을 하지만 다르게 구현되어 있는 행위(전략)들을 공통의 인터페이스로 구현<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2️⃣ 각각의 전략 패턴을 클래스로 구현<br>

<br><br><br>

## 📌 참고

>  출처
- 헤드퍼스트 디자인 패턴
- https://hudi.blog/strategy-pattern/
- https://steady-coding.tistory.com/381