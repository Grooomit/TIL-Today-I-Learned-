# Spring Testing

## 단위 테스트

### 테스트 종류

- **기능 테스트**
    - 애플리케이션을 사용하는 사용자 입장에서 애플리케이션이 제공하는 기능이 올바르게 동작하는 테스트
- **통합 테스트**
    - 클라이언트 측 툴 없이 개발자가 짜 놓은 테스트 코드를 실행시켜서 이루어지는 테스트
- **슬라이스 테스트**
    - 애플리케이션을 특정 계층으로 쪼개어서 하는 테스트
    - Mock(가짜) 객체를 사용해서 계층별로 끊어서 테스트 한다.
    - (=부분 통합 테스트)
- **단위 테스트**
    - 메서드 단위로 테스트가 동작한다.
    - 최대한 독립적인 것이 좋다.
    - Postman으로 HTTP요청을 보내서 테스트하는 과정을 단순화 할 수 있다.

### 단위 테스트를 위한 F.I.R.S.T 원칙

- **Fast** : 테스트 케이스는 빨라야 한다.
- **Independent** : 각각의 테스트 케이스는 독립적이어야 한다.
- **Repeatable** : 테스트 케이스는 어떤 환경에서도 반복해서 실행이 가능해야 된다.
- **Self-validating** : 단위 테스트는 성공 또는 실패라는 자체 검증 결과를 보여주어야 한다.
- **Timely** : 테스트 하려는 기능 구현을 하기 직전에 작성해야 한다.

## JUnit을 사용한 단위 테스트

### JUnit이란?

- Java 언어로 만들어진 애플리케이션을 테스트 하기 위한 오픈 소스 테스트 프레임워크로서 사실상 Java의 표준 테스트 프레임워크이다.

- JUnit 기본 작성법
    - **JUnit을 사용한 테스트 케이스의 기본 구조**
        - `@Test` 테스트 하고자 하는 대상에 추가한다.
            
            ```java
            public class JunitDefaultStructure {
            		// (1)
                @Test
                public void test1() {
                    // 테스트 하고자 하는 대상에 대한 테스트 로직 작성
                }
            		// (2)
                @Test
                public void test2() {
                    // 테스트 하고자 하는 대상에 대한 테스트 로직 작성
                }
            		// (3)
                @Test
                public void test3() {
                    // 테스트 하고자 하는 대상에 대한 테스트 로직 작성
                }
            }
            ```
            
    
    - **Assertion 메서드 사용하기**
        - Assertion : 예상하는 결과 값이 ‘참’이길 바라는 논리적인 표현
        - `assertEquals()` : 기대하는 값과 실제 결과 값이 같은지 검증
            
            ```java
            public class HelloJUnitTest {
                @DisplayName("Hello JUnit Test")  // (1)
                @Test
                public void assertionTest() {
                    String expected = "Hello, JUnit";
                    String actual = "Hello, JUnit";
            
                    assertEquals(expected, actual); // (2)
                }
            }
            ```
            
        - `assertNotNull()` : Null 여부 테스트
            
            ```java
            public class AssertionNotNullTest {
            
                @DisplayName("AssertionNull() Test")
                @Test
                public void assertNotNullTest() {
                    String currencyName = getCryptoCurrency("ETH");
            
            				// (1) assertNotNull(테스트 대상, 테스트 실패시 메시지);
                    assertNotNull(currencyName, "should be not null");
                }
            
                private String getCryptoCurrency(String unit) {
                    return CryptoCurrency.map.get(unit);
                }
            }
            ```
            
        - `assertThrows()` : 예외(Exception) 테스트
            
            <aside>
            💡 예외 타입이 다를 경우 “failed” 가 출력되지만, 해당 예외타입을 상속하는 상위 타입을 예외 클래스에 넣을 경우 “passed”가 출력된다.
            
            </aside>
            
            ```java
            public class AssertionExceptionTest {
            
                @DisplayName("throws NullPointerException when map.get()")
                @Test
                public void assertionThrowExceptionTest() {
                    // (1) assertThrows(예외 클래스, 람다식(테스트 대상 메서드 호출));
                    assertThrows(NullPointerException.class, () -> getCryptoCurrency("XRP"));
                }
            
                private String getCryptoCurrency(String unit) {
                    return CryptoCurrency.map.get(unit).toUpperCase();
                }
            }
            ```
            
    
    - 테스트 케이스 실행 전, 전처리
        - `@BeforeEach` : 테스트 케이스(메서드)가 각각 실행될 때마다 테스트 케이스 실행 직전에 먼저 실행되어 초기화 작업 등을 진행할 수 있다.
            
            ```java
            public class BeforeEach2Test {
                private Map<String, String> map;
            
                @BeforeEach
                public void init() {
                    map = new HashMap<>();
                    map.put("BTC", "Bitcoin");
                    map.put("ETH", "Ethereum");
                    map.put("ADA", "ADA");
                    map.put("POT", "Polkadot");
                }
            
                @DisplayName("Test case 1")
                @Test
                public void beforeEachTest() {
                    map.put("XRP", "Ripple");
                    assertDoesNotThrow(() -> getCryptoCurrency("XRP"));
                }
            
                @DisplayName("Test case 2")
                @Test
                public void beforeEachTest2() {
                    System.out.println(map);
                    assertDoesNotThrow(() -> getCryptoCurrency("XRP"));
                }
            		// Test case 2가 실행되기 전에 init() 메서드가 호출되면서 map 객체초기화
            		// => NullpointerException 발생, "failed"
            
                private String getCryptoCurrency(String unit) {
                    return map.get(unit).toUpperCase();
                }
            }
            ```
            
        - `@BeforeAll` : 클래스 레벨에서 테스트 케이스를 한꺼번에 실행시키면 테스트 케이스가 실행되기 전에 딱 한번만 초기화 작업을 할 수 있도록 해주는 애너테이션
            - BeforeAll을 사용해서 map 객체를 한번만 초기화 ⇒ 두개의 테스트 케이스 모두 passed이다.
            
            ```java
            public class BeforeAllTest {
                private static Map<String, String> map;
            
                @BeforeAll
                public static void initAll() {
                    map = new HashMap<>();
                    map.put("BTC", "Bitcoin");
                    map.put("ETH", "Ethereum");
                    map.put("ADA", "ADA");
                    map.put("POT", "Polkadot");
                    map.put("XRP", "Ripple");
            
                    System.out.println("initialize Crypto Currency map");
                }
            
                @DisplayName("Test case 1")
                @Test
                public void beforeEachTest() {
                    assertDoesNotThrow(() -> getCryptoCurrency("XRP"));
                }
            
                @DisplayName("Test case 2")
                @Test
                public void beforeEachTest2() {
                    assertDoesNotThrow(() -> getCryptoCurrency("ADA"));
                }
            
                private String getCryptoCurrency(String unit) {
                    return map.get(unit).toUpperCase();
                }
            }
            ```
            
    
    - **테스트 케이스 실행 후, 후처리**
        - `@AfterEach`
        - `@AfterAll`
    
    - **Assumption을 이용한 조건부 테스트**
        - Assumption : ~라고 가정하고
        - `assumTrue()` : 파라미터로 입력된 값이 True이면 나머지 아래 로직들을 실행
            
            ```java
            public class AssumptionTest {
                @DisplayName("Assumption Test")
                @Test
                public void assumptionTest() {
                    // (1)
                    assumeTrue(System.getProperty("os.name").startsWith("Windows"));
            //        assumeTrue(System.getProperty("os.name").startsWith("Linux")); // (2)
                    System.out.println("execute?");
                    assertTrue(processOnlyWindowsTask());
                }
            
                private boolean processOnlyWindowsTask() {
                    return true;
                }
            }
            ```
            
    
- JUnit으로 비즈니스 로직에 단위 테스트 적용하기
    
    ```java
    public class StampCalculatorTestWithoutJUnit {
        public static void main(String[] args) {
            calculateStampCountTest();        // (1) 첫 번째 단위 테스트
            calculateEarnedStampCountTest();  // (2) 두 번째 단위 테스트
        }
    
        private static void calculateStampCountTest() {
            // given
            int nowCount = 5;
            int earned = 3;
    
            // when
            int actual = StampCalculator.calculateStampCount(5, 3);
    
            int expected = 7;
    
            // then
            System.out.println(expected == actual);
        }
    
        private static void calculateEarnedStampCountTest() {
            // given
            Order order = new Order();
            OrderCoffee orderCoffee1 = new OrderCoffee();
            orderCoffee1.setQuantity(3);
    
            OrderCoffee orderCoffee2 = new OrderCoffee();
            orderCoffee2.setQuantity(5);
    
            order.setOrderCoffees(List.of(orderCoffee1, orderCoffee2));
    
            // when
            int actual = StampCalculator.calculateEarnedStampCount(order);
    
            int expected = 8;
    
            // then
            System.out.println(expected == actual);
        }
    }
    ```