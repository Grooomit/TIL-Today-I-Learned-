# Status Pattern
<!-- TOC -->

- [Status Pattern](#status-pattern)
    - [Status Pattern 이란?](#status-pattern-%EC%9D%B4%EB%9E%80)
    - [패턴 구조](#%ED%8C%A8%ED%84%B4-%EA%B5%AC%EC%A1%B0)
    - [예제](#%EC%98%88%EC%A0%9C)
    - [고려해야 할 점](#%EA%B3%A0%EB%A0%A4%ED%95%B4%EC%95%BC-%ED%95%A0-%EC%A0%90)
    - [실제 예시](#%EC%8B%A4%EC%A0%9C-%EC%98%88%EC%8B%9C)

<!-- /TOC -->

<br>

## Status Pattern 이란?

> 객체의 내부 상태에 따라 스스로 행동을 변경할 수 있게 허가하는 패턴으로, 이렇게 하면 객체는 마시 자신의 클래스를 바꾸는 것처럼 보인다.

- 주된 목적은 상태 전이를 위한 조건 로직이 지나치게 복잡한 경우, 이를 해소하기 위함
    - 상태 전이 로직이란 객체의 상태와 이들 간의 전이 방법을 제어하는 것으로, 보통 클래스 내부 여러곳에 흩어져 존재

- State 패턴을 구현한다는 것은 각 상태에 대응하는 별도의 클래스를 만들고, 상태 전이 로직을 그 클래스들로 옮기는 작업

<br>

## 패턴 구조

![image](https://user-images.githubusercontent.com/107091097/224718724-1ab7089e-d607-47d4-b39b-5db74147f1f5.png)

- Context
    - 사용자가 관심 있는 인터페이스를 정의
    - 객체의 각 상태를 정의한 State의 구현체 인스턴스를 관리
- State
    - Context 의 각 상태별 행동을 정의

<br>

## 예제

> 간단하게 동적 하나를 받아 티켓을 뽑을 수 있는 티켓 자판기 구현

<img src="https://user-images.githubusercontent.com/107091097/224719122-71f7e523-1826-45d2-b5d0-a0a6507c5d41.png" width="80%">

- 동전이 없는 상태
    - 동전 투입을 기다리는 상태
    - 티켓 출력을 해도 상태 변화 X
    - 동전을 넣으면 투입된 상태로 변경
- 동전이 투입된 상태
    - 티켓 출력 가능
    - 동전을 추가 투입해도 상태 유지
    - 티켓 출력하면 동전이 없는 상태로 변경

```java
// State
public interface State {
    void insertCoin();
    void printTicket();
}
```
```java
// 동전이 없는 상태
class NoCoinState implements state {
    TicketMachine  ticketMachine;

    NoCoinState(TicketMachine ticketMachine) {
        this.ticketMachine = ticketMachine;
    }

    @Override
    public void insertCoin() {
        // 동전을 넣었다면 동전이 있는 상태로 이동
        ticketMachine.setState(ticketMachine.getCoinState());
    }

    @Override
    public void printTicket() {
        System.out.println("동전이 없습니다. 동전을 넣어주세요.");
    }
}
```
```java
// 동전이 있는 상태
class CoinState implements State {
    private final TicketMachine ticketMachine;

    CoinState(TicketMachine ticketMachine) {
        this.ticketMachine = ticketMachine;
    }

    @Override
    public void insertCoin() {
        System.out.println("이미 동전이 들어있습니다.");
    }

    @Override
    public void printTicket() {
        // 티켓을 출력하고, 동전을 동전저장소에 추가 + 동전이 없는 상태로 변경
        TicketPrinter.print();
        CoinRepository.add(1);
        ticketMachine.setState(ticketMachine.getNoCoinState());
    }
}
```
```java
// Context 티켓 자판기
public class TicketMachine {
    final State noCoinState;
    final State coinState;
    private State currentState;

    public TicketMachine() {
        this.noCoinState = new NoCoinState(this);
        this.coinState = new CoinState(this);

        this.currentState = noCoinState;
    }

    public void insertCoin() {
        currentState.insertCoin();
    }

    public void setState(State newState) {
        this.currentState = newState;
    }

    public State getCoinState() {
        return coinState;
    }

    public State getNoCoinState() {
        return noCoinState;
    }
}
```

<br>

## 고려해야 할 점

- 하나의 상태를 클래스 하나로 명확히 표현하는 것과, 상태 변수의 값으로 표현하는 것 중에 고려해야 한다.
    - 클래스로 명확히 표현하는 것이 낫다면, State Pattern 으로 리팩토링 가능
    - 더 복잡해진다면, 굳이 State Pattern 을 도입할 필요 X
- State Pattern 은 `if / else / switch` 를 효과적으로 제거
- 클래스의 수가 취급해야 하는 상태의 수만큼 추가로 늘어난다는 점에 주의
- 각 상태가 자신의 다음 상태를 알아야 한다는 특징
- 각각의 상태별로 똑같이 행동하는 메서드가 많다면, State Pattern 이 필요하지 않을 수 있음
- 각 상태를 Singleton 으로 관리하는 것도 고려

<br>

## 실제 예시

> GoF 에 따르면 TCP connection 구현에 State Pattern 이 사용되었다.

`존슨(Johnson)과 츠바이크(Zweig)는 실제로 상태 패턴을 정의하면서 TCP 연결 프로토콜에 적용하였습니다.`

```java
public interface TCPState {

    void activeOpen(TCPConnection tcpConnection);
    void passiveOpen(TCPConnection tcpConnection);
    void close(TCPConnection tcpConnection);
    void acknowledge(TCPConnection tcpConnection);
    void send(TCPConnection tcpConnection);
}
```
- `TCPClosed implements TCPState`
- `TCPEstablished implements TCPState`
- `TCPListen implements TCPState`
