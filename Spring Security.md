# Spring Security 

## Spring Security 개요

### Spring Security란

> Spring MVC 기반 애플리케이션의 인증(Authentication)과 인가 기능을 지원하는 보안 프레임워크로써, Spring MVC 기반 애플리케이션에 보안을 적용하기위한 사실상의 표준이다.
> 

- **보안 기능**
    - 다양한 유형의 사용자 인증 기능 적용(폼, 토큰, OAuth 2, LDAP 인증)
    - 애플리케이션 사용자의 역할에 따른 권한 레벨 적용
    - 애플리케이션에서 제공하는 리소스에 대한 접근 제어
    - 민감한 정보에 대한 데이터 암호화
    - SSL 적용
    - 웹 보안 공격 차단

- **용어 정리**
    - **Principal(주체)**
        - 앱에서 작업을 수행할 수 있는 사용자, 디바이스 또는 시스템. 일반적으로  인증 프로세스가 성공적으로 수행된 사용자의 계정 정보를 의미
    - **Authentication(인증)**
        - 애플리케이션을 사용하는 사용자가 본인이 맞음을 증명하는 절차
        - 사용자를 식별하기 위한 정보를 **Credential** 이라고 한다.
    - **Authorization(인가, 권한부여)**
        - Authentication이 정상적으로 수행된 사용자에게 하나 이상의 권한을 부여하여 특정 리소스에 접근할 수 있게 허가하는 과정
    - **Access Control(접근 제어)**
        - 사용자가 애플리케이션의 리소스에 접근하는 행위를 제어
    

### Spring Security의 기본 구조

- 의존 라이브러리 추가
- Spring Security Configuration 적용
    - `@Configuration` 애너테이션만 추가하면 Spring Security가 적용된다.
- InMemory User로 사용자 정보추가
- HTTP 보안 구성 기본
- 커스텀 로그인 페이지 지정하기
- Request URL에 접근 권한 부여
- 로그인 로그아웃 기능 설정

- `PasswordEncoder`로 패스워드 암호화
- **데이터베이스 연동없이 로그인 인증 (테스트환경, 데모환경)**
    - 회원가입 폼을 통해 InMemory User 등록
        - `MemberService` Bean 등록을 위한 `JavaConfiguration` 구성
        - `InMemoryMemberService` 구현

- **데이터베이스 연동을 통한 로그인 인증**
    - **Custom UserDetailsService를 사용하는 방법**
        - `SecurityConfiguration` 설정 변경 및 추가
        - `JavaConfiguration`의 Bean 등록 변경
        - `DBMemberService` 구현
            - User의 인증 정보를 DB에 저장하는 역할
        - Custom `UserDetailsService` 구현
            
            ⇒ 데이터베이스에서 조회한 User의 인증 정보를 기반으로 인증을 처리
            
            - `HelloUserDetailsService`
            - `HelloAuthorityUtils`
        - H2 웹 콘솔에서 등록한 회원 정보 확인 및 로그인 인증 테스트
        - Custom `UserDetails` 구현
            - `HelloUserDetailsService` 개선
        - User의 Role을 DB에서 관리
            - User의 권한 정보 테이블 생성
            - 회원 가입 시, User의 권한 정보를 DB에 저장
            - 로그인 인증 시, User의 권한 정보를 DB에서 조회하는 작업
    
    - **Custom `AuthenticationProvider`를 사용하는 방법**
        - `AuthenticationProvider`는 Spring Security에서 클라이언트로부터 전달받은 인증 정보를 바탕으로 인증된 사용자인지에 대한 인증 처리를 수행하는 Spring Security 컴포넌트다.

## Spring Security 웹 요청 처리 흐름과 필터

### Spring Security의 웹 요청 처리 흐름

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/94ec0d3b-ffd7-4cc0-8106-77912fc3603f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220929T131220Z&X-Amz-Expires=86400&X-Amz-Signature=e535d5b11ffda142be0c676068069958a175ee576475d26cd2025b49c35b56fc&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

1. 사용자가 보호된 리소스를 요청
2. 인증 관리자 역할을 하는 컴포넌트가 사용자의 Credential을 요청
    1. 사용자의 Credential : 해당 사용자를 증명하기 위한 구체적인 수단(Password 등)
3. 사용자는 인증 관리자에게 Credential을 제공
4. 인증 관리자는 Credential 저장소에서 사용자의 Credential을 조회
5. 인증 관리자는 사용자가 제공한 Credential과 Credential 저장소에서 조회한 정보를 비교해 검증 작업 수행
6. 유효한 Credential이 아니라면 Exception Throw
7. 유효한 Credential이라면 ⇒
8. 접근 결정 관리자 역할을 하는 컴포넌트는 사용자가 적절한 권한을 부여받았는지 검증
9. 적절한 권한을 부여 받지 못한 사용자라면 Exception Throw
10. 적절한 권한을 부여 받은 사용자라면 보호된 리소스의 접근을 허용

### **웹 요청에서의 서블릿 필터와 필터 체인 역할**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e9d685ee-d796-48de-adbd-baf0a5993574/Untitled.png)

- 컴포넌트가 중간에서 웹 요청을 가로채 사용자의 Credential과 접근권한을 검증하는 동작을 한다.

<aside>
💡 ⇒ 서블릿 기반 애플리케이션의 경우, 애플리케이션의 엔드포인트에 요청이 도달하기 전에 중간에서 요청을 가로챈 후 어떤 처리를 할 수 있는 포인트를 제공하는데 그것이 바로 **서블릿 필터**다

</aside>

- 서블릿 필터는 자바에서 제공하는 API. `javax.servlet.Filter` 인터페이스를 구현한다.
- 서블릿 필터는 하나 이상의 필터들을 연결해 필터 체인을 구성할 수 있다.
- 각각의 필터들이 `doFilter()` 라는 메서드를 구현해야하며, 메서드 호출을 통해 필터 체인을 형성

<aside>
💡 **특별한 작업들을 수행한 뒤, HttpServlet을 거쳐 DispatcherServlet에 요청이 전달**되며, 반대로 DispatcherServlet에서 전달한 **응답에 대해 역시 특별한 작업을 수행할 수 있습니다.**

</aside>

### **Spring Security에서의 필터 역할**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/912d6e7e-69c8-460c-a725-ec936233b9a7/Untitled.png)

- `DelegatingFilterProxy` 와 `FilterChainProxy` 클래스는 Filter 인터페이스를 구현하기 때문에 엄연히 서블릿 필터로써의 역할을 한다.
- **`DelegatingFilterProxy`**
    - 서블릿 컨테이너 영역의 필터와 `ApplicationContext`에 Bean으로 등록된 **필터들을 연결해주는 브릿지 역할**
- **`FilterChainProxy`**
    - Spring Security의 **Filter Chain**은 Spring Security에서 **보안을 위한 작업을 처리하는 필터의 모음**이며**,** Spring Security의 **Filter를 사용하기 위한 진입점**이 바로 `FilterChainProxy` 다.

- **Filter와 FilterChain**
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d6f9b735-b7ca-4464-a814-091e214d6e40/Untitled.png)
    
    - Servlet FilterChain은 요청 URI path를 기반으로 HttpServletRequest를 처리한다. 따라서 서버가 요청을 받으면 **서블릿 컨테이너는 요청 URI의 경로를 기반으로 어떤 Filter와 Servlet을 매핑할지 결정**
    - Filter는 FilterChain 안에서 순서를 지정
    - Filter의 순서는 매우 중요하며, SpringBoot에서 순서를 지정하는 방법은
        - Filter에 `@Order` 애너테이션을 추가하거나 `Orderd` 인터페이스 구현
        - `FilterRegistrationBean` 을 이용해 Filter의 순서 지정

- `**DelegatingPasswordEncoder**`
    - Spring Security에서 지원하는 `**PasswordEncoder` 구현 객체를 생성해주는 컴포넌트**로써, 사용할 `PasswordEncoder`를 결정하고, 패스워드를 **단방향으로 암호화**해준다.
    - 사용하고자 하는 암호화 알고리즘을 지정하지 않으면 Spring Security에서 권장하는 최신 암호화 알고리즘을 사용하여 패스워드를 암호화 할 수 있다.
    - 언제든지 암호화 방식을 변경할 수 있다. ⇒ 기존 암호화된 패스워드에 대한 마이그레이션 작업이 진행되어야함.

## Spring Security 인증 구성요소

### Spring Security의 인증 처리 흐름 및 컴포넌트

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c12ea90e-9408-4662-8400-9997d9b19366/Untitled.png)

- `**UsernamePasswordAuthenticationFilter**`
    - **사용자의 로그인 요청을 처리**하는 Spring Security Filter 다.
- `**UsernamePasswordAuthenticationToken**`
    - `Authentication` 인터페이스를 구현한 클래스이며, `Authentication`은 **아직 인증이 되지않은 `Authentication` 을 의미**한다.
- `**AuthenticationManager**`
    - **인증 처리를 총괄하는 매니저** 역할을 하는 인터페이스
    - `ProviderManager` 는 `AuthenticationManager`를 구현한 구현클래스다.
- `**UserDetails**`
    - 사용자의 자격을 증명해주는 **Credential 를 포함하고 있는 컴포넌트**다.
    - `UserDetails`를 제공하는 컴포넌트가 바로 `UserDetailsService` 이다.
- `**UserDetailsService**`
    - 데이터베이스 등의 저장소에서 사용자의 **Credential을 조회**하여 `AuthenticationProvider` 에게 제공한다.
- `**AuthenticationProvider**`
    - `UsernamePasswordAuthenticationFilter` 가 생성하는 **Authentication은 인증을 위해 필요한 사용자의 로그인 정보**를 가지고 있지만, `AuthenticationProvider` 가 생성한 **Authentication은 인증에 성공한 사용자의 정보**(Principal, Credential, GrantedAuthorities)를 가지고 있다.
    - 인증된 Authentication을 `ProviderManager` 에게 전달하고, `ProviderManager`는 다시 `UsernamePasswordAuthenticationFilter`에게 전달한다.
- `**SecurityContextHolder**`
    - 인증된 Authentication을 전달 받은 `UsernamePasswordAuthenticationFilter`는 `SecurityContextHolder`를 이용해 `SecurityContext`에 인증된 Authentication을 저장한다.
    - `SecurityContext`는 다시 HttpSession에 저장되어 **사용자의 인증 상태를 유지**한다.

### Spring Security의 인증 컴포넌트

## Spring Security 권한 부여 구성요소

### Spring Security 컴포넌트로 보는 권한 부여 처리 흐름

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94ec0d3b-ffd7-4cc0-8106-77912fc3603f/Untitled.png)

- `**AuthorizationFilter**`
    - Spring Security **Filter Chain에서 URL을 통해 사용자의 액세스를 제한하는 권한 부여 Filter**
    - `SecurityContextHolder`로부터 Authentication을 획득
    - 획득한 Authentication과 HttpServletRequest를 `AuthorizationManager` 에게 전달
- `**AuthorizationManager**`
    - **권한 부여 처리를 총괄**하는 매니저 역할을 하는 인터페이스
    - `RequestMatcherDelegatingAuthorizationManager` 는 AuthorizationManager를 구현하는 구현체 중 하나다.
- `**RequestMatcherDelegatingAuthorizationManager**`
    - RequestMatcher 평가식을 기반으로 해당 평가식에 매치되는 `**AuthorizationManager`에게 권한 부여 처리를 위임**하는 역할
    - 직접 권한 부여 처리를 하는 것이 아니라 `RequestMatcher`를 통해 매치되는 `AuthorizationManager` 구현 클래스에게 위임만 한다.
    - 내부에서 매치되는 AuthorizationManager 구현 클래스가 있다면 해당 **AuthorizationManager 구현 클래스가 사용자의 권한을 체크**
    - 적절한 권한이 아니라면 AccessDeniedException이 throw되고 `**ExceptionTranslationFilter`가 AccesDeniedException을 처리**한다.
- `RequestMatcher`
    - RequestMatcher는 SecurityConfiguration에서 `.antMatchers("/orders/**").hasRole("ADMIN")`와 같은 메서드 체인 정보를 기반으로 생성된다.
