# Decorator Pattern

## Decorator Pattern 이란?

> 데코레이터 패턴은 동작을 포함하는 특수 래퍼 개체 안에 개체를 배치하여 개체에 새로운 동작을 추가할 수 있는 구조적 디자인 패턴이다. 이것은 런타임에 개체의 동작을 동적으로 변경할 수 있다.

<br>

## Decorator Pattern 을 사용하는 이유

- 개체의 동작을 동적으로 수정할 수 있다.
- 여러 Decorator 를 사용하여 각각 고유한 동작을 추가하는 하나의 개체를 래핑할 수 있다.
- Decorator Pattern 은 책임을 동적으로 추가하거나 제거할 수 있기 때문에 상속보다 더 유연하다.

<br>

## Decorator Pattern 을 구현하는 방법

1. 기본 동작을 정의하는 인터페이스`(Component)`를 만든다.
2. 인터페이스의 구체적인 구현`(ConcreteComponent)`을 만든다.
3. 인터페이스를 구현하고 인터페이스 유형의 개체에 대한 필드가 있는 추상 데코레이터 클래스`(Base Decorator)`를 만든다.
4. 추상 데코레이터 클래스`(BaseDecorator)`를 확장하는 구체적인 데코레이터 클래스`(ConcreteDecorators)`를 만든다.
5. 각 구체적인 데코레이터 클래스`(ConcreteDecorators)`에서 필드에 포함된 개체에 동작을 추가한다.
6. 구체적인 데코레이터 클래스`(ConcreteDecorators)`를 사용하여 개체를 래핑하고 동작을 동적으로 추가한다.

<br>

## Decorator Pattern 의 장점

- 기존 객체에 새로운 기능을 동적으로 추가
- 기능을 추가하기 위해 상속을 사용하지 않음
- 여러 데코레이터를 쌓아 개체에 여러 기능을 추가할 수 있다.

<br>

## Decorator Pattern 의 단점

- Decorator 를 너무 많이 사용하면 코드가 더 복잡해질 수 있다.
- Decorator 는 여러 개체에 추가되고 기능 수가 증가하면 혼란스러워질 수 있다.

<br>

## Decorator Pattern 구현

### with Java

1. 기본 동작을 정의하는 인터페이스`(Component)`를 만든다.
```java
interface Pizza {
    String getDescription();
    double getCost();
}
```

2. 인터페이스의 구체적인 구현`(ConcreteComponent)`을 만든다.
    - PlainPizza 클래스는 Pizza 인터페이스를 구현하고 재료와 가격을 제공한다.
```java
class PlainPizza implements Pizza {
    public String getDescription() {
        return "Thin Dough";
    }
 
    public double getCost() {
        return 4.00;
    }
}
```
3. 인터페이스를 구현하고 인터페이스 유형의 개체에 대한 필드가 있는 추상 데코레이터 클래스`(Base Decorator)`를 만든다.
    - Pizza 인터페이스를 구현하고 기존 피자에 새로운 토핑을 추가하는 추상 클래스를 만든다.
```java
abstract class ToppingDecorator implements Pizza {
    protected Pizza tempPizza;
    public ToppingDecorator(Pizza newPizza){
        tempPizza = newPizza;
    }
 
    public String getDescription() {
        return tempPizza.getDescription();
    }
 
    public double getCost() {
        return tempPizza.getCost();
    }
}
```
4. 추상 데코레이터 클래스`(BaseDecorator)`를 확장하는 구체적인 데코레이터 클래스`(ConcreteDecorators)`를 만든다.
    - ToppingDecorator 클래스를 확장하고 피자에 모짜렐라 및 토마토 소스 토핑을 추가한다.
```java
class Mozzarella extends ToppingDecorator {
    public Mozzarella(Pizza newPizza) {
        super(newPizza);
        System.out.println("Adding Mozzarella");
    }
 
    public String getDescription() {
        return tempPizza.getDescription() + ", Mozzarella";
    }
 
    public double getCost() {
        return tempPizza.getCost() + 0.50;
    }
}
class TomatoSauce extends ToppingDecorator {
    public TomatoSauce(Pizza newPizza) {
        super(newPizza);
        System.out.println("Adding Tomato Sauce");
    }
 
    public String getDescription() {
        return tempPizza.getDescription() + ", Tomato Sauce";
    }
 
    public double getCost() {
        return tempPizza.getCost() + 0.35;
    }
}
```
5. 각 구체적인 데코레이터 클래스`(ConcreteDecorators)`에서 필드에 포함된 개체에 동작을 추가한다.
    - 다양한 클래스의 객체를 생성하고 그 설명을 출력한다.
```java
public class DecoratorExample {
    public static void main(String args[]) {
        Pizza basicPizza = new PlainPizza();
        System.out.println("재료: " + basicPizza.getDescription());
        System.out.println("가격: " + basicPizza.getCost());
 
        Pizza mozzarellaPizza = new Mozzarella(basicPizza);
        System.out.println("재료: " + mozzarellaPizza.getDescription());
        System.out.println("가격: " + mozzarellaPizza.getCost());
 
        Pizza tomatoSaucePizza = new TomatoSauce(mozzarellaPizza);
        System.out.println("재료: " + tomatoSaucePizza.getDescription());
        System.out.println("가격: " + tomatoSaucePizza.getCost());
    }
}
```

## 결론

Decorator Pattern 은 객체에 새로운 기능을 동적으로 추가할 수 있는 유용한 디자인 패턴이다. 여러 데코레이터를 쌓아서 많은 기능을 가진 복잡한 개체를 만들 수 있습니다. 그러나 코드를 더 복잡하고 이해하기 어렵게 만들 수 있으므로 이 패턴을 과도하게 사용하지 않도록 주의하세요.