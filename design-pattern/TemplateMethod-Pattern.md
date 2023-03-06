# Template Method Pattern

<!-- TOC -->

- [Template Method Pattern](#template-method-pattern)
    - [Template Method 란?](#template-method-%EB%9E%80)
    - [용도](#%EC%9A%A9%EB%8F%84)
    - [구현단계](#%EA%B5%AC%ED%98%84%EB%8B%A8%EA%B3%84)
    - [구현](#%EA%B5%AC%ED%98%84)
        - [with Java](#with-java)

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

> 고정된 단계 순서를 정의해야 하지만, 하위 클래스에 따라 일부 단계를 사용자 정의하거나 다르게 구현할 수 있는 많은 소프트웨어 애플리케이션에서 사용된다.

<br>

## 구현단계

1. 서브클래스가 구현할 수 있는 하나 이상의 추상 메서드와 함께 전체 알고리즘을 포함하는 추상 베이스 클래스를 정의
2. 서브클래스는 추상 메서드를 재정의하여 구체적인 구현을 제공
3. 베이스 클래스는 알고리즘의 구체적인 구현을 제공하며, 서브클래스가 구현하는 추상 메서드를 호출
4. 클라이언트는 알고리즘을 실행하고 추상 메서드의 구현을 하위 클래스에 위임하는 기본 클래스의 인스턴스에서 템플릿 메서드를 호출하여 패턴을 사용

<br>

## 구현

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
