## DI(Dependency Injection)

### Spring 컨테이너(Container)와 빈(Bean)

> 스프링 컨테이너는 내부에 존재하는 애플리케이션 빈의 생명주기를 관리
⇒ Bean 생성, 관리, 제거 등의 역할을 담당
> 
- **What is Spring Container?**
    - Spring에서 xml로 설정했지만, Springboot 에서는 자동으로 설정
    - 빈의 인스턴스화, 구성, 전체 생명주기 및 제거까지 관리
    - 스프링 컨테이너를 통해 원하는 만큼 많은 객체를 가질 수 있다.
    - 의존성 주입을 통해 애플리케이션의 컴포넌트를 관리
        - 스프링 컨테이너는 서로 다른 빈을 연결해 애플리케이션의 빈을 연결

- **Why use Spring Container?**
    - 객체를 사용하기 위해 new 생성자를 사용하면, 서로 참조가 심할수록 의존성이 높아진다.
    - 낮은 결합도와 높은 캡슐화가 OOP의 핵심이므로, **객체 간의 의존성을 낮추기 위해 Spring 컨테이너가 사용된다.**

- **How is Spring Container created?**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e381e0a-c6dc-4aba-b374-62001bad1554/Untitled.png)
    
    - 스프링 컨테이너는 Configuration Metadata를 사용
    - 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스피링 빈을 등록
        
        ⇒ 애너테이션 기반의 자바 설정 클래스로 Spring을 만드는 것을 의미
        
        ```java
        // Spring Container 생성
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        ```
        
    - 스프링 컨테이너를 만드는 다양한 방법은 `ApplicationContext` 인터페이스의 구현체다.

- **스프링 컨테이너의 종류**
    - `BeanFactory` : 스프링 컨테이너의 최상위 인터페이스
        - 빈을 등록하고 생성하고 조회하고 돌려주는 등 빈을 관리하는 역할
        - `@Bean` : 스프링 빈의 이름으로 사용해 빈 등록한다.
    - `ApplicationContext` : BeanFactory의 기능을 상속받아 제공
        - 빈을 관리하고 검색하는 기능을 BeanFactory가 제공하고 그 외에 부가기능을 제공
        - MessageSource / EnvironmentCapable / ApplicationEventPublisher / ResourceLoader
    - `@Configuration` : 구성정보를 담당하는것을 설정할때 @Configuration 을 붙여줍니다.
    - `@Bean` : 각 메서드에 `@Bean` 을 붙이면 스프링 컨테이너에 자동을 등록

- **빈(Bean)**
    
    > 스프링 컨테이너에 의해 관리되는 재사용 소프트웨어 컴포넌트이다.
    > 
    - **Spring 컨테이너가 관리하는 자바 객체**를 의미하며, 하나 이상의 빈을 관리한다.
    - 빈(bean)은 인스턴스화된 객체를 의미
    - 스트링 컨테이너에 등록된 객체를 **스프링 빈**이라고 한다.
    - 빈은 컨테이너에 사용되는 설정 메타데이터로 생성된다.
        - 설정 메타데이터 : 컨테이너의 명령과 인스턴스화, 설정, 조립할 객체를 정의
- **BeanDefinition**
    - 스프링은 다양한 설정 형식을 BeanDefinition이라는 추상화 덕분에 지원할 수 있다.
    - Bean은 BeanDefinition(빈 설정 메타정보)로 정의되고 BeanDefinition에 따라서 활용하는 방법이 달라진다.
    - 속성에 따라 컨테이너가 Bean을 어떻게 생성하고 관리할지 결정한다.
    
    <aside>
    💡 스프링 컨테이너는 설정 형식이 XML인지 Java 코드인지 모르고 BeanDefinition만 알면 된다.
    
    **⇒** **Spring이 설정 메타정보를 BeanDefinition 인터페이스를 통해 관리하기 때문**
    
    </aside>
    

### 빈 스코프(Bean Scope)

- Bean Definition
    - Bean Definition을 만들 때 해당 bean definition에 의해 정의된 클래스의 실제 인스턴스를 만들기 위한 레시피를 만든다.
        
        ⇒ 빈이 존재할 수 있는 범위를 의미
        
    
    | singleton | 각 Spring 컨테이너에 대한 단일 객체 인스턴스에 대한 단일 bean definition 범위 지정 |
    | --- | --- |
    | prototype | 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 짧은 범위의 스코프 |
    | request | 웹 요청이 들어오고 나갈때 까지 유지되는 스코프 |
    | session | 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프 |
    | application | 웹의 서블릿 컨텍스와 같은 범위로 유지되는 스코프 |
    | websocket | 단일 bean definition 범위를 WebSocket의 라이프사이클까지 확장. ApplicationContext의 컨텍스트에서만 유효 |
- **싱글톤 스코프**
    
    > 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 **디자인 패턴**
    > 
    - 스프링 컨테이너의 시작과 함께 생성되어서 종료될 때까지 유지
    - 하나의 공유 인스턴스만 관리
    - 단일 인스턴스는 싱글톤 빈의 캐시에 저장된다.
    - 싱글톤 스코프의 스프링 빈은 여러번 호출해도 모두 같은 인스턴스 참조 주소값을 가집니다.
    
    ⇒ static 영역에 객체 인스턴스를 미리 1개 생성하여 getIstance() 메서드를 통해서만 조회할 수 있도록 한다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10905094-d648-4bab-9ffd-f91b26b7243d/Untitled.png)
    

### Java 기반 컨테이너(Container) 설정

### Spring DI(Dependency Injection)

### Component 스캔

## AOP(Aspect Oriented Programming)

### AOP 정의

### 타입별 Advice

### Pointcut 표현식

### JoinPoint

### 애너테이션을 이용한 AOP
