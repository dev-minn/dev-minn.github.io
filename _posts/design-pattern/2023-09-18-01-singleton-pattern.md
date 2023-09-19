---
title:  "[헤드퍼스트 디자인패턴] Chapter 05. 싱글톤 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern]

toc: true
toc_sticky: true

date: 2023-09-18
---


## 📌 싱글톤패턴

> 💡 <b>하나뿐인 특별한 객체 만들기</b>
{: .notice--info}

<br>

- 클래스 인스턴스를 하나만 만들고, 그 인스턴스로의 전역 접근을 제공함
- 실제로 적용할 때는 클래스에서 하나뿐인 인스턴스를 관리하도록 만들면 됨
- 다른 어떤 클래스에서도 자신의 인스턴스를 추가로 만들지 못하게 해야 함
- 인스턴스가 필요하다면 반드시 클래스 자신을 거치도록 해야함
- 어디서든 그 인스턴스에 접근할 수 있도록 전역 접근 지점을 제공함
- 언제든 이 인스턴스가 필요하면 클래스에 요청할 수 있게 만들어 놓고, 요청이 들어오면 그 하나뿐인 인스턴스를 건네주도록 만들어야 함
- 자원을 많이 잡아먹는 인스턴스가 있다면 싱클톤 패턴 기법이 유용

<br>

## 📌 사용예시

- 스레드 풀, 캐시, 대화상자, 사용자 설정 등 레지스트리 설정을 처리하는 객체
- 로그 기록용 객체
- 프린터나 그래픽 카드 같은 디바이스를 위한 디바이스 드라이버 등

<br>

<img src="/assets/images/05001.png" witdh="600" height="400">

<br>

## 📌 멀티스레딩 문제 발생

> 💡 <b>예제</b>
> - 2개의 스레드에서 아래의 코드를 실행
{: .notice--info}

```java
ChocolateBoiler boiler = ChocolateBoiler.getInstance();
boiler2.fill();
boiler2.boil();
boiler2.drain();
```

<img src="/assets/images/05002.png" witdh="600" height="400">

<br>

> getInstance() 를 동기화하면 멀티스레딩과 관련된 문제가 해결됨 -> 하지만, 속도 이슈 발생

```java
public class Singleton {

    private static Singleton uniqueInstance;

    // 기타 인스턴스 변수

    public Singleton() {}

    public static synchronized Singleton getInstance() {
        if(uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }

    // 기타 메서드
}
```

<br>

## 📌 더 효율적으로 멀티스레딩 문제 해결하기

### 방법 1. getInstance() 의 속도가 그리 중요하지 않다면 그냥 둠

### 방법 2. 인스턴스가 필요할 때는 생성하지 말고 처음부터 만듬

```java
public class Singleton {

    /*
     * 정적 초기화 부분 (static initializer) 에서 Singleton 의 인스턴스를 생성함
     * 이러면 스레드를 써도 문제가 없음
     */ 
    private static Singleton uniqueInstance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {

        // 인스턴스는 이미 있으니깐 리턴만 하면 됨
        return uniqueInstance;
    }
}
```

### 방법 3. 'DCL' 을 써서 getInstance() 에서 동기화되는 부분을 줄임

- DCL(Double-Checked Locking) 을 사용하면 인스턴스가 생성되어 있는지 확인한 다음 생성되어 있지 않을때만 동기화할 수 있음
- 처음에만 동기화하고 나중에는 동기화하지 않아도 됨

```java
public class Singleton {

    /*
     * volatile 키워드를 사용하면 멀티스레딩을 쓰더라도 
     * uniqueInstance 변수가 Singleton 인스턴스로 초기화되는 과정이 올바르게 진행됨 
     */
    private volatile static Singleton uniqueInstance;

    private Singleton() {}

    public static Singleton getInstance() {

        // 인스턴스가 있는지 확인하고, 없으면 동기화된 블록으로 들어감
        if(uniqueInstance == null) {
            synchronizied (Singleton.class) {
                // 블록에서도 다시 한 번 변수가 null 인지 확인한 다음 인스턴스를 생성함
                if(uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
    }
}
```

> 💡 <b>전역변수가 싱글톤보다 나쁜 이유?</b>
>
> - 자바의 전역 변수는 객체의 정적 레퍼런스임. 전역 변수를 이런 식으로 사용하는 데는 단점이 있음
>   - 게으른 인스턴스를 생성할 수 없고, 처음부터 끝까지 인스턴스를 가지고 있어야 함
>   - 전역변수를 사용하다 보면 간단한 객체에 대한 전역 레퍼런스를 자꾸 만들게 되면서 네임 스페이스를 지저분하게 만드는 경향이 생기게 됨
{: .notice--info}

<br>

## 📌 enum 사용

- 동기화 문제, 클래스 로딩 문제, 리플렉션, 직렬화와 역직렬화 문제 등은 ENUM 으로 싱글톤을 생성해서 해결할 수 있음

```java
public enum Singleton {
    UNIQUE_INSTANCE;        
    
    // 기타 필요한 필드

    public String getDescription() {
		return "I'm a thread safe Singleton!";
	}
}

public class SingletonClient {
    public static void main(String[] args) {
        Singleton singleton = Singleton.UNIQUE_INSTANCE;
        System.out.println(singleton.getDescription());
        // 여기서 싱글톤 사용
    }
}
```

<br>

## 📌 예제

### 초콜릿 보일러

```java
public class ChocolateBoiler {

    private boolean empty;
    private boolean boiled;
    
    ChocolateBoiler() {
        empty = true;
        boiled = false;
    }

    public void fill() {
		if (isEmpty()) {
			empty = false;
			boiled = false;
			// fill the boiler with a milk/chocolate mixture
		}

		System.out.println("empty : " + empty + ", boil : " + boiled);
	}
 
	public void drain() {
		if (!isEmpty() && isBoiled()) {
			// drain the boiled milk and chocolate
			empty = true;
		}

		System.out.println("empty : " + empty + ", boil : " + boiled);
	}
 
	public void boil() {
		if (!isEmpty() && !isBoiled()) {
			// bring the contents to a boil
			boiled = true;
		}

		System.out.println("empty : " + empty + ", boil : " + boiled);
	}
  
	public boolean isEmpty() {
		return empty;
	}
 
	public boolean isBoiled() {
		return boiled;
	}

}
```

```java
public class ChocolateController {
    public static void main(String args[]) {
		ChocolateBoiler boiler = new ChocolateBoiler();
		boiler.fill();
        boiler.boil();
        boiler.drain();
	}
}
```

> 실행결과

```
empty : false, boil : false
empty : false, boil : true
empty : true, boil : true
```

### 초콜릿 보일러 -> 싱글톤패턴 리팩토링

```java
public class ChocolateBoiler {

    private boolean empty;
    private boolean boiled;
    private static ChocolateBoiler uniqueInstance;

    private ChocolateBoiler() {
        empty = true;
        boiled = false;
    }

    public static ChocolateBoiler getInstance() {
        if(uniqueInstance == null) {
            System.out.println("Creating unique instance of Chocolate Boiler");
            uniqueInstance = new ChocolateBoiler();
        }

        System.out.println("Returning instance of Chocolate Boiler");
        return uniqueInstance;
    }

    public void fill() {
		if (isEmpty()) {
			empty = false;
			boiled = false;
			// fill the boiler with a milk/chocolate mixture
		}
	}
 
	public void drain() {
		if (!isEmpty() && isBoiled()) {
			// drain the boiled milk and chocolate
			empty = true;
		}
	}
 
	public void boil() {
		if (!isEmpty() && !isBoiled()) {
			// bring the contents to a boil
			boiled = true;
		}
	}
  
	public boolean isEmpty() {
		return empty;
	}
 
	public boolean isBoiled() {
		return boiled;
	}

}
```

```java
public class ChocolateController {
    public static void main(String args[]) {
		// will return the existing instance
		ChocolateBoiler boiler = ChocolateBoiler.getInstance();
        boiler.fill();
		boiler.boil();
		boiler.drain();
	}
}
```

> 실행결과

```
Creating unique instance of Chocolate Boiler
Returning instance of Chocolate Boiler
```

### 초콜릿 보일러 -> enum 을 사용한 싱클톤패턴 리팩토링

```java
public enum ChocolateBoiler {
    INSTANCE;
    
    private boolean empty;
    private boolean boiled; 

    public boolean getEmpty() {
        return empty;
    }

    public void setEmpty(boolean empty) {
        this.empty = empty;
    }

    public boolean getBoiled() {
        return boiled;
    }

    public void setBoiled(boolean boiled) {
        this.boiled = boiled;
    }

    public void fill() {
		if (isEmpty()) {
			empty = false;
			boiled = false;
			// fill the boiler with a milk/chocolate mixture
		}
	}
 
	public void drain() {
		if (!isEmpty() && isBoiled()) {
			// drain the boiled milk and chocolate
			empty = true;
		}
	}
 
	public void boil() {
		if (!isEmpty() && !isBoiled()) {
			// bring the contents to a boil
			boiled = true;
		}
	}

    public boolean isEmpty() {
		return this.empty;
	}
 
	public boolean isBoiled() {
		return this.boiled;
	}
}
```

```java
public class ChocolateController {
    public static void main(String[] args) {
        ChocolateBoiler singleton = ChocolateBoiler.INSTANCE;

        
        System.out.println("singleton.getEmpty(): " + singleton.getEmpty());
        System.out.println("singleton.getBoiled(): " + singleton.getBoiled());

        singleton.setEmpty(true);
        singleton.setBoiled(false);

        System.out.println("singleton.getEmpty(): " + singleton.getEmpty());
        System.out.println("singleton.getBoiled(): " + singleton.getBoiled());

        singleton.fill();
        singleton.boil();
        singleton.drain();

        System.out.println("singleton.getEmpty(): " + singleton.getEmpty());
        System.out.println("singleton.getBoiled(): " + singleton.getBoiled());
    }
}
```

> 실행결과

```
singleton.getEmpty(): false
singleton.getBoiled(): false
singleton.getEmpty(): true
singleton.getBoiled(): false
singleton.getEmpty(): true
singleton.getBoiled(): true
```

<br>

## 📌 정리

✅ 어떤 클래스에 싱글톤 패턴을 적용하면 그 클래스의 인스턴스가 1개만 있도록 할 수 있음<br>
✅ 싱글톤 패턴을 사용하면 하나뿐인 인스턴스를 어디서든지 접근할 수 있도록 할 수 있음<br>
✅ 자바에서 싱글톤 패턴을 구현할 때는 private 생성자와 정적 메서드, 정적 변수를 사용함<br>
✅ 멀티 스레드를 사용하는 애플리케이션에서는 속도와 자원 문제를 파악해 보고 적절한 구현법을 사용해야 함<br>
✅ <b>(주의!) DCL 을 써서 구현하는 자바 5 이전에 나온 버전에서는 스레드 관련 문제가 생길 수 있음.</b><br>
✅ 클래스 로더가 여러 개 있으면 싱글톤이 제대로 작동하지 않고, 여러 개의 인스턴스가 생길 수 있음<br>
✅ 자바의 enum 을 쓰면 간단하게 싱글톤을 구현할 수 있음<br>


<br><br><br>

## 📌 참고

>  출처
- 헤드퍼스트 디자인 패턴