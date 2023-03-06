# Template Method Pattern

<!-- TOC -->

- [Template Method Pattern](#template-method-pattern)
    - [Template Method 란?](#template-method-%EB%9E%80)
    - [용도](#%EC%9A%A9%EB%8F%84)
    - [구현단계](#%EA%B5%AC%ED%98%84%EB%8B%A8%EA%B3%84)
    - [예제](#%EC%98%88%EC%A0%9C)
        - [with Java](#with-java)
    - [다른 패턴과의 관계](#%EB%8B%A4%EB%A5%B8-%ED%8C%A8%ED%84%B4%EA%B3%BC%EC%9D%98-%EA%B4%80%EA%B3%84)
    - [고려할 점](#%EA%B3%A0%EB%A0%A4%ED%95%A0-%EC%A0%90)
        - [실용주의 디자인 패턴](#%EC%8B%A4%EC%9A%A9%EC%A3%BC%EC%9D%98-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)
        - [패턴을 활용한 리팩토링](#%ED%8C%A8%ED%84%B4%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-%EB%A6%AC%ED%8C%A9%ED%86%A0%EB%A7%81)

<!-- /TOC -->

<br>

<img src="https://www.visualdive.com/wp-content/uploads/2022/01/211223_%EC%A7%84%EC%A7%9C-%EB%A7%9B%EC%9E%88%EB%8A%94-%EB%90%9C%EC%B0%8C-%EB%A0%88%EC%8B%9C%ED%94%BC-819x1024.png" width="60%">

<br>

## Template Method 란?

> 템플릿 메서드는 '알고리즘에서 불변적인 부분은 한 번만 구현하고 가변적인 동작은 서브클래스에서 구현할 수 있도록 남겨둔 것'을 말한다. -패턴을 활용한 리팩토링- 

- 알고리즘을 구 성하는 메서드 목록과 그 호출 순서
- 서브클래스가 꼭 오버라이드해야 할 추상 메서드
- 서브클래스가 오버라이드해도 되는 훅 메서드 (=구체 메서드)

<br>

## 용도

> 템플릿 메서드 패턴은 프레임워크, 라이브러리, API 등 다양한 상황에서 다양한 형태로 재사용할 수 있는 공통 알고리즘이 필요한 많은 애플리케이션에서 사용된다.

- 웹 프레임워크
    - Django 및 Ruby on Rails 와 같은 프레임워크에서는 템플릿 메서드 패턴을 사용하여 요청-응답 주기를 정의
    - base class 는 HTTP 요청을 처리하는 단계의 순서를 정의
    - subclass 는 다양한 유형의 요청을 처리하는 방법에 대한 구체적인 구현을 제공
- 게임 엔진
    - 게임 루프를 정의하는데 사용
    - base class 는 게임의 각 프레임에서 발생하는 이벤트의 순서를 정의하는 메서드 집합 제공
    - subclass 는 사용자 입력을 처리하거나 게임 엔티티를 업데이트하는 방법에 대한 구체적인 구현 제공
- 데이터 처리 프레임워크
    - Apache Hadoop 과 같은 데이터 처리 프레임워크에서 MapReduce 알고리즘을 정의하는 데 사용

> 고정된 알고리즘의 단계를 정의해야 하지만, 하위 클래스에 따라 일부 단계를 사용자 정의하거나 다르게 구현할 수 있는 많은 소프트웨어 애플리케이션에서 사용된다.

- 예시
    - javascript 의 JQuery ajax() HTTP 요청 처리
    - React 의 컴포넌트 라이프사이클 메서드인 componentDidMount(), ..
    - Spring 의 DB 접근 기술인 jdbcTemplate, HTTP 요청을 처리하는 dispatcherServlet

<br>

## 구현단계

<img src="https://refactoring.guru/images/patterns/diagrams/template-method/structure-indexed.png" width="70%">

1. `Abstract Class`는 알고리즘의 단계들의 역할을 하는 메서드들을 선언하며, 이러한 메서드를 특정 순서로 호출하는 실제 템플릿 메서드도 선언합니다. 단계들은 abstract로 선언되거나 일부 디폴트 구현을 갖습니다.

2. `Concrete Class` 들은 모든 단계들을 오버라이드할 수 있지만 템플릿 메서드 자체는 오버라이드 할 수 없습니다.

<br>

## 예제

### with Java

> 음료를 만드는 방법 예제

```java
public class Coffee {
    // 커피 만드는 방법

    void prepareRecipe() {
        boilWater();
        brewCoffeeGrinds();
        pourInCup();
        addSugarAndMilk();
    }

    public void boilWater() {
        System.out.println("물 끓이는 중");
    }
    public void brewCoffeeGrinds() {
        System.out.println("필터를 통해서 커피를 우려내는 중");
    }
    public void pourInCup() {
        System.out.println("컵에 따르는 중");
    }
    public void addSugarAndMilk() {
        System.out.println("설탕과 우유를 추가하는 중");
    }
}
```

```java
public class Tea {
    // 티를 만드는 방법
    void prepareRecipe() {
        boilWater();
        steepTeaBag();
        pourInCup();
        addLemon();
    }

    public void boilWater() {
        System.out.println("물 끓이는 중");
    }
    public void steepTeaBag() {
        System.out.println("차를 우려내는 중");
    }
    public void pourInCup() {
        System.out.println("컵에 따르는 중");
    }
    public void addLemon() {
        System.out.println("레몬을 추가하는 중");
    }
}
```

템플릿 메소드 패턴은 위의 두 클래스를 다음과 같이 리팩토링한다.

- 공통적인 부분을 뽑아 추상 클래스로 만든다.
    - 알고리즘의 세부 항목에서 차이가 있는 곳은 추상 메소드로 재정의

```java
public abstract class CaffeineBeverage {
    // 알고리즘을 갖고 있는 이 메소드를 '템플릿 메소드'라 부른다
    final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    abstract void brew();           // 서브클래스에서 구현
    abstract void addCondiments();  // 서브클래스에서 구현

    void boilWater() {
        System.out.println("물 끓이는 중");
    }
    void pourInCup() {
        System.out.println("컵에 따르는 중");
    }
}
```
```java
public class Coffee extends CaffeineBeverage {
    public void brew() {
        System.out.println("필터로 커피를 우려내는 중");
    }
    public void addCondiments() {
        System.out.println("설탕과 커피를 추가하는 중");
    }
}
```
```java
public class Tea extends CaffeineBeverage {
    public void brew() {
        System.out.println("차를 우려내는 중");
    }
    public void addCondiments() {
        System.out.println("레몬을 추가하는 중");
    }
}
```
- 음료의 종류에 따라 `brew()`, `addCondiments()` 를 재정의하고 있다.

<br>

## 다른 패턴과의 관계

- `팩토리 메서드`는 `템플릿 메서드`의 특수화라고 생각 할 수 있다.
- `템플릿 메서드`는 클래스 수준에서 작동하므로 정적이다. `전략 패턴`은 객체 수준에서 작동하므로 런타임에 행동들을 전환할 수 있도록 한다.

## 고려할 점

### 실용주의 디자인 패턴

- Template Method 패턴은 가능한 절제해 사용해야 한다. 클래스 자체가 전적으로 파생 클래스의 재정의에 의존하는 일종의 '프레임워크'가 되면 매우 부서지기 쉽기 때문이다. 기반 클래스는 매우 깨지기 쉽다. 
- 나는 MFC에서 프로그래밍을 할 때, 마이크로소프트가 새로운 버전을 릴리즈할 때마다 전체 애플리케이션을 재작성해야만 했던 악몽을 떨쳐버릴 수가 없다. 종종 코드는 잘 컴파일되지만, 몇몇 기반 클래스의 메서드가 변경되어 프로그램이 제대로 실행되지 않았던 것이다.

> Template Method 패턴은 또한 '이디엄'과 '패턴' 사이가 얼마나 가까울 수 있는지를 보여주는 예이기도 하다. Template Method 패턴은 다형성을 조금 응용했을 뿐 패턴이란 영광의 타이틀을 쓰기엔 부족하다고 주장할 수도 있는 것이다.

<br>

### 패턴을 활용한 리팩토링

- Template Method 패턴을 구현할 때에 실무적인 주의사항이 하나 있는데, 서브클래스에서 오버라이드해야 하는 메서드가 너무 많으면 곤란하다는 것이다. 서브클래스를 구현하기가 어려워지기 때문이다.

- 그래서 `Design Patterns` 에서는 서브클래스에서 오버라이드해야 하는 추상 메서드의 개수를 최소화해야 한다고 지적한다. 그러지 않으면 템플릿 메서드의 내용을 자세히 살펴보지 않고는 프로그래머가 어떤 메서드를 오버라이드해야 할지 쉽게 알 수 없을 것이다.

- Java 같은 프로그래밍 언어에서는 템플릿 메서드를 final 로 선언해 서브클래스가 실수로 오버라이드하는 것을 예방할 수도 있다. 단, 이런 방법은 클라이언트 코드에서 템플릿 메서드의 불변적인 부분을 전혀 변경할 필요가 없는 것이 확실할 때에만 사용해야 한다.