# Factory Pattern

<!-- TOC -->

- [Factory Pattern](#factory-pattern)
    - [Factory Method Pattern](#factory-method-pattern)
        - [용도](#%EC%9A%A9%EB%8F%84)
        - [사용하는 이유](#%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
        - [장단점](#%EC%9E%A5%EB%8B%A8%EC%A0%90)
        - [동작 방식](#%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D)
            - [with Java](#with-java)
            - [with Go](#with-go)
    - [추상 팩토리 패턴](#%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%ED%8C%A8%ED%84%B4)
        - [용도](#%EC%9A%A9%EB%8F%84)
        - [Factor Method Pattern 과의 차이점](#factor-method-pattern-%EA%B3%BC%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
        - [동작 방식](#%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D)
            - [with Java](#with-java)

<!-- /TOC -->

<br>

## Factory Method Pattern

> 부모 클래스에서 객체들을 생성할 수 있는 인터페이스를 제공하지만, 자식 클래스들이 생성될 객체들의 유형을 변경할 수 있도록 하는 생성 패턴

- 생성할 객체의 정확한 클래스를 지정하지않고 특정 유형의 객체를 생성하는데 팩토리 메서드를 사용
    - 팩토리 메서드를 사용하는 팩토리 메서드를 사용하는 코드를 변경하지 않고도 자식 클래스에서 팩토리 메서드를 재정의하여 다른 유형의 객체 생성 가능

<br>

### 용도

> 많은 유형의 객체가 있고, 그 생성을 유연한 방식으로 관리해야 하는 복잡한 시스템에서 유용

<br>

### 사용하는 이유

> 유연하고 캡슐화된 방식으로 객체를 생성하는 방법을 제공하는 디자인 패턴으로, 유지 관리가 용이하고 재사용 가능한 코드를 생성

1. 캡슐화
    - 객체 생성 프로세스를 캡슐화하여 나머지 코드에 영향을 주지않고 객체 생성 방식을 쉽게 변경
2. 유연성
    - 서브클래스가 팩토리 메서드를 재정의하고 다양한 유형의 객체를 생성할 수 있도록 허용
3. 재사용 가능성
    - 동일한 팩토리 메서드를 사용하여 여러 유형의 객체를 생성할 수 있도록 함으로써 코드 재사용 촉진
4. 추상화
    - 객체 생성을 나머지 코드에서 추상화할 수 있도록 하여 추상화를 촉진하므로 코드가 모듈화되고 유지관리 용이

<br>

### 장단점

- 장점
    - 단일 책임 원칙 (SRP)
    - 개방 폐쇄 원칙 (OCP)
- 단점
    - 복잡성
    - 오버헤드
    - 제한된 유연성


<br>

### 동작 방식 

<img src="https://refactoring.guru/images/patterns/diagrams/factory-method/structure-2x.png" width="80%">

#### with Java

```java
public interface ShapeFactory {
    public Shape createShape();
}
```
1. 객체를 생성할 팩토리 메서드에 대한 인터페이스를 정의한다.
    - `createShape()` 메서드는 팩토리 메서드의 실제 구현을 제공하는 구체적인 클래스에서 구현된다.

```java
public class CircleFactory implements ShapeFactory {
    public Shape createShape() {
        return new Circle();
    }
}
```
2. `ShapeFactory` 인터페이스를 구현하는 구체적인 클래스를 만들어 `Circle` 객체 생성
    - 이 클래스는 새로운 `Circle` 객체를 반환하는 `createShape()` 메서드를 구현한다.

```java
public class RectangleFactory implements ShapeFactory {
    public Shape createShape() {
        return new Rectangle();
    }
}
```
3. 2번과 마찬가지로 `Rectangle` 객체를 생성하는 구현클래스 생성

```java
public class Client {
    public static void main(String[] args) {
        ShapeFactory factory1 = new CircleFactory();
        Shape shape1 = factory1.createShape(); // Circle 객체 생성

        ShapeFactory factory2 = new RectangleFactory();
        Shape shape2 = factory2.createShape(); // Rectangle 객체 생성
    }
}
```
4. 인터페이스 구현 클래스를 사용하여 객체 생성

> `CircleFactory` 와 `RectangleFactory` 객체를 생성하고 이를 통해 각각 `Circle` 과 `Rectangle` 을 생성한다. Client 는 팩토리 메서드와만 상호 작용하며 생성되는 특정 클래스에 대해 알 필요가 없다. 이를 통해 코드의 유연성과 유지 관리성이 향상된다.

<br>

#### with Go

```go
type PizzaFactory interface {
    CreatePizza() Pizza
}
```
1. 객체를 생성할 팩토리 메서드에 대한 인터페이스 정의

```go
type CheesePizzaFactory struct{}

func (c CheesePizzaFactory) CreatePizza() Pizza {
    return &CheesePizza{}
}

type PepperoniPizzaFactory struct{}

func (p PepperoniPizzaFactory) CreatePizza() Pizza {
    return &Pepperoni{}
}
```
2. 인터페이스를 구현하는 구체적인 클래스 `CheesePizzaFactory`, `PepperoniPizzaFactory` 생성

```go
type Pizza interface {
    GetName() string
}

type CheesePizza struct{}

func (c CheesePizza) GetName() string {
    return "치즈 피자"
}

type Pepperoni struct{}

func (p Pepperoni) GetName() string {
    return "페페로니 피자"
}
```
3. 각 피자 유형을 나타내는 구체적인 클래스 생성

```go
func main() {
    var factory PizzaFactory
    var pizza Pizza

    factory = CheesePizzaFactory{}
    pizza = factory.CreatePizza() // 치즈 피자 만들기
    fmt.Println(pizza.GetName()) // "치즈 피자"

    factory = PepperoniPizzaFactory{}
    pizza = factory.CreatePizza() // 페페로니 피자 만들기
    fmt.Println(pizza.GetName()) // "페페로니 피자"
}
```
4. 구현한 클래스를 사용하여 팩토리 메서드 패턴 구현
    - Client 는 Factory 메서드와만 상호작용하며, 생성되는 특정 클래스인 `Pepperoni`, `Cheese` 에 대해서는 알 필요가 없다.

<br><br>

## 추상 팩토리 패턴

> 구체적인 클래스를 지정하지 않고도 관련 또는 종속 객체의 제품군을 생성하기 위한 인터페이스를 제공하는 디자인 패턴

- 시스템에서 다양한 관련 객체를 생성해야 하지만 해당 객체의 특정 구현에 코드를 연결하고 싶지 않을 때 유용하다.

<br>

### 용도

- 클라이언트 코드가 함께 작동하는 관련 객체 집합을 만들어야 하는 상황에서 자주 사용된다.
- 예시
    - 사용자 인터페이스에서 버튼, 메뉴, 텍스트 상자와 같은 컴포넌트 세트는 하나의 추상 팩토리로 생성될 수 있다.

<br>

### Factor Method Pattern 과의 차이점

> 단일 유형의 객체 또는 특정 유형의 관련 객체 집합을 생성해야 하는 경우 `Factory Method Pattern` 을, 여러 유형의 관련 객체 집합을 생성해야 하는 경우 `Abstract Factory Pattern` 을 사용하는 것이 적합

<br>

### 동작 방식

<img src="https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure-2x.png" width="80%">

#### with Java

> 추상 팩토리 패턴을 사용하여, 여러 운영체제에서 실행할 수 있는 사용자 인터페이스를 구현해보자.

```java
public interface UIFactory {
    public Button createButton();
    public Label createLabel();
}
```
1. 다양한 유형의 UI 컴포넌트를 생성하는 메서드를 선언하는 추상 팩토리 인터페이스를 정의

```java
public class WindowsUIFactory implements UIFactory {
    public Button createButton() {
        return new WindowsButton();
    }

    public Label createLabel() {
        return new WindowsLabel();
    }
}

public class MacUIFactory implements UIFactory {
    public Button createButton() {
        return new MacButton();
    }

    public Label createLabel() {
        return new MacLabel();
    }
}
```
2. UIFactory 인터페이스를 구현하는 각 운영체제에 대한 구체적인 팩토리 클래스를 정의한다.

```java
public interface Button {
    public void click();
}

public interface Label {
    public void setText(String text);
}
```
3. 각 구성 요소를 나타내는 추상 클래스 또는 인터페이스를 사용하여 UI 구성 요소 자체를 정의한다.

```java
public class WindowsButton implements Button {
    public void click() {
        // Windows 에서 버튼을 클릭했을 때 동작하는 특정 코드
    }
}

public class MacButton implements Button {
    public void click() {
        // Mac 에서 버튼을 클릭했을 때 동작하는 특정 코드
    }
}

public class WindowsLabel implements Label {
    public void setText(String text) {
        // Windows 에서 라벨을 입력했을 때 동작하는 특정 코드
    }
}

public class MacLabel implements Label {
    public void setText(String text) {
        // Mac 에서 라벨을 입력했을 때 동작하는 특정 코드
    }
}
```
4. 각 운영체제별 컴포넌트를 구현하는 클래스 생성

```java
UIFactory factory = null;
String osName = System.getProperty("os.name").toLowerCase();
if (osName.indexOf("win") >= 0) {
    factory = new WindowsUIFactory();
} else if (osName.indexOf("mac") >= 0) {
    factory = new MacUIFactory();
}

Button button = factory.createButton();
Label label = factory.createLabel();

button.click();
label.setText("Hello, world!");
```
5. 추상 팩토리를 사용하여 각 운영체제에 맞는 UI 구성요소 생성

> 추상 팩토리를 사용하여 코드를 특정 구현에 종속시키지 않고 현재 운영체제에 대한 관련 UI 컴포넌트 제품군을 생성할 수 있다. 유연한 모듈식 코드 구현 가능