# Strategy Pattern
<!-- TOC -->

- [Strategy Pattern](#strategy-pattern)
    - [Strategy Pattern 이란?](#strategy-pattern-%EC%9D%B4%EB%9E%80)
        - [Strategy Pattern 을 사용하는 이유](#strategy-pattern-%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
        - [동작 방식](#%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D)
        - [용도](#%EC%9A%A9%EB%8F%84)
    - [Strategy Pattern 의 장단점](#strategy-pattern-%EC%9D%98-%EC%9E%A5%EB%8B%A8%EC%A0%90)
        - [장점](#%EC%9E%A5%EC%A0%90)
        - [단점](#%EB%8B%A8%EC%A0%90)
    - [구현](#%EA%B5%AC%ED%98%84)
        - [with Java](#with-java)
        - [with Go](#with-go)

<!-- /TOC -->

<br>

## Strategy Pattern 이란?

<img src="https://user-images.githubusercontent.com/124122215/226179088-06975af8-f14d-4e24-8953-1d5c05a676d1.png" width="60%">

> 전략 패턴은 런타임에 알고리즘의 동작을 선택할 수 있도록 하는 동작 설계 패턴이다. 알고리즘 계열을 정의하고 각각을 캡슐화하고 상호 교환 가능하게 만드는 방법이다.

- 전략 패턴을 사용하면 알고리즘을 사용하는 클라이언트와 독립적으로 알고리즘을 변경할 수 있다.
- 패턴은 각각 특정 알고리즘을 구현하는 Context Class 와 일련의 전략으로 구성된다. Context 클래스는 위임을 사용하여 작업을 전략 개체에 위임한다.
    - Context 클래스 : 특정 전략을 구현하는 객체에 대한 참조를 보유하는 클래스
        - 기본 전략 객체에 대한 통로 인터페이스 역할을 하여 전략과 상호 작용하는 단순화되고 통합된 방법을 제공
        > 클라이언트와 기본 전략 간의 추상화 수준을 제공하여 Context 클래스가 보유하는 객체 참조를 변경하기만 하면 런타임에 쉽게 전략을 변경할 수 있다.

- 이를 통해 클라이언트 코드에 영향을 주지 않고, 사용 중인 알고리즘을 런타임에 수정할 수 있다.

<br>

### Strategy Pattern 을 사용하는 이유

> 전략 패턴은 객체가 동작을 동적으로 변경할 수 있도록 하는데 사용된다. 이는 여러 알고리즘을 정의하고 객체가 런타임에 알고리즘 간에 전환할 수 있도록 함으로써 수행할 수 있다.

- 전략패턴을 사용하면 객체가 클래스를 변경하지 않고도 상황에 따라 동작을 변경할 수 있다는 장점이 있다.
    - 클린 코드와 유지 관리에 용이 + 유연성 향상 + 기존 코드에 영향을 주지않고 새 알고리즘 추가

<br>

### 동작 방식

> 다양한 전략을 나타내는 객체와 전략 객체에 따라 동작이 달라지는 Context 객체를 만드는 작업이 포함된다. 전략 객체는 Context 객체의 실행 알고리즘을 변경한다.

- 패턴은 알고리즘 계열을 정의하고 각 알고리즘을 캡슐화하며 상호 교환 가능하게 만든다. 패턴을 사용하면 클라이언트가 적절한 알고리즘을 동적으로 선택할 수 있으므로, 더 큰 유연성과 문제 분리가 제공된다.
- Context 객체에는 전략 객체에 대한 참조가 있으며, 메서드 실행을 전략 객체에 위임한다. Context 개게는 전략 객체의 구체적인 클래스를 알지 못하고, 전략 객체의 인터페이스만 알고 있다.
    - 전략 객체에서 Context 객체를 분리하고 컨텍스트 객체에서 사용하는 알고리즘을 쉽게 변경할 수 있다.

<br>

### 용도

- 예시 : 전자 상거래 웹사이트의 배송비를 계산하는 애플리케이션
    - 전자 상거래에 표준, 신속, 익일 배송과 같은 여러 배송 옵션이 있는데, 새 배송 옵션이 추가되거나 기존 옵션이 변경될 때마다 코드를 수정해야 하는 대신 전략 패턴을 사용하여 각 배송 옵션에 대한 알고맂므을 별도의 객체로 정의할 수 있다.

<br>

## Strategy Pattern 의 장단점

### 장점

1. 관심사의 분리
    - 패턴은 실제 구현에서 알고리즘의 동작을 분리하여 런타임 시 동작을 보다 쉽게 변경할 수 있도록 해준다.
2. 재사용성
    - 동작과 구현이 분리되어 있으므로 애플리케이션의 다른 부분에서 동일한 동작을 재사용할 수 있다.
3. 유연성
    - 런타임에 동작을 변경할 수 있으므로 기존 코드를 변경하지 않고도 응용 프로그램을 새로운 요구사항에 맞게 조정할 수 있다.
4. 유지 관리 향상
    - 코드는 애플리케이션이 확장되고 변경되더라도 더 쉽게 이해하고 유지 관리 할 수 있는 방식을 구성된다.

<br>

### 단점

1. 복잡성 증가
    - 패턴은 코드에 복잡성 계층을 추가하여 이해하고 디버그하기 어렵게 만든다.
2. 성능 오버헤드
    - 추가 객체 및 동적 디스패치가 필요하므로 성능 오버헤드가 발생할 수 있다.
        - 디스패치는 어떤 메서드를 호출할 것인가를 결정하여 실행하는 과정
        - 동적 디스패치는 메서드 오버라이딩이 되어있는 경우 실행시점에 어떤 메서드를 실행할 지 결정되는 것
3. 코드 크기 증가
    - 추가 클래스와 객체가 필요하므로 코드 사이즈가 커질 수 있다.

<br>

## 구현

<img src="https://user-images.githubusercontent.com/124122215/226180856-413961b3-90c3-471b-9975-d5a67111b2a8.png" width="70%">

### with Java

- Java 의 전략 패턴은 다음 단계를 사용하여 구현할 수 있다.

1. 임의의 알고리즘을 정의하는 인터페이스를 만든다.
2. 각 알고리즘에 대한 구체적인 클래스를 구현하고 인터페이스를 구현한다.
3. 전략 객체에 대한 참조를 보유하는 Context 클래스를 만든다.
4. Context 클래스는 전략 객체에 알고리즘을 수행하는 책임을 위임한다.

```java
// Step 1: 인터페이스 생성
interface SortStrategy {
    void sort(int[] numbers);
}

// Step 2: 구체적인 클래스 구현
class BubbleSort implements SortStrategy {
    @Override
    public void sort(int[] numbers) {
        // 버블 정렬 알고리즘 로직
    }
}

class QuickSort implements SortStrategy {
    @Override
    public void sort(int[] numbers) {
        // 퀵 정렬 알고리즘 로직
    }
}

// Step 3: context 클래스 생성
class Sorter {
    private SortStrategy strategy;

    public Sorter(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void sort(int[] numbers) {
        this.strategy.sort(numbers);
    }
}

// Step 4: 전략패턴 실행
public class Main {
    public static void main(String[] args) {
        int[] numbers = {5, 4, 1, 3, 2};
        Sorter sorter = new Sorter(new BubbleSort());
        sorter.sort(numbers); // 버블 정렬 수행

        sorter.setStrategy(new QuickSort());
        sorter.sort(numbers); // 퀵 정렬 수행
    }
}
```
- `Sorter` 클래스는 Context 역할을 하며 숫자 정렬 책임을 전략 객체 (`BubbleSort` 또는 `QuickSort`) 에 위임한다. `setStrategy` 코드는 메서드를 호출하여 런타임에 정렬 전략을 변경할 수 있다.

<br>

### with Go

```go
// 다양한 정렬 알고리즘을 위한 인터페이스 정의
type Sorter interface {
    Sort([]int) []int
}

// 정렬 알고리즘 구현 
type BubbleSort struct{}

func (b *BubbleSort) Sort(numbers []int) []int {
    // 버블 정렬 구현 로직
    return numbers
}

type QuickSort struct{}

func (q *QuickSort) Sort(numbers []int) []int {
    // 퀵 정렬 구현 로직
    return numbers
}

// Context 생성 : 정렬 알고리즘에 대한 참조를 갖고, 런타임에 알고리즘을 변경할 수 있는 구조체
type Context struct {
    sorter Sorter
}

func (c *Context) SetSorter(sorter Sorter) {
    c.sorter = sorter
}

func (c Context) Sort(numbers []int) []int {
    return c.sorter.Sort(numbers)
}

func main() {
    context := Context{}
    bubbleSort := BubbleSort{}
    quickSort := QuickSort{}

    numbers := []int{5, 1, 3, 2, 4}

    context.SetSorter(bubbleSort)
    fmt.Println("버블 정렬 : ", context.Sort(numbers))
    
    context.setSorter(quickSort)
    fmt.Println("퀵 정렬 : ", context.Sort(numbers))
}
```