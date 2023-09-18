---
title:  "Chapter 02. 옵저버 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern]

toc: true
toc_sticky: true

date: 2023-09-14
---


## 📌 옵저버패턴

> 💡 <b>디자인 원칙</b>
> - 상호작용하는 객체 사이에는 가능하면 느슨한 결합을 사용함
{: .notice--info}

<br>

- 감시자 패턴
- 직접 상태 값을 관찰하는게 아니라 수동적으로 상태/값을 전달 받아 처리하는 패턴
- 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체에게 연락이 가고 자동으로 내용이 갱신되는 방식으로 일대다(one-to-many) 의존성을 정의
- 옵저버 패턴은 객체들 사이에 일대다 관계를 정의
- 주제는 동일한 인터페이스를 써서 옵저버에게 연락을 함
- Observer 인터페이스를 구현하기만 하면 어떤 구상 클래스의 옵저버라도 패턴에 참여할 수 있음
- 주제는 옵저버들이 Observer 인터페이스를 구현한다는 것을 제외하면 옵저버에 관해 전혀 모름. 따라서 이들 사이의 결합은 느슨한 결합
- 옵저버 패턴을 사용하면 주제가 데이터를 보내거나 옵저버가 데이터를 가져올 수 있음
- 옵저버 패턴은 여러 개의 주제와 메시지 유형이 있는 복잡한 상황에서 사용하는 출판-구독 패턴과 친척
- 옵저버 패턴은 자주 쓰이는 패턴으로 모델-뷰-컨트롤러(MVC)

<br>

## 📌 사용시기

- 앱이 한정된 시간, 특정한 케이스에만 다른 객체를 관찰해야 하는 경우
- 대상 객체의 상태가 변경될 때마다 다른 객체의 동작을 트리거해야 할 때
- 한 객체의 상태가 변경되면 다른 객체도 변경해야 할 때, 그런데 어떤 객체들이 변경되어야 하는 지 몰라도 될 때
- MVC 패턴에서도 사용됨(Model, View, Controller)
    - MVC 의 Model 과 View 의 관계는 Observer 패턴의 Subject 역할과 Observer 역할의 관계에 대응됨
    - 하나의 Model 에 다수의 View 가 대응

<br>

## 📌 장점

- Subject 의 상태 변경을 주기적으로 조회되지 않고 자동으로 감지할 수 있음
- 개방-폐쇄 원칙을 준수함
- 런타임 시점에서 발행자와 구독 알림 관계를 맺을 수 있음
- 상태를 변경하는 객체(Subject)와 변경을 감지하는 객체(Observer)의 관계를 느슨하게 유지할 수 있음(느슨한 결합)

<br>

## 📌 단점

- 구독자는 알림 순서를 제어할 수 없고, 무작위 순서로 알림을 받음
    - 하드코딩으로 구현할 수는 없겠지만, 복잡성과 결합성만 높아지기 때문에 추천되지는 않는 방법
- 옵저버 패턴을 자주 구성하면 구조와 동작을 알아보기 힘들어져 코드 복잡도가 증가
- 다수의 옵저버 객체들 등록 이후 해지하지 않는다면 메모리 누수가 발생할 수도 있음 

<br>

## 📌 예제

> 💡 <b>옵저버패턴 예제</b>
>
>  - Publisher(이벤트 발생 객체) 와 Observer 를 인터페이스로 구현해 Observer Pattern 구현 
>       - Observer 구현체는 외부에서 직접적으로 접근하지 않고 오로지 Publisher 를 통해 접근
>       - Observer 구현체가 이벤트를 감지한다는 것은 Publisher 에서 이벤트가 발생했을 때
>       - notifyObservers() 를 통해 모든 Observer 목록의 notify() 를 실행해준다는 뜻
{: .notice--info}

<br>

### 패턴 구현

#### STEP 1. 관찰자 인터페이스 구현

```java
public interface Publisher {
    
    // 관찰자 객체 추가
    public void addObserver(Observer o);

    // 관찰자 객체 삭제
    public void deleteObserver(Observer o);

    // 관찰자들에게 이벤트 발생 전달
    public void notifyObservers();

}
```

#### STEP 2. Observer 인터페이스 구현

```java
public interface Observer {
    
    // 이벤트 발생에 따른 행위(이벤트 발생 감지)
    public void notify(boolean play);
}
```

#### STEP 3. 구현체 생성

```java
public class PlayController implements Publisher {
    
    private List<Observer> observers = new ArrayList<>();
    private boolean play;
    private Observer myOb;

    @Override
    public void addObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void deleteObserver(Observer o) {
        observers.remove(o);
    }

    // Observer 들에게 변경사항을 전달
    @Override
    public void notifyObservers() {

        // Observer 목록들에게 이벤트 전달
        for(int i=0; i<observers.size(); i++) {
            observers.get(i).notify(play);
        }
    }

    // 이벤트 발생 함수
    public void setFlag(boolean play) {
        this.play = play;
        notifyObservers();
    }

    public boolean getFlag() {
        return play;
    }
}
```

```java
public class ObserverA implements Observer {
    
    private boolean bPlay;
    private Publisher publisher;

    public ObserverA(Publisher publisher) {
        this.publisher = publisher;
        publisher.addObserver(this);
    }

    @Override
    public void notify(boolean play) {
        this.bPlay = play;
        myActControl();
    }

    public void myActControl() {
        if(bPlay) {
            System.out.println("myClassA : 동작을 시작합니다.");
        } else {
            System.out.println("myClassA : 동작을 정지합니다.");
        }
    }
}
```

```java
public class ObserverB implements Observer {
    
    private boolean bPlay;
    private Publisher publisher;

    // 객체를 생성할 때
    public ObserverB(Publisher publisher) {
        this.publisher = publisher;
        publisher.addObserver(this);
    }

    // 이벤트 감지
    @Override
    public void notify(boolean play) {
        this.bPlay = play;
        myActControl();
    }

    // 행위
    public void myActControl() {
        if(bPlay) {
            System.out.println("myClassB : 동작을 시작합니다.");
        } else {
            System.out.println("myClassB : 동작을 정지합니다.");
        }
    }
}
```

> Main 테스트 프로세스
>   - Observer 구현체는 생성자를 통해 publisher 객체에 등록 됨
>   - PlayController 가 setFlag() 를 통해 이벤트를 발생시키고, Observer 구현체들의 notify 를 실행(이벤트감지)
>   - Observer 구현체들은 myActController() 를 통해 이벤트 감지의 따른 행위를 실행

```java
public class Main {

    public static void main(String[] args) {
        PlayController playController = new PlayController();
        Observer ob1 = new ObserverA(playController);
        Observer ob2 = new ObserverB(playController);

        // 이벤트 발생
        playController.setFlag(true);

        // Observer 삭제
        playController.deleteObserver(ob1);
        playController.setFlag(false);

    }
}
```

> 실행결과

```
myClassA : 동작을 시작합니다.
myClassB : 동작을 시작합니다.
myClassB : 동작을 정지합니다.
```

<br>

## 📌 정리

✅ 직접 상태 값을 관찰하는게 아니라 수동적으로 상태/값을 전달 받아 처리하는 패턴<br>
✅ 옵저버 패턴은 객체들 사이에 일대다 관계를 정의<br>
✅ Observer 인터페이스를 구현하기만 하면 어떤 구상 클래스의 옵저버라도 패턴에 참여할 수 있음<br>
✅ 대상 객체의 상태가 변경될 때마다 다른 객체의 동작을 트리거해야 할 때 사용됨<br>
✅ 상태를 변경하는 객체(Subject)와 변경을 감지하는 객체(Observer)의 관계를 느슨하게 유지할 수 있음(느슨한 결합)<br>


<br><br><br>

## 📌 참고

>  출처
- 헤드퍼스트 디자인 패턴
- https://inpa.tistory.com/
- https://cjw-awdsd.tistory.com/44