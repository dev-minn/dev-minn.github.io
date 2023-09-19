---
title:  "Chapter 04. 팩토리 패턴" 

categories:
  -  Design Pattern
tags:
  - [Design Pattern, Java]

toc: true
toc_sticky: true

date: 2023-09-15
---


## 📌 팩토리패턴

> 💡 <b>객체지향 빵 굽기</b>
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
- 팩토리 메서드와 생산자 클래스는 항상 추상으로 선언할 필요는 없음
- 간단한 구상 제품은 기본 팩토리 메소드를 정의해서 Creator 의 서브클래스 없이 만들 수 있음

<br>

> 💡 <b>구상클래스(new)</b>
> - new 를 사용하면 구상 클래스의 인스턴스가 만들어짐
> - 구상 클래스를 바탕으로 코딩하면 나중에 코드를 수정해야 할 가능성이 커지고, 유연성이 떨어짐
{: .notice--info}

<br>

## 📌 예제

> 💡 <b>예제 - 팩토리 메서드 패턴</b>
>
>  - 피자 주문하기
{: .notice--info}


<br>

### 피자주문

> 피자주문 메서드

```java
Pizza orderPizza(String type) {

    Pizza pizza;

    /**
     * 피자 종류를 파탕으로 올바른 구상 클래스의 인스턴스를 만들고
     * pizza 인스턴스 변수에 그 인스턴스를 대입
     * 여기에 있는 모든 피자 클래스는 Pizza 인터페이스를 구현
     * 
     * -> 이 부분이 문제가 되는 부분
     * -> 이 부분이 변경되는 부분이기 때문
     */  
    if(type.equals("cheese")) {
        pizza = new CheesePizza();
    } else if(type.eqals("greek")) {
        pizza = new GreekPizza();
    } else if(type.eqals("pepperoni")) {
        pizza = new PepproniPizza();
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
    
    return pizza;
}
```

<br>

> 💡 <b>객체 생성 부분 캡슐화하기</b>
>
>  - 객체 생성을 처리하는 클래스를 팩토리(Factory)라고 부름
>  - SimplePizzaFactory 를 만들고 나면 orderPizza() 메소드는 새로 만든 객체의 클라이언트가 됨
>     - 새로 만든 객체를 호출
>     - 피자가 필요할 때마다 피자 공장에 피자 하나를 만들어 달라고 부탁한다고 생각
{: .notice--info}

<br>

### 피자주문 리팩토링

#### SETEP 1. 객체 생성 팩토리 만들기

```java
public class PizzaStore {

    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
    
}
```

#### STEP 2. 피자 주문

```java
public class PizzaStore {

    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
    
}
```

> 구조

<img src="/assets/images/04001.png" witdh="600" height="400">

<br>

> 💡 <b>Tip</b>
>
> 디자인 패턴을 얘기할 때, '인터페이스를 구현한다' 라는 표현이 나온다고 해서<br>
> 항상 "클래스를 선언하는 부분에 implements 키워드를 써서 어떤 자바 인터페이스를 구현하는 클래스를 만든다" 라고 생각하면 안됨<br>
> 일반적으로 상위 형식(클래스와 인터페이스)에 있는 구상 클래스는 그 상위 형식의 '인터페이스를 구현하는' 클래스라고 생각하면 됨
{: .notice--info}

<br>

### 피자가게 프레임워크 만들기 -> 서브클래스에서 결정하도록 만들기

#### STEP 1. 추상 클래스 생성

> PizzaStore 추상 클래스에 피자 생성 추상 메서드로 선언하고, 지역별 스타일에 맞게 PizzaStore 의 서브클래스를 만듬 

```java
public abstract class PizzaStore {

    public Pizza orderPizza(String type) {
        
        // 팩토리 객체가 아닌 PizzaStroe 에 있는 orderPizza 호출
        Pizza pizza = createPizza(type);

        System.out.println("===== Making a " + pizza.getName() + " =====");
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }

    // 팩토리 객체 대신 이 메서드 사용
    public abstract Pizza createPizza(String item);
}
```

#### STEP 2. 서브클래스 생성

> PizzaStore 의 서브클래스를 만들고 지역별 특성에 맞게 createPizza() 메서드 구현

```java
public class NYPizzaStore extends PizzaStore {
    
    public Pizza createPizza(String item) {
		if (item.equals("cheese")) {
			return new NYStyleCheesePizza();
		} else if (item.equals("veggie")) {
			return new NYStyleVeggiePizza();
		} else if (item.equals("clam")) {
			return new NYStyleClamPizza();
		} else if (item.equals("pepperoni")) {
			return new NYStylePepperoniPizza();
		} else return null;
	}
}

public class ChicagoPizzaStore extends PizzaStore {

	public Pizza createPizza(String item) {
        if (item.equals("cheese")) {
                return new ChicagoStyleCheesePizza();
        } else if (item.equals("veggie")) {
                return new ChicagoStyleVeggiePizza();
        } else if (item.equals("clam")) {
                return new ChicagoStyleClamPizza();
        } else if (item.equals("pepperoni")) {
                return new ChicagoStylePepperoniPizza();
        } else return null;
	}
}
```


#### STEP 3. 지역별 피자 스타일 클래스 생성

> PizzaStore 별로 피자 스타일이 다름

```java
public class NYStyleCheesePizza extends Pizza {
    
    public NYStyleCheesePizza() {
      name = "NY Style Sauce and Cheese Pizza";
      dough = "Thin Crust Dough";
      sauce = "Marinara Sauce";
  
      toppings.add("Grated Reggiano Cheese");
    }
}

public class ChicagoStyleCheesePizza extends Pizza {

	public ChicagoStyleCheesePizza() { 
		 name = "Chicago Style Deep Dish Cheese Pizza";
		 dough = "Extra Thick Crust Dough";
		 sauce = "Plum Tomato Sauce";
 
		 toppings.add("Shredded Mozzarella Cheese");
	}
 
	public void cut() {
		System.out.println("Cutting the pizza into square slices");
	}
}
```

#### STEP 4. 피자 클래스 생성

> 피자 클래스

```java
public class Pizza {
    String name;
    String dough;
    String sauce;
    List<String> toppings = new ArrayList<String>();

    public String getName() {
        return name;
    }

    public void prepare() {
        System.out.println("[Prepare]");
    }
    
    public void bake() {
        System.out.println("[Bake for 25 minutes at 350]");
    }

    public void cut() {
        System.out.println("[Cut the pizza into diagonal slices]");
    }

    public void box() {
        System.out.println("[Place pizza in official PizzaStore box]");
    }

    public String toString() {
        StringBuffer display = new StringBuffer();
        display.append("====== " + name + " ======\n");
        display.append(dough + "\n");
        display.append(sauce + "\n");
        for(String topping : toppings) {
            display.append(topping + "\n");
        }
        return display.toString();
    }
}
```

#### STEP 5. 피자 주문

- 피자 주문

```java
public class Client {
    
    public static void main(String[] args) {

        PizzaStore nyStore = new NYPizzaStore();
        PizzaStore chicagoStore = new ChicagoPizzaStore();

        Pizza pizza = new Pizza();

        System.out.println("--------------------------------------------");
        System.out.println("Ethan ordered a " + pizza.getName() + "\n");
        pizza = nyStore.orderPizza("cheese");
        System.out.println("--------------------------------------------");
        
        pizza = chicagoStore.orderPizza("cheese");
        System.out.println("Juri ordered a " + pizza.getName() + "\n");
        System.out.println("--------------------------------------------");
    }
}
```

> 실행결과

```
--------------------------------------------
Ethan ordered a null

===== Making a NY Style Sauce and Cheese Pizza =====
[Prepare]
[Bake for 25 minutes at 350]
[Cut the pizza into diagonal slices]
[Place pizza in official PizzaStore box]
--------------------------------------------
===== Making a Chicago Style Deep Dish Cheese Pizza =====
[Prepare]
[Bake for 25 minutes at 350]
Cutting the pizza into square slices
[Place pizza in official PizzaStore box]
Juri ordered a Chicago Style Deep Dish Cheese Pizza

--------------------------------------------
[Prepare]
[Bake for 25 minutes at 350]
[Cut the pizza into diagonal slices]
[Place pizza in official PizzaStore box]
Hun ordered a Chicago Style Deep Dish Cheese Pizza

--------------------------------------------
```

<br>

- 병렬 클래스 계층구조

<img src="/assets/images/04002.png" witdh="600" height="400">

<br><br>

## 📌 추상 팩토리 패턴

- 구상 클래스에 의존하지 않고도 서로 연관되거나 객체로 이루어진 제품군을 생성하는 인터페이스를 제공
- 구상 클래스는 서브클래스에서 만듬

<img src="/assets/images/04005.png" witdh="600" height="400">


> 💡 <b>디자인 원칙</b>
> - 의존성 뒤집기 원칙(Dependency Inversion Principle)
> - 추상화된 것에 의존하게 만들고 구상 클래스에 의존하지 않게 만듬
{: .notice--info}

<br>

###  의존성 뒤집기 원칙을 지키는 방법

- 변수에 구상 클래스의 레퍼런스를 저장하지 않아야 함
  - new 연산자를 사용하면 구상 클래스의 레퍼런스를 사용하게 됨 -> 팩토리를 써서 구상 클래스의 레퍼런스를 변수에 저장하는 일을 미리 방지해야 함
- 구상 클래스에서 유도된 클래스를 만들지 말아야 함
  - 구상 클래스에서 유도된 클래스를 만들면 특정 구상 클래스에 의존하게 됨
  - 인터페이스나 추상 클래스 처럼 추상화된 것으로부터 클래스를 만들어야 함
- 베이스 클래스에 이미 구현되어 있는 메소드를 오버라이드 하지 말아야 함
  - 이미 구현되어 있는 메소드를 오버라이드 한다면 베이스 클래스가 제대로 추상화되지 않음
  - 베이스 클래스에서 메소드를 정의할 때는 모든 서브클래스에서 공유할 수 있는 것만 정의해야 함

<br><br>

## 📌 예제

> 💡 <b>예제</b>
>
> - 원재료를 생산하는 공장을 만들고 지점까지 재료를 배달 후 피자 생성
> - 지역별로 팩토리를 만듬. 각 생성 메소드를 구현하는 PizzaIngredientFactory 클래스를 만들어야 함
>   - ReggianoCheese, RedPeppers, ThickCrustDough 와 같이 팩토리에서 사용할 원재료 클래스를 구현. 상황에 따라 서로 다른 지역에서 같은 재료 클래스를 쓸 수도 있음
>   - 그리고 나서 새로 만든 원재료 팩토리를 PizzaStore 코드에서 사용하도록 모든 것을 하나로 묶어야 함
{: .notice--info}


### STEP 1. 팩토리용 인터페이스 정의

> 모든 원재료를 생산하는 팩토리용 인터페이스 정의

```java
public interface PizzaIngredientFactory {
    public Dough createDough();
	public Sauce createSauce();
	public Cheese createCheese();
	public Veggies[] createVeggies();
	public Pepperoni createPepperoni();
	public Clams createClam();
}
```

### STEP 2. 지역별 원재료 팩토리 구현

> 뉴욕 원재료 팩토리 구현

```java
public class NYPizzaIngredientFactory implements PizzaIngredientFactory{

    public Dough createDough() {
		return new ThinCrustDough();
	}
 
	public Sauce createSauce() {
		return new MarinaraSauce();
	}
 
	public Cheese createCheese() {
		return new ReggianoCheese();
	}
 
	public Veggies[] createVeggies() {
		Veggies veggies[] = { new Garlic(), new Onion(), new Mushroom(), new RedPepper() };
		return veggies;
	}
 
	public Pepperoni createPepperoni() {
		return new SlicedPepperoni();
	}

	public Clams createClam() {
		return new FreshClams();
	}
    
}
```

> 시카고 원재료 팩토리 구현

```java
public class ChicagoPizzaIngredientFactory implements PizzaIngredientFactory{
    
    public Dough createDough() {
		return new ThickCrustDough();
	}

	public Sauce createSauce() {
		return new PlumTomatoSauce();
	}

	public Cheese createCheese() {
		return new MozzarellaCheese();
	}

	public Veggies[] createVeggies() {
		Veggies veggies[] = { new BlackOlives(), 
		                      new Spinach(), 
		                      new Eggplant() };
		return veggies;
	}

	public Pepperoni createPepperoni() {
		return new SlicedPepperoni();
	}

	public Clams createClam() {
		return new FrozenClams();
	}
}
```

### STEP 3. 팩토리에서 생산한 원재료만 사용

> Pizza 클래스가 팩토리에서 생산한 원재료만 사용

```java
public abstract class Pizza {
    String name;

	Dough dough;
	Sauce sauce;
	Veggies veggies[];
	Cheese cheese;
	Pepperoni pepperoni;
	Clams clam;

	abstract void prepare();

	void bake() {
		System.out.println("Bake for 25 minutes at 350");
	}

	void cut() {
		System.out.println("Cutting the pizza into diagonal slices");
	}

	void box() {
		System.out.println("Place pizza in official PizzaStore box");
	}

	void setName(String name) {
		this.name = name;
	}

	String getName() {
		return name;
	}

	public String toString() {
		StringBuffer result = new StringBuffer();
		result.append("---- " + name + " ----\n");
		if (dough != null) {
			result.append(dough);
			result.append("\n");
		}
		if (sauce != null) {
			result.append(sauce);
			result.append("\n");
		}
		if (cheese != null) {
			result.append(cheese);
			result.append("\n");
		}
		if (veggies != null) {
			for (int i = 0; i < veggies.length; i++) {
				result.append(veggies[i]);
				if (i < veggies.length-1) {
					result.append(", ");
				}
			}
			result.append("\n");
		}
		if (clam != null) {
			result.append(clam);
			result.append("\n");
		}
		if (pepperoni != null) {
			result.append(pepperoni);
			result.append("\n");
		}
		return result.toString();
	}
}
```

### STEP 4. 지역별로 원재료 팩토리에서 처리

```java
public class CheesePizza extends Pizza{
    PizzaIngredientFactory ingredientFactory;
 
	public CheesePizza(PizzaIngredientFactory ingredientFactory) {
		this.ingredientFactory = ingredientFactory;
	}
 
	void prepare() {
		System.out.println("Preparing " + name);
		dough = ingredientFactory.createDough();
		sauce = ingredientFactory.createSauce();
		cheese = ingredientFactory.createCheese();
	}
}

public class ClamPizza extends Pizza {
	PizzaIngredientFactory ingredientFactory;
 
	public ClamPizza(PizzaIngredientFactory ingredientFactory) {
		this.ingredientFactory = ingredientFactory;
	}
 
	void prepare() {
		System.out.println("Preparing " + name);
		dough = ingredientFactory.createDough();
		sauce = ingredientFactory.createSauce();
		cheese = ingredientFactory.createCheese();
		clam = ingredientFactory.createClam();
	}
}
```

### STEP 5. 각 지역별로 재료 공장의 레퍼런스 전달

```java
public class NYPizzaStore extends PizzaStore {
 
	protected Pizza createPizza(String item) {
		Pizza pizza = null;
		PizzaIngredientFactory ingredientFactory = 
			new NYPizzaIngredientFactory();
 
		if (item.equals("cheese")) {
  
			pizza = new CheesePizza(ingredientFactory);
			pizza.setName("New York Style Cheese Pizza");
  
		} else if (item.equals("veggie")) {
 
			pizza = new VeggiePizza(ingredientFactory);
			pizza.setName("New York Style Veggie Pizza");
 
		} else if (item.equals("clam")) {
 
			pizza = new ClamPizza(ingredientFactory);
			pizza.setName("New York Style Clam Pizza");
 
		} else if (item.equals("pepperoni")) {

			pizza = new PepperoniPizza(ingredientFactory);
			pizza.setName("New York Style Pepperoni Pizza");
 
		} 
		return pizza;
	}
}

public class ChicagoPizzaStore extends PizzaStore {

	protected Pizza createPizza(String item) {
		Pizza pizza = null;
		PizzaIngredientFactory ingredientFactory =
		new ChicagoPizzaIngredientFactory();

		if (item.equals("cheese")) {

			pizza = new CheesePizza(ingredientFactory);
			pizza.setName("Chicago Style Cheese Pizza");

		} else if (item.equals("veggie")) {

			pizza = new VeggiePizza(ingredientFactory);
			pizza.setName("Chicago Style Veggie Pizza");

		} else if (item.equals("clam")) {

			pizza = new ClamPizza(ingredientFactory);
			pizza.setName("Chicago Style Clam Pizza");

		} else if (item.equals("pepperoni")) {

			pizza = new PepperoniPizza(ingredientFactory);
			pizza.setName("Chicago Style Pepperoni Pizza");

		}
		return pizza;
	}
}
```

### STEP 6. 피자 주문

```java
public class Client {
    public static void main(String[] args) {
		PizzaStore nyStore = new NYPizzaStore();
		PizzaStore chicagoStore = new ChicagoPizzaStore();
 
		Pizza pizza = nyStore.orderPizza("cheese");
		System.out.println("Ethan ordered a " + pizza + "\n");
 
		pizza = chicagoStore.orderPizza("cheese");
		System.out.println("Joel ordered a " + pizza + "\n");

		pizza = nyStore.orderPizza("clam");
		System.out.println("Ethan ordered a " + pizza + "\n");
 
		pizza = chicagoStore.orderPizza("clam");
		System.out.println("Joel ordered a " + pizza + "\n");

		pizza = nyStore.orderPizza("pepperoni");
		System.out.println("Ethan ordered a " + pizza + "\n");
 
		pizza = chicagoStore.orderPizza("pepperoni");
		System.out.println("Joel ordered a " + pizza + "\n");

		pizza = nyStore.orderPizza("veggie");
		System.out.println("Ethan ordered a " + pizza + "\n");
 
		pizza = chicagoStore.orderPizza("veggie");
		System.out.println("Joel ordered a " + pizza + "\n");
	}
}
```

> 구조

<img src="/assets/images/04003.png" witdh="600" height="400">

<br><br>

## 📌 팩토리 메서드 패턴과 추상 팩토리 패턴

- 두 패턴 모두 객체를 만드는 일을 함
- 객체 생성을 캡슐화하는 패턴
- 클라이언트와 구상 클래스가 서로 분리된 유연한 디자인을 구현할 수 있게 도와줌
- <b>팩토리 메서드 패턴</b>
  - 상속으로 객체를 만듬
  - 객체를 생성할 때 클래스를 확장하고 팩토리 메서드를 오버라이드
  - 팩토리 메서드 패턴을 사용하는 이유는 서브클래스로 객체를 만들려는 것
    - 구상 형식을 서브 클래스에서 처리해 주니까, 자신이 사용할 추상 형식만 알면 됨 -> 클라이언트와 구상 형식을 분리하는 역할
  - 클라이언트 코드와 인스턴스를 만들어야 할 구상 클래스를 분리시켜야 할 때 사용
  - 서브클래스를 만들고 팩토리 메서드를 구현하기만 하면 됨
- <b>추상 팩토리 패턴</b>
  - 객체 구성으로 만듬
  - 제품군을 만드는 추상 형식 제공
  - 제품이 생산되는 방법은 이 형식의 서브클래스에서 정의
  - 팩토리를 사용하고 싶으면 일단 인스턴스를 만든 다믕 추상 형식을 써서 만든 코드에 전달하면 됨
  - 새로운 제품군을 추가하려면 인터페이스를 바꿔야 함
  - 클라이언트에서 서로 연관된 일련의 제품을 만들어야 할 때, 즉 제품군은 만들어야 할 때 사용

<img src="/assets/images/04006.png" witdh="600" height="400">

<br>

## 📌 정리

✅ 팩토리를 쓰면 객체 생성을 캡슐화할 수 있음<br>
✅ 간단한 팩토리는 엄밀하게 말해서 디자인 패턴은 아니지만, 클라이언트와 구상 클래스의 분리하는 간단한 기법으로 활용할 수 있음<br>
✅ 팩토리 메서드 패턴은 상속을 활용. 객체 생성을 서브클래스에게 맡기고 서브클래스는 팩토리 메서드를 구현해서 객체를 생산<br>
✅ 추상 팩토리 패턴은 객체 구성을 활용. 팩토리 인터페이스에서 선언한 메서드에서 객체 생성이 구현<br>
✅ 모든 팩토리 패턴은 애플리케이션의 구상 클래스 의존성을 줄여줌으로써 느슨한 결합을 도와줌<br>
✅ 팩토리 메서드 패턴은 특정 클래스에서 인스턴스를 만드는 일을 서브클래스에게 넘김<br>
✅ 추상 팩토리 패턴은 구상 클래스에 직접 의존하지 않고도 서로 관련된 객체로 이루어진 제품군을 만드는 용도로 쓰임<br>
✅ 의존성 뒤집기 원칙을 따르면 구상 형식 의존을 피하고 추상화를 지향할 수 있음<br>
✅ 팩토리는 구상 클래스가 아닌 추상 클래스와 인터페이스에 맞춰서 코딩할 수 있게 해 주는 강력한 기법<br>

<br><br><br>

## 📌 참고

>  출처
- 헤드퍼스트 디자인 패턴