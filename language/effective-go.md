# Effective Go

<!-- TOC -->

- [Effective Go](#effective-go)
    - [introduction](#introduction)
    - [Formatting](#formatting)
    - [주석Commentary](#%EC%A3%BC%EC%84%9Dcommentary)
    - [명칭 - Names](#%EB%AA%85%EC%B9%AD---names)
        - [패키지명](#%ED%8C%A8%ED%82%A4%EC%A7%80%EB%AA%85)
        - [Getters](#getters)
        - [인터페이스명](#%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%AA%85)
        - [대소문자 혼합](#%EB%8C%80%EC%86%8C%EB%AC%B8%EC%9E%90-%ED%98%BC%ED%95%A9)
    - [세미콜론Semicolons](#%EC%84%B8%EB%AF%B8%EC%BD%9C%EB%A1%A0semicolons)
    - [제어구조](#%EC%A0%9C%EC%96%B4%EA%B5%AC%EC%A1%B0)
        - [If](#if)
        - [For](#for)

<!-- /TOC -->

<br>

## introduction

- Go 는 시스템 프로그래밍을 염두에 두고 설계된 범용 언어다. 강력한 typed 과 garbage-collected 이 존재하고, concurrent programming 을 명시적으로 지원한다.

- 프로그램은 패키지로 구성되며, 패키지의 속성을 통해 종속성을 효율적으로 관리할 수 있다.
  - 패키지는 같은 디렉토리에 있는 소스 파일들의 모음으로, 함께 컴파일된다.

<br>

## Formatting

> formatting 이슈는 중요한 것은 아니지만 가장 논쟁거리다. Go 에서는 대다수의 포맷팅 이슈를 자동으로 처리한다.

- `gofmt` 프로그램은 Go 프로그램을 읽은 뒤, 표준 스타일의 들여쓰기와 수직정렬, 유지 그리고 필요시 주석을 재포맷팅한 소스를 내놓는다.

  - 구조체의 필드에 적힌 주석 정렬

  ```go
  type T struct {
      name    string // name of the object
      value   int    // its value
  }
  ```

- 표준 패키지들에 있는 모든 Go 코드는 `gofmt`로 포맷팅 되어있다.
  - 들여쓰기
    - 탭을 사용하며, `gofmt` 는 기본값으로 탭을 사용한다.
    - 만약 꼭 써야하는 경우에만 spaces를 사용하라.
  - 괄호
    - 제어 구조들 (`if`, `for`, `switch`)의 문법엔 괄호가 없다.
    - 연산자 우선순위 계층이 간단하며 명확하다.
  - 한 줄 길이
    - Go 는 한 줄 길이에 제한이 없다.

<br>

## 주석(Commentary)

> Go언어는 C언어 스타일의 `/* */` 블럭주석과 C++스타일의 `//` 한줄 주석을 제공한다. 한줄주석은 일반적으로 사용되고, 블럭주석은 대부분의 패키지 주석에 나타난다.

- 프로그램 및 웹서버이기도 한 `godoc` 은 패키지의 내용에 대한 문서를 추출하도록 Go 소스 파일을 처리한다.
- 모든 패키지는 패키지 구문 이전에 블럭주석 형태의 패키지 주석이 있어야 한다.

  ```go
  /*
  Package regexp implements a simple library for regular expressions.

  The syntax of the regular expressions accepted is:
  ...
  */
  package regexp
  ```

- 별로 줄을 그어 쓰는 지나친 포캣은 주석에 필요없다. 스페이스나 정렬등에 의존하지 말라. 그런 것들은 `gofmt` 와 마찬가지로 `godoc`이 처리한다.
- [fmt package](https://pkg.go.dev/fmt)의 패키지주석은 좋은 예이다.

- 문서 주석은 매우 다양한 자동화된 표현들을 가능케 하는 완전한 문장으로 작성될 때 효과가 가장 좋다. 첫 문장은 선언된 이름으로 시작하는 한 줄짜리 문장으로 요약되어야 한다.
  ```go
  // Compile parses a regular expression and returns, if successful,
  // a Regexp that can be used to match against text.
  func Compile(str string) (*Regexp, error) {
  ```

<br>

## 명칭 - Names

> 이름의 첫 문자가 대문자인지 아닌지에 따라서 이름의 패키지 밖에서의 노출여부가 결정된다.

### 패키지명

- 패키지가 임포트되면, 패키지명은 패키지 내용들에 대한 접근자가 된다.
- underscores or mixedCaps 는 표기할 필요 없다.

```go
import "bytes"

func main() {
  a = bytes.Buffer()

}
```

<br>

### Getters

> Go 는 getters 와 setters 를 자체적으로 제공하지 않는다. 직접 만들어 사용하면 되지만, getter 의 이름에 Get 을 넣는건 Go 언어 답지도, 필수적이지도 않다.

```go
owner := obj.Owner()
if owner != user {
  obj.SetOwner(user)
}
```

<br>

### 인터페이스명

> 하나의 메서드를 갖는 인터페이스는 메서드 이름에 `-er` 접미사를 붙이거나 에이전트 명사를 구성하는 유사한 변형에의해 지정된다.

- 예를들면 Reader, Writer, Formatter, CloseNotifer 등이 있다.
- 문자 변환 메서드를 만든다면 `ToString` 이 아닌 `String` 을 사용하라.

<br>

### 대소문자 혼합

> Go 에서의 네이밍 규칙은 여러 단어로된 이름을 명명할 때 언더바(\_) 대신 대소문자 혼합(MixedCaps, mixedCaps)을 사용하는 것이다.

<br>

## 세미콜론(Semicolons)

> C언어 처럼, Go의 정식문법은 구문을 종료하기 위하여 세미콜론을 사용하지만, C언어와는 달리 세미콜론은 소스상에 나타나지 않는다.

- 구문분석기(lexer) 는 간단한 규칙을 써서 스캔을 하는 과정에 자동으로 세미콜론을 삽입한다.

  - 구문분석기는 구문을 끝낼 수 있는 토큰뒤에 새로운 라인이 오면, 세미콜론을 삽입한다.

  ```go
  break continue fallthrough return ++ -- ) }
  ```

  - 닫는 중괄호 바로 앞에서 생략할 수 있다.

- Go 에서는 세미콜론을 for loop 구문에서 변수 초기화와 조건, 그리고 진행 변수를 구분할때에만 사용 한다.

- 제어문(if, for ...) 의 여는 중괄호({) 를 다음 라인에 사용하지 말아야 한다.

<br>

## 제어구조

> Go 언어에서는 do 나 while 반복문이 존재하지 않으며, 좀 더 일반화된 for, 유연한 switch 가 존재한다.

- 타입 switch와 다방향 통신 멀티플렉서, select 의 새로운 제어 구조가 포함되어 있다.

### If

```go
if x > 0 {
  return y
}
```

- 중괄호를 의무적으로 사용해야 한다.
- if 와 switch 가 초기화 구문을 허용하므로 지역변수를 설정하기 위해 사용된 초기화 구문을 흔히 볼 수 있다.

  ```go
  if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
  }
  ```

- 변수의 단축선언, `v :=` 에서 변수 v 는 이미 선언되었더라도 다음의 경우 재선언이 가능하다.
  - 이 선언이 기존의 선언과 같은 스코프에 있어야 하고
  - 초기화 표현내에서 상응하는 값은 v에 할당할 수 있고,
  - 적어도 하나 이상의 새로운 변수가 선언문 안에 함께 있어야 한다.

<br>

### For

> Go 언어에서 for 반복문은 C언어와 비슷하지만 일치하지는 않는다. for 는 while 처럼 동작할 수 있고, 따라서 do-while 이 없다.

```go
// C언어와 같은 경우
for init; condition post { }

// C언어의 while 처럼 사용
for condition { }

// C언어의 for(;;) 처럼 사용
for { }
```

- 만약 배열, slice, string, map, 채널로 부터 읽어 들이는 반복문을 작성한다면, range 구문이 이 반복문을 관리해줄 수 있다.

  ```go
  for key, value := range oldMap {
    newMap[key] = value
  }
  ```

  ```go
  sum := 0
  for _, value := range array {
    sum += value
  }
  ```

- string의 경우, range 는 UTF-8 파싱에 의한 개별적인 유니코드 문자를 처리하는데 유용할 것이다. 잘못된 인코딩은 하나의 바이트를 제거하고 U+FFFD 룬 문자로 대체할 것이다.