---
title:  "Chapter 3. 데코레이터 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern]

toc: true
toc_sticky: true

date: 2023-09-14
---


## 📌 데코레이터패턴

> 💡 <b>디자인 원칙</b>
> - OCP(Open-Closed Principle)
> - 클래스는 확장에는 열려 있어야 하지만 변경에는 닫혀 있어야 함
{: .notice--info}

<br>

- 객체에 추가 요소를 동적으로 더할 수 있음
- 데코레이터를 사용하면 서브클래스를 만들 때보다 훨씬 유연하게 기능을 확장할 수 있음  
- 데코레이터의 슈퍼클래스는 자신이 장식하고 있는 객체의 슈퍼클래스와 같음
- 한 객체를 여러 개의 데코레이터로 감쌀 수 있음
- 데코레이터는 자신이 감싸고 있는 객체와 같은 슈퍼클래스를 가지고 있기에 원래 객체(싸여 있는 객체)가 들어갈 자리에 데코레이터 객체를 넣어도 상관없음
- <b>데코레이터는 자신이 장식하고 있는 객체에게 어떤 행동을 위임하는 일 말고도 추가 작업을 수행할 수 있음</b>
- 객체는 언제든지 감쌀 수 있으므로 실행 중에 필요한 데코레이터를 마음대로 적용할 수 있음
- 데코레이터 순선느 원본 대상 객체 생성자를 장식자 생성자가 Wrapping 하는 형태로 간다고 보면 됨
    - new 장식자(new 원본())
- 디자인의 유연성 면에서 보면 상속으로 확장하는 일은 별로 좋은 선택은 아님
- 기존 코드 수정 없이 행동을 확장해야 하는 상황도 있음
- 구성과 위임으로 실행 중에 새로운 행동을 추가할 수 있음
- 상속 대신 데코레이터 패턴으로 행동을 확장할 수 있음
- 데코레이터 패턴은 구상 구성요소를 감싸 주는 데코레이터를 사용
- 데코레이터 클래스의 형식은 그 클래스가 감싸는 클래스 형식을 반영
- 상속이나 인터페이스 구현으로 자신이 감쌀 클래스와 같은 형식을 가짐
- 데코레이터는 자기가 감싸고 있는 구성 요소의 메소드를 호출한 결과에 새로운 기능을 더함으로써 행동을 확장함
- 구성 요소를 감싸는 데코레이터의 개수에는 제한이 없음
- 구성 요소의 클라이언트는 데코레이터의 존재를 알 수 없음
- 클라이언트가 구성 요소의 구체적인 형식에 의존하는 경우는 예외
- 데코레이터 패턴을 사용하면 자잘한 객체가 매우 많이 추가될 수 있고, 데코레이터를 너무 많이 사용하면 코드가 필요 이상으로 복잡해짐


<img src="assets/images/03001.png" witdh="600" height="400">

<br>

## 📌 사용시기

- 객체 책임과 행동이 동적으로 상황에 따라 다양한 기능이 빈번하게 추가/삭제 되는 경우
- 객체의 결합을 통해 기능이 생성될 수 있는 경우
- 객체를 사용하는 코드를 손상시키지 않고 런타임에 객체에 추가 동작을 할당할 수 있어야 하는 경우
- 상속을 통해 서브클래싱으로 객체의 동작을 확장하는 것이 어색하거나 불가능 할 때

<br>

## 📌 장점

- 데코레이터를 사용하면 서브클래스를 만들때 보다 훨씬 더 유연하게 기능을 확장할 수 있음
- 객체를 여러 데코레이터로 래핑하여 여러 동작을 결합할 수 있음
- 컴파일 타임이 아닌 런타임에 동적으로 기능을 변경할 수 있음
- 각 장식자 클래스마다 고유의 책임을 가져 단일책임원칙 준수
    - 단일책임원칙(SRP - 하나의 클래스는 하나의 기능을 담당하여 하나의 책임을 수행)
- 클라이언트 코드 수정없이 기능 확장이 필요하면 장식자 클래스를 추가하면 되니 준수 
    - 개방폐쇄원칙(OCP - 기존의 코드를 변경하지 않으면서, 기능을 추가할 수 있도록 설계)
- 구현체가 아닌 인터페이스를 바라봄으로써 의존역전원칙 준수
    - 의존역전원칙(DIP - 클래스를 참조할 때 직접 참조하는 것이 아니라 그 대상의 상위요소[추상클래스/인터페이스]로 참조)

<br>

## 📌 단점

- 만일 장식자 일부를 제거하고 싶다면, Wrapper 스택에서 특정 Wrapper 를 제거하는 것은 어려움
- 데코레이터를 조합하는 초기 생성코드가 보기 안 좋을 수 있음
    - new A(new B(new C(new D())))
- 어느 장식자를 먼저 데코레이팅 하느냐에 따라 데코레이터 스택 순서가 결정지게 되는데, 만일 순서에 의존하지 않는 방식으로 데코레이터를 구현하기는 어려움

<br>

## 📌 예제

> 💡 <b>예제</b>
>
>  - Data 클래스를 멀티쓰레드 환경에서도 사용할 수 있도록 동기화 처리
{: .notice--info}

### 상속을 통한 구현

#### STEP 1. MyData 클래스 구현

```java
public class MyData {
    private int data;

    public void setData(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }
}
```

#### STEP 2. MyData 클래스를 상속해 메서드를 오버라이딩 해 동기화 처리

```java
public class SynchronizedData extends MyData {
    
    private int data;

    public void setData(int data) {
        // synchronized (대상객체) { ... } 
        synchronized(this) {
            this.data = data;
        }
    }

    public int getData() {
        synchronized(this) {
            return data;
        }
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        SynchronizedData data = new SynchronizedData();
        data.setData(1);
        System.out.println(data.getData());
    }
}
```

<br>

### 데코레이터 패턴 리팩토링

> 💡 <b>데코레이터패턴 예제</b>
>
>  - Data 클래스를 멀티쓰레드 환경에서도 사용할 수 있도록 동기화 처리
{: .notice--info}

#### STEP 1. 원본 객체와 장식된 객체 모두를 묶는 인터페이스

> 동기화 처리가 안된 data 클래스와 동기화 처리가 된 data 클래스 모두를 묶어두는 IData 인터페이스 선언

```java
public interface IData {
    void setData(int data);
    int getData();
}
```

#### STEP 2. 장식될 원본 객체

```java
public class Mydata2 implements IData {
    private int data;

    public void setData(int data) {
        this.data = data;
    }

    public int getData() {
        return data;
    }
}
```

#### STEP 3. 데코레이터를 추상화한 MyDataDecorator 선언

> 데코레이터를 추상화한 MyDataDecorator 선언

```java
public class MyDataDecorator implements IData {

    // 최상위 인터페이스 타입으로 장식할 원본 객체 선언
    private IData mydataObj;

    MyDataDecorator(IData mydataObj) {
        this.mydataObj = mydataObj;
    }

    public void setData(int data) {
        this.mydataObj.setData(data);
    }

    public int getData() {
        return mydataObj.getData();
    }
}
```

<br>

> 💡 <b>추상클래스로 선언한 이유?</b>
>
>  - 동기화 처리 외에 또 다른 처리 기능이 추가되었을 때 유연하게 확장시키기 위해서
>  - 각 장식자 클래스의 중복되는 코드를 묶기 위해서
{: .notice--info}


#### STEP 4-1. 장식자 클래스

> MyDataDecorator 추상 클래스를 상속하는 서브 장식 클래스 구현체 SynchronizedDecorator 를 선언

```java
public class SynchronizedDecorator extends MyDataDecorator {
    
    SynchronizedDecorator(IData mydataObj) {
        super(mydataObj);
    }

    public void setData(int data) {
        synchronized(this) {
            System.out.println("동기화된 data 처리를 시작합니다.");
            // 부모 메서드를 호출함으로써 자신을 감싸고 있는 장식자의 메서드를 호출
            super.setData(data);        
            System.out.println("동기화된 data 를 처리를 완료하였습니다.");
        }
    }

    public int getData() {
        int result = 0;
        synchronized(this) {
            System.out.println("동기화된 data 처리를 시작합니다.");
            result = super.getData();
            System.out.println("동기화된 data 를 처리를 완료하였습니다.");
        }
        return result;
    }
}
```

#### STEP 4-2. 부가적 장식자 클래스

> 나중에 기능 추가 요구사항이 와도 코드 수정없이 유연하게 클래스를 정의

```java
public class AnotherSkillDecorator extends MyDataDecorator {

    private IData mydataObj;

    AnotherSkillDecorator(IData mydataObj) {
        super(mydataObj);
    }

    public void setData(int data) {
        long stratTime = System.nanoTime();
        super.setData(data);
        long endTime = System.nanoTime();
        long durationTimeSec = endTime - stratTime;
        System.out.println(durationTimeSec + "n/s");
    }

    public int getData() {
       long stratTime = System.nanoTime();
       int result = super.getData();
       long endTime = System.nanoTime();
       long durationTimeSec = endTime - stratTime;
       System.out.println(durationTimeSec + "n/s");
       return result;
    }
}
```

> Client 클래스 프로세스

```java
public class Client {
    public static void main(String[] args) {
        // 동시성이 필요없을 때
        IData iData = new Mydata2();

        // 동시성이 필요할 때
        IData dataSync = new SynchronizedDecorator(iData);
        dataSync.setData(1);
        System.out.println(dataSync.getData());

        // 시간 측정하고 싶을 때
        IData another1 = new AnotherSkillDecorator(iData);
        another1.setData(1);

        // 동시성이 적용된 로직 안의 코드를 시간 측정하고 싶을 때
        IData another2 = new SynchronizedDecorator(new AnotherSkillDecorator(iData));
        another2.setData(1);

        // 동시성이 적용된 코드를 시간 측정하고 싶을 때
        IData another3 = new AnotherSkillDecorator(new SynchronizedDecorator(iData));
        another3.setData(1);
    }
}
```

> 실행결과

```
동기화된 data 처리를 시작합니다.       
동기화된 data 를 처리를 완료하였습니다.
동기화된 data 처리를 시작합니다.
동기화된 data 를 처리를 완료하였습니다.
1
1500n/s
동기화된 data 처리를 시작합니다.
800n/s
동기화된 data 를 처리를 완료하였습니다.
동기화된 data 처리를 시작합니다.
동기화된 data 를 처리를 완료하였습니다.
904500n/s
```

<br>

## 📌 정리

✅ 객체에 추가 요소를 동적으로 더할 수 있음
✅ 데코레이터를 사용하면 서브클래스를 만들 때보다 훨씬 유연하게 기능을 확장할 수 있음
✅ 데코레이터는 자신이 장식하고 있는 객체에게 어떤 행동을 위임하는 일 말고도 추가 작업을 수행할 수 있음
✅ 상속이나 인터페이스 구현으로 자신이 감쌀 클래스와 같은 형식을 가짐 
✅ 상속 대신 데코레이터 패턴으로 행동을 확장할 수 있음


<br><br><br>

## 📌 참고

>  출처
- 헤드퍼스트 디자인 패턴
- https://inpa.tistory.com/