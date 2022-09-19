## Spring MVC 아키텍처


- Spring MVC 정의
    - **Spring MVC는 클라이언트의 요청을 편리하게 처리해주는 프레임워크이다.**
    - **우리가 만들게 될 샘플 애플리케이션은 Spring MVC가 제공해주는 기능을 이용해서 만든다.**
    - MVC
        - **Model** : 클라이언트에게 응답으로 돌려주는 **작업의 처리 결과 데이터**
        - **View** : Model 데이터를 이용해서 웹브라우저 같은 클라이언트 애플리케이션의 화면에 보여지는 리소스(Resource)를 제공하는 역할
            
            → JSON(JavaScript Object Notation**) :** Spring MVC에서 클라이언트 애플리케이션과 서버 애플리케이션이 주고 받는 데이터 형식
            
        - **Controller** : 클라이언트 측의 요청을 직접적으로 전달 받는 엔드포인트(Endpoint)로써 Model과 View의 중간에서 상호 작용을 해주는 역할
    
    ⇒ 클라이언트 측의 요청을 전달 받아서 비즈니스 로직을 거친 후에 Model 데이터가 만들어지면, 이 Model 데이터를 View로 전달하는 역할을 합니다.
<br />

- Spring MVC 동작 방식
    - Client가 요청 데이터 전송 → Controller가 요청 데이터 수신 → 비즈니스 로직 처리 → Model 데이터 생성 → Controller에게 Model 데이터 전달 → Controller가 View에게 Model 데이터 전달 → View가 응답 데이터 생성
<br />

- Spring MVC 구성 요소
    
    ![image](https://user-images.githubusercontent.com/107091097/191025216-e4dd2e66-fc5f-4a83-a7fd-946a31e3350f.png)
    
    1. 먼저 클라이언트가 요청을 전송하면 `DispatcherServlet`이라는 클래스에 요청이 전달됩니다.
    2. `DispatcherServlet`은 **클라이언트의 요청을 처리할 Controller에 대한 검색**을 HandlerMapping 인터페이스에게 요청합니다.
    3. `HandlerMapping`은 **클라이언트 요청과 매핑되는 핸들러 객체를 다시 DispatcherServlet에게 리턴**해줍니다.
        
        → 핸들러 객체는 해당 핸들러의 Handler 메서드 정보를 포함하고 있습니다.
        Handler 메서드는 Controller 클래스 안에 구현된 요청 처리 메서드를 의미합니다.
        
    4. 요청을 처리할 Controller 클래스를 찾았으니 이제는 **실제로 클라이언트 요청을 처리할 Handler 메서드**를 찾아서 호출해야 합니다. `DispatcherServlet`은 Handler 메서드를 직접 호출하지 않고, HandlerAdpater에게 **Handler 메서드 호출을 위임**합니다.
    5. `HandlerAdapter`는 DispatcherServlet으로부터 전달 받은 Controller 정보를 기반으로 **해당 Controller의 Handler 메서드를 호출**합니다.
        
        > 이제 전체 처리 흐름의 반환점을 돌았습니다. 이제부터는 반대로 되돌아 갑니다. ^^
        > 
    6. `Controller`의 Handler 메서드는 비즈니스 로직 처리 후 리턴 받은 **Model 데이터를 HandlerAdapter에게 전달**합니다.
    7. `HandlerAdapter`는 전달받은 **Model 데이터와 View 정보를 다시 DispatcherServlet에게 전달**합니다.
    8. `DispatcherServlet`은 전달 받은 View 정보를 다시 ViewResolver에게 **전달해서 View 검색을 요청**합니다.
    9. `ViewResolver`는 View 정보에 해당하는 View를 찾아서 **View를 다시 리턴**해줍니다.
    10. `DispatcherServlet`은 ViewResolver로부터 전달 받은 View 객체를 통해 Model 데이터를 넘겨주면서 클라이언트에게 전달할 **응답 데이터 생성을 요청**합니다.
    11. `View`는 응답 데이터를 생성해서 다시 DispatcherServlet에게 전달합니다.
    12. `DispatcherServlet`은 **View로부터 전달 받은 응답 데이터를 최종적으로 클라이언트에게 전달**합니다.


## Controller

### Controller 구성요소

- 1. 패키지 구조 생성
    - **기능 기반 패키지 구조** : 애플리케이션에서 구현해야 하는 기능을 기준으로 패키지를 구성. **테스트와 리팩토링 용이**
        
        → 하나의 기능을 완성하기 위한 계층별(API 계층, 서비스 계층, 데이터 액세스 계층)클래스들이 모여있습니다.
        
    - **계층 기반 패키지 구조** : 패키지를 하나의 계층(Layer)으로 보고 클래스들을 계층별로 묶어서 관리하는 구조

- 2. 애플리케이션의 Controller 설계
    - **애플리케이션의 기능 요구 사항**
    - **애플리케이션에 필요한 리소스** : REST API 기반의 애플리케이션에서 일반적으로 제공해야 될 기능을 리소스로 분류

- 3. 엔트리포인트 클래스 작성
    - **`@SpringBootApplication` :**
        - 자동 구성을 활성화합니다.
        - 애플리케이션 패키지 내에서 `@Component`가 붙은 클래스를 검색한 후(scan), **Spring Bean**으로 등록하는 기능을 활성화합니다.
        - `@Configuration` 이 붙은 클래스를 자동으로 찾아주고, 추가적으로 Spring Bean을 등록하는 기능을 활성화합니다.
    - **`SpringApplication.run(Section3Week1Application.class, args);` :** Spring 애플리케이션을 부트스트랩하고, 실행하는 역할을 합니다.

- 4. 애플리케이션의 Controller 구조 작성
    - `@RestController`
        - Spring MVC에서는 특정 클래스에 `@RestController` 를 추가하면 해당 클래스가 REST API의 리소스(자원, Resource)를 처리하기 위한 API 엔드포인트로 동작함을 정의합니다.
        - 또한 `@RestController` 가 추가된 클래스는 애플리케이션 로딩 시, Spring Bean으로 등록해줍니다.
    - `@RequestMapping` 은 클라이언트의 요청과 클라이언트 요청을 처리하는 **핸들러 메서드(Handler Method)를 매핑**해주는 역할을 합니다.

- 5. 핸들러 메서드 작성
    - **요청 핸들러 메서드(Request Handler Method) :** 클라이언트의 요청을 전달 받아서 처리
    - `@PostMapping` 은 클라이언트의 요청 데이터(request body)를 서버에 생성할 때 사용하는 애너테이션
        - **HttpStatus.CREATED** : 클라이언트의 POST 요청을 처리해서 요청 데이터(리소스)가 정상적으로 생성되었음을 의미하는 HTTP 응답 상태
    - `@RequestParam` 은 클라이언트 쪽에서 전송하는 요청 데이터를 전송하면 이를 서버 쪽에서 전달 받을 때 사용하는 애너테이션
    - `@GetMapping` 은 클라이언트가 서버에 리소스를 조회할 때 사용하는 애너테이션
        - 별도의 URI를 지정해주지 않으면 클래스 레벨의 URI(“/v1/members”)에 매핑
    - `@PathVariable` 은 핸들러 메서드의 파라미터 종류 중 하나
    - **ResponseEntity** :  HttpEntity의 확장 클래스로써 HttpStatus 상태 코드를 추가한 전체 HTTP 응답(상태 코드, 헤더 및 본문)을 표현하는 클래스
        - → 주로 @Controller 또는 @RestController 애너테이션이 붙은 Controller 클래스의 핸들러 메서드(Handler Method)에서 요청 처리에 대한 응답을 구성하는데 사용
            
            ```java
            @RestController
            @RequestMapping("/v1/coffees")
            public class ResponseEntityExample01 {
                @PostMapping
                public ResponseEntity postCoffee(Coffee coffee) {
             
                    // coffee 정보 저장
                    return new ResponseEntity<>(coffee, HttpStatus.CREATED);
                }
            }
            ```
            
    
    <aside>
    💡 핸들러 메서드의 리턴 값으로 Map 객체를 리턴하면 Spring MVC 내부적으로 JSON 형식의 데이터를 생성해준다. 즉, 클래스 레벨의 `@RequestMapping` 에 ‘produces’ 애트리뷰트를 지정할 필요가 없다.
    
    </aside>
    

### HTTP Header

- **HTTP 헤더**
    - HTTP 메시지의 구성 요소 중 하나로써 클라이언트의 요청이나 서버의 응답에 포함되어 부가적인 정보를 HTTP 메시지에 포함.
    
    - ‘Content-Type’ 헤더 정보
        - 클라이언트와 서버가 주고 받는 HTTP 메시지 바디(body, 본문)의 데이터 형식이 무엇인지를 알려주는 역할
        - 클라이언트와 서버는 이 ‘Content-Type’이 명시된 데이터 형식에 맞는 데이터들을 주고 받는 것
    - 대표적인 HTTP 헤더 예시
        - **Authorization :** 클라이언트가 적절한 자격 증명을 가지고 있는지를 확인하기 위한 정보
        - **User-Agent :** 모바일 에이전트에서 들어오는 요청인지 모바일 이외에 다른 에이전트에서 들어오는 요청인지를 구분해서 처리

- HTTP Request 헤더 정보 얻기
    - `@RequestHeader`로 개별 헤더 정보 받기
        
        ```java
        **// @RequestHeader를 사용해서 특정 헤더 정보만 읽는 예제**
        @RestController
        @RequestMapping(path = "/v1/coffees")
        public class CoffeeController {
        	@PostMapping
        	public ResponseEntity postCoffee(**@RequestHeader**("user-agent") String userAgent,
        																	@RequestParam("korName") String korName,
        																	@RequestParam("engName") String engName,
        																	@RequestParam("price") int price) {
        			System.out.println("user-agent: " + userAgent);
        
        			return new ResponseEntity<>(new Coffee(korName, engName, price), HttpStatus.CREATED);
        	}
        }
        ```
        
    - `@RequestHeader`로 전체 헤더 정보 받기
        
        ```java
        **// @RequestHeader를 사용해서 Request의 모든 헤더 정보를 Map으로 전달 받는 예제**
        @RestController
        @RequestMapping(path = "/v1/members")
        public class MemberController {
        		@PostMapping
        		public ResponseEntity postMember(**@RequestHeader** Map<String, String> headers,
        																		@RequestParam("email") String email,
        																		@RequestParam("name") String name,
        																		@RequestParam("phone") String phone) {
        				for (Map.Entry<String, String> entry : headers.entrySet()) {
        						System.out.println("key : " + entry.getKey() + ", value : " + entry.getValue());
        				}
        		
        				return new ResponseEntity<>(new Member(email, name, phone), HttpStatus.CREATED);
        	}
        }
        ```
        
    - `HttpServletRequest` 객체로 헤더 정보 얻기
        - 단순히 특정 헤더 정보에 접근하고자 한다면  `HttpServletRequest`대신에 `@RequestHeader` 를 이용하는 편이 낫다.
        
        ```java
        **// HttpServletRequest 객체를 통해서 Request 헤더 정보 얻기**
        @RestController
        @RequestMapping(path = "/v1/orders")
        public class OrderController {
            @PostMapping
            public ResponseEntity postOrder(HttpServletRequest httpServletRequest,
        																		@RequestParam("memberId") long memberId,
        																		@RequestParam("coffeeId") long coffeeId) {
        				System.out.println("user-agent: " + httpServletRequest.getHeader("user-agent"));
        
        				return new ResponseEntity<>(new Order(memberId, coffeeId), HttpStatus.CREATED);
        		}
        }
        ```
        
    - `HttpEntity` 객체로 헤더 정보 얻기
        
        ```java
        **// HttpEntity 객체를 통해서 Request 헤더 정보를 읽어오는 예제**
        @RestController
        @RequestMapping(path = "/v1/coffees")
        public class CoffeeController{
            @PostMapping
            public ResponseEntity postCoffee(@RequestHeader("user-agent") String userAgent,//(1)
                                             @RequestParam("korName") String korName,
                                             @RequestParam("engName") String engName,
                                             @RequestParam("price") int price) {
                System.out.println("user-agent: " + userAgent);
                return new ResponseEntity<>(new Coffee(korName, engName, price),
                        HttpStatus.CREATED);
            }
        
        		@GetMapping
        		public ResponseEntity getCoffees(HttpEntity httpEntity) {
        				for(Map.Entry<String, List<String>> entry : httpEntity.getHeaders().entrySet()) {
        						System.out.println("key: " + entry.getKey() + ", value: " + entry.getValue());
        				}
        
        				System.out.println("host: " + httpEntity.getHeaders().getHost());
        				return null;
        		}
        }
        ```
        

- HTTP Response 헤더(Header) 정보 추가
    - `HttpHeaders`객체는 `ResponseEntity`에 포함을 시키는 처리가 필요하지만, `HttpServletResponse`객체는 헤더 정보만 추가할 뿐 별도의 처리가 필요없습니다.
    - `ResponseEntity`와 `HttpHeaders`를 이용해 헤더 정보 추가하기
        
        ```java
        **// ResponseEntity와 HttpHeaders를 이용해서 위치 정보를 커스텀 헤더로 추가**
        @RestController
        @RequestMapping(path = "/v1/members")
        public class MemberController{
            @PostMapping
            public ResponseEntity postMember(@RequestParam("email") String email,
                                             @RequestParam("name") String name,
                                             @RequestParam("phone") String phone) {
        				HttpHeaders headers = new HttpHeaders();
        				headers.**set**("Client-Geo-Location", "Korea,Seoul");
        
        				return new ResponseEntity<>(new Member(email, name, phone), headers, HttpStatus.CREATED);
        		}
        }
        ```
        
    - `HttpServletResponse` 객체로 헤더 정보 추가하기
        
        ```java
        **// HttpServletResponse를 이용해서 위치 정보를 커스텀 헤더로 추가**
        @RestController
        @RequestMapping(path = "/v1/members")
        public class MemberController{
            @GetMapping
            public ResponseEntity getMembers(HttpServletResponse response) {
        				response.addHeader("Clinet-Geo-Location", "Korea,Seoul");
        		
        				return null;
        		}
        }
        ```
        

### Rest Client

- Rest API 서버에 HTTP 요청을 보낼 수 있는 클라이언트 툴 또는 라이브러리
    - Postman은 UI가 갖춰진 Rest Client이다.
- **RestTemplate**
    - 원격지에 있는 다른 Backend 서버에 HTTP 요청을 전송할 수 있는 **Rest Client API**
    - RestTemplate을 사용할 수 있는 기능 예
        - 결제 서비스
        - 카카오톡 등의 메시징 서비스
        - Google Map 등의 지도 서비스
        - 공공 데이터 포털, 카카오, 네이버 등에서 제공하는 Open API
        - 기타 원격지 API 서버와의 통신
    - RestTemplate 사용 단계
        - RestTemplate 객체 생성
            - `RestTemplate` 의 생성자 파라미터로 HTTP Client 라이브러리의 구현 객체를 전달해야함
                
                → Apache HttpComponents를 사용하기 위해서는 build.gradle의 dependencies 항목에 `implementation` `'org.apache.httpcomponents:httpclient'` 의존 라이브러리를 추가해야합니다.
                
            
            ```java
            **// HttpComponentsClientHttpRequestFactory 클래스를 통해 Apache HttpComponents를 전달**
            public class RestClientExample01 {
                public static void main(String[] args) {
                    // (1) 객체 생성
                    RestTemplate restTemplate = 
                            new RestTemplate(new HttpComponentsClientHttpRequestFactory());
                }
            }
            ```
            
        - HTTP 요청을 전송할 엔드포인트의 URI 객체를 생성한다.
            - `RestTemplate` 객체를 생성했다면 HTTP Request를 전송할 Rest 엔드포인트의 URI를 지정
            
            ```java
            public class RestClientExample01 {
                public static void main(String[] args) {
                    // (1) 객체 생성
                    RestTemplate restTemplate =
                            new RestTemplate(new HttpComponentsClientHttpRequestFactory());
            
            				// (2) URI 생성
            				UriComponents uriComponents =
            								UriComponentsBuilder
            													.newInstance()   // UriComponentsBuilder 객체 생성
            													.scheme("http")  // URI의 scheme을 설정
            													.host("worldtimeapi.org")  // 호스트 정보를 입력
            														.port(80)  // 포트번호 지정 (default : 80)
            													.path("/api/timezone/{continents}/{city}")  // URI의 경로 입력
            													.encode()  // URI에 사용된 템플릿 변수 인코딩
            													.build();  // UriComponents 객체 생성
            				URI uri = uriComponents.expand("Asia", "Seoul").toUri();  // expand : URI 템플릿 변수값 대체, toUri : URI 객체 생성
            		}
            }
            ```
            
        - `getForObject()`, `getForEntity()`, `exchange()` 등을 이용해서 HTTP 요청을 전송한다.
            
            ```java
            public class RestClientExample01 {
                public static void main(String[] args) {
                    // (1) 객체 생성
                    RestTemplate restTemplate =
                            new RestTemplate(new HttpComponentsClientHttpRequestFactory());
            
                    // (2) URI 생성
                    UriComponents uriComponents =
                            UriComponentsBuilder
                                    .newInstance()
                                    .scheme("http")
                                    .host("worldtimeapi.org")
            //                        .port(80)
                                    .path("/api/timezone/{continents}/{city}")
                                    .encode()
                                    .build();
                    URI uri = uriComponents.expand("Asia", "Seoul").toUri();
            
            				// (3) Request 전송
            				String result = restTemplate.**getForObject**(uri, **String.class**);
            				// getForObject() 메서드는 HTTP Get 요청을 통해 서버의 리소스를 조회
            				// URI uri : Request를 전송할 엔드포인트의 URI 객체 지정
            				// Class<T> responseType : 응답으로 전달 받을 클래스의 타입 지정
            				
            				System.out.println(result);
            		}
            }
            ```
            
            - `getForObject()` 를 이용한 **커스텀 클래스 타입으로** 원하는 정보만 요청 받기
                
                <aside>
                ⚠️ 전달 받고자하는 **응답 데이터의 JSON 프로퍼티 이름과 클래스의 멤버변수이름이 동일**해야 한다. getter 메서드 역시 동일한 이름이어야 한다.
                
                </aside>
                
                ```java
                // (3) Request 전송. WorldTime 클래스로 응답 데이터를 전달 받는다.
                WorldTime worldTime = restTemplate.getForObject(uri, WorldTime.class);
                
                System.out.println("# datatime: " + worldTime.getDatetime());
                System.out.println("# timezone: " + worldTime.getTimezone());
                System.out.println("# day_of_week: " + worldTime.getDay_of_week());
                
                // WorldTime 클래스 선언
                public class WorldTime {
                    private String datetime;
                    private String timezone;
                    private int day_of_week;
                
                    public String getDatetime() {
                        return datetime;
                    }
                
                    public String getTimezone() {
                        return timezone;
                    }
                
                    public int getDay_of_week() {
                        return day_of_week;
                    }
                }
                ```
                
            - `getForEntity()` 를 사용한 Response Body(바디, 컨텐츠) + Header(헤더) 정보 전달 받기
                
                ```java
                **// (3) Request 전송. ResponseEntity로 헤더와 바디 정보를 모두 전달 받을 수 있다.**
                        ResponseEntity<WorldTime> response =
                                restTemplate.getForEntity(uri, WorldTime.class);
                
                        System.out.println("# datatime: " + response.getBody().getDatetime());
                        System.out.println("# timezone: " + response.getBody().getTimezone()());
                        System.out.println("# day_of_week: " + response.getBody().getDay_of_week());
                        System.out.println("# HTTP Status Code: " + response.getStatusCode());
                        System.out.println("# HTTP Status Value: " + response.getStatusCodeValue());
                        System.out.println("# Content Type: " + response.getHeaders().getContentType());
                        System.out.println(response.getHeaders().entrySet());
                ```
                
            

## [DTO(Data Transfer Object)](https://www.notion.so/DTO-Date-Transfer-Object-a71d3264612b427d885eb9baabe0a7e9)

---

### HTTP 요청/응답에서의 DTO

- DTO(Data Transfer Object)
    - DTO : 애플리케이션 아키텍처 패턴의 일종. 데이터를 전송하기 위한 용도의 객체
    - DTO가 필요한 이유
        - DTO 클래스를 이용한 코드의 간결성
            
            ⇒ **DTO 클래스가 클라이언트의 요청 데이터(Request Body)를 하나의 객체로 전달 받는 역할**
            
        - 데이터 **유효성 검증**의 단순화
            - **유효성 검증** : 서버 쪽에서 유효한 데이터를 전달 받기 위해 데이터를 검증하는 것
            - ex) 이메일 주소 입력으로 **형식에 어긋나는 입력**을 받을 경우
        - HTTP 요청의 수 감소 → 비용 절약
        
        > HTTP 요청을 전달 받는 핸들러 메서드는 요청을 전달 받는 것이 주 목적이기 때문에 최대한 간결하게 작성되는 것이 좋다.
        ⇒ DTO 클래스를 사용해서 유효성 검증 로직 구현.
        > 

- DTO 클래스 구현
    - `@Email` 애너테이션 : 클라이언트의 요청 데이터에 **유효한 이메일 주소가 포함되어 있지 않을 경우** 유효성 검증에 실패하기때문에 클라이언트의 요청거부
    - getter 메서드가 없으면 Response Body에 해당 멤버 변수의 값이 포함되지 않는 문제가 발생
        
        ```java
        public class MemberDto {
            @Email
            private String email;
            private String name;
            private String phone;
        		// getter 메서드
        		...
        ```
        
    - MemberDto 클래스에서 이메일에 대한 유효성 검증
        - `@Valid` 애너테이션 : MemberDto 객체에 유효성 검증을 적용하게 해주는 애너테이션
        
        ```java
        @RestController
        @RequestMapping("/v1/members")
        public class MemberController {
            @PostMapping
            public ResponseEntity postMember(@Valid MemberDto memberDto) {
                return new ResponseEntity<MemberDto>(memberDto, HttpStatus.CREATED);
            }
        ```
        
        → `@RequestParam` 을 각각 세 번 사용하여 Request Body를 전달 받은 것과 달리 `MemberPostDto` 객체로 Request Body를 한번에 전달 받음으로써 코드가 간결
        

- HTTP 요청/응답 데이터에 DTO 적용하기
    - HTTP Request Body가 Json형식인 경우
        - **`@RequestBody` 애너테이션**
            - JSON 형식의 Request Body를 MemberPostDto 클래스의 객체로 변환을 시켜주는 역할
        - **`@ResponseBody` 애너테이션**
            - JSON 형식의 Response Body를 클라이언트에게 전달하기 위해 DTO 클래스의 객체를 Response Body로 변환하는 역할
            - ResponseEntity 를 사용하면 ResponseBody 대체할 수 있다.
        - **JSON 직렬화(Serialization)와 역직렬화(Deserialization)**
            - **역직렬화** : JSON 형식의 데이터를 DTO 같은 Java의 객체로 변환
            - **직렬화** : Java의 객체를 JSON 형식으로 변환하는 것
            
            > JSON 직렬화(Serialization): Java 객체 → JSON
            JSON 역직렬화(Deserialization): JSON → Java 객체
            > 
    - HTTP Request Body가 Json형식이 아닌 경우
        
        

### DTO 유효성 검증

- DTO 클래스에 유효성 검증 적용
    
    > DTO 클래스에서 유효성 검증 로직이 실행되게 하기 위해서는 DTO 클래스에 `**@Valid**` **애너테이션을 추가**해야 한다.
    > 
    
    > @PathVariable이 추가된 변수에 유효성 검증이 정상적으로 수행되려면 클래스 레벨에 `**@Validated**`
     애너테이션을 반드시 붙여주어야 한다
    > 
    - `@NotBlank`
        - 정보가 비어있지 않은지를 검증
        - null 값이나 공백(””), 스페이스(” “) 같은 값들을 모두 허용하지 않음
    - `@Email`
        - 유효한 이메일 주소인지를 검증
    - `@Pattern`
        - 정규표현식(Reqular Expression)에 매치되는 유효한 패턴인지를 검증
        - 정보가 비어있으면 검증에 성공
        - 속성 : regexp, message
    
    > DTO 클래스에서 유효성 검증 로직이 실행되게 하기 위해서는 DTO 클래스에 `@Valid` 애너테이션을 추가해야 합니다.
    > 
    > 
    > ```java
    > @RestController
    > @RequestMapping("/v1/members")
    > public class MemberController {
    >     @PostMapping
    >     public ResponseEntity postMember(**@Valid** @RequestBody MemberPostDto memberDto) {
    >         return new ResponseEntity<>(memberDto, HttpStatus.CREATED);
    >     }
    > ```
    > 
    - 정규 표현식
        - ‘^’ : 문자열의 시작
        - ‘$’ : 문자열의 끝
        - ‘*’ : ‘*’ 앞에서 평가할 대상이 0개 또는 1개 이상인지를 평가
        - ‘\s’ : 공백 문자열
        - ‘\S’ : 공백 문자열이 아닌 나머지 문자열
        - ‘.’ : 임의의 문자 하나
        

- 쿼리 파라미터 및 `@PathVariable` 에 대한 유효성 검증
    - `**@Min(1)**` : memberId에 1 이상의 숫자일 경우에만 유효성 검증 통과
    - `**@Validated**` : URI path에 유효하지 않은 값을 입력해도 유효성 검증이 정상적으로 수행되도록 함.
        - @PathVariable이 추가된 변수에 유효성 검증이 정상적으로 수행되려면 반드시 필요함.
    
    ```java
    @RestController
    @RequestMapping("/v1/members")
    @Validated   // (1)
    public class MemberController {
    @PatchMapping("/{member-id}")
        public ResponseEntity patchMember(**@PathVariable**("member-id") **@Min(1)** long memberId,
                                        @Valid @RequestBody MemberPatchDto memberPatchDto) {
            memberPatchDto.setMemberId(memberId);
    ```
    

- Jakarta Bean Validation
    - **Jakarta Bean Validation** 은 라이브러리처럼 사용할 수 있는 API가 아닌 스펙(사양, Specification) 자체

- Custom Validator 를 사용한 유효성 검증
    - Custom Validator 구현 절차
        - Custom Annotation 정의
            
            ```java
            @Target(ElementType.FIELD)
            @Retention(RetentionPolicy.RUNTIME)
            @Constraint(validatedBy = {NotSpaceValidator.class}) // (1)
            public @interface NotSpace {
                String message() default "공백이 아니어야 합니다"; // (2)
                Class<?>[] groups() default {};
                Class<? extends Payload>[] payload() default {};
            }
            ```
            
        - Custom Annotation 에 바인딩 되는 Custom Validator 구현
            - Custom Validator 를 구현하기 위해서는 `ConstraintValidator` 인터페이스를 구현해야 한다.
            
            ```java
            public class NotSpaceValidator implements ConstraintValidator<NotSpace, String> {
            
                @Override
                public void initialize(NotSpace constraintAnnotation) {
                    ConstraintValidator.super.initialize(constraintAnnotation);
                }
            
                @Override
                public boolean isValid(String value, ConstraintValidatorContext context) {
                    return value == null || StringUtils.hasText(value);
                }
            }
            ```
            
        - 검증이 필요한 DTO 클래스의 멤버 변수에 Custom Annotation 추가
        

## 추가 학습

---

[DTO(Date Transfer Object)](https://www.notion.so/DTO-Date-Transfer-Object-a71d3264612b427d885eb9baabe0a7e9)

[[SpringMVC] Spring MVC Framework란 - Heee's Development Blog](https://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html)
