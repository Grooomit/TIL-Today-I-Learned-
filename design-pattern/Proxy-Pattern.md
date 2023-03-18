# Proxy Pattern

<!-- TOC -->

- [Proxy Pattern](#proxy-pattern)
  - [Proxy Pattern 이란?](#proxy-pattern-%EC%9D%B4%EB%9E%80)
  - [구조](#%EA%B5%AC%EC%A1%B0)
  - [원격 프록시와 가상 프록시 비교](#%EC%9B%90%EA%B2%A9-%ED%94%84%EB%A1%9D%EC%8B%9C%EC%99%80-%EA%B0%80%EC%83%81-%ED%94%84%EB%A1%9D%EC%8B%9C-%EB%B9%84%EA%B5%90)
  - [데코레이터 패턴과의 차이](#%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0-%ED%8C%A8%ED%84%B4%EA%B3%BC%EC%9D%98-%EC%B0%A8%EC%9D%B4)
  - [구현](#%EA%B5%AC%ED%98%84)
    - [with Java](#with-java)

<!-- /TOC -->

<br>

## Proxy Pattern 이란?

> 다른 객체에 대한 접근을 제어하기 위한 대리자 또는 자리채움자 역할을 하는 객체를 둔다. - GoF 디자인 패턴 -

> 특정 객체로의 접근을 제어하는 대리인(특정 객체를 대변하는 객체) 를 제공한다. - Head First Design Pattern -

- 프록시에서 접근을 제어하는 방법
  - 원격 프록시를 써서 원격 객체로의 접근을 제어할 수 있다.
  - 가상 프록시를 써서 생성하기 힘든 자원으로의 접근을 제어할 수 있다.
  - 보호 프록시를 써서 접근 권한이 필요한 자원으로의 접근을 제어할 수 있다.

## 구조

<img src="https://user-images.githubusercontent.com/124122215/225789741-ce976a66-2f6a-4fe9-a050-475e8b7e1f7a.png" width="70%" style="background-color:white;">

- Subject 구현체(Proxy)로 Request() 함수를 호출할 때 `RealSubject` 를 참조하고 있는 `Proxy`가 `RealSubject` 를 대리하는 구조

- Subject
  - Proxy 와 RealSubject 모두 Subject 인터페이스를 구현한다. 그러면 어떤 클라이언트에서든 프록시를 주제와 똑같은 식으로 다룰 수 있다.
- RealSubject
  - 진짜 작업을 대부분을 처리하는 객체
  - Proxy 는 그 객체로의 접근을 제어하는 객체
- Proxy
  - Proxy 에서 RealSubject 의 인스턴스를 생성하거나, 그 객체의 생성 과정에 관여하는 경우가 많다.
  - Proxy 에는 진짜 작업을 처리하는 객체의 레퍼런스가 들어있다.
  - 진짜 객체가 필요하면 그 레퍼런스를 사용해서 요청을 전달한다.

<br>

## 원격 프록시와 가상 프록시 비교

- 원격프록시
  - 원격 프록시는 다른 JVM에 들어있는 객체의 대리인에 해당하는 로컬 객체
  - 프록시의 메서드를 호출하면 그 호출이 네트워크에 전달되어 결국 원격 객체의 메서드가 호출된다.
- 가상프록시
  - 가상 프록시는 생성하는 데 많은 비용이 드는 객체를 대신한다.
  - 진짜 객체가 필요한 상황이 오기 전까지 객체의 생성을 미루는 기능을 제공

<br>

## 데코레이터 패턴과의 차이

> 데코레이터 : 보호 프록시는 특히 Decorator 로 보기 쉽다. 구조상 차이는 없으며 의도가 다를 뿐이다. Decorator 는 데코레이팅되지 않은 객체를 직접 접근하는 것을 허용한다.

> Decorator 디자인 패턴은 Proxy 패턴의 구조와 매우 유사하다. ConcreteComponent는 (=Proxy) 데코레이터를 통해 호출되는 몇 가지 동작을 구현한다. 두 클래스는 공통 기본 클래스로부터 상속받는다.
>
> > Decorator 와 Proxy 패턴간의 주요한 차이점은 그 의도에 있다. 데코레이터는 기능을 추가하거나, ConcreteComponent 의 핵심 기능에 추가 기능을 동적으로 선택할 수 있는 옵션을 제공한다.
>
> > 프록시는 세부적으로 정의된 하우스키핑 코드를 원본으로부터 분리하는 역할을 한다.

<br>

## 구현

### with Java

> 가상 파일 시스템에서 특정 파일에 대한 액세스를 제한하기 위해 보안 계층을 추가

```java
// virtual file system 인터페이스 정의
interface FileSyste {
    void readFile(String filename);
    void writeFile(String filename, String content);
}

// virtual file system 구현
class VirtualFileSystem implements FileSystem {
    public void readFile(String filename) {
        System.out.println("Reading file " + filename);
    }

    public void writeFile(String filename, String content) {
        System.out.println("Writing file " + filename + " with content: " + content);
    }
}

// virtual file system 에 보안 계층을 추가하는 Proxy 구현
class SecureFileSystem implements FileSystem {
    private FileSystem fileSystem;
    private List<String> authorizedFiles;

    public SecureFileSystem(FileSystem fileSystem, List<String> authorizedFiles) {
        this.fileSystem = fileSystem;
        this.authorizedFiles = authorizedFiles;
    }

    public void readFile(String filename) {
        if (authorizedFiles.contains(filename)) {
            fileSystem.readFile(filename);
        } else {
            System.out.println("Access denied to file " + filename);
        }
    }

    public void writeFile(String filename) {
        if (authorizedFiles.contains(filename)) {
            fileSystem.writeFile(filename, content);
        } else {
            System.out.println("Access denied to file " + filename);
        }
    }
}
```

```java
// 활용
public class Main {
    public static void main(String[] args) {
        // virtual file system 생성
        FileSystem fileSystem = new VirtualFileSystem();

        // 보안 파일 시스템 프록시를 생성하여 승인된 파일에만 액세스 허용
        List<String> authorizedFiles = Array.asList("file1.txt", "file2.txt");
        FileSystem secureFileSystem = new SecureFileSystem(fileSystem, authorizedFiles);

        // 승인된 파일 읽기 및 쓰기
        secureFileSystem.readFile("file1.txt"); // "Reading file file1.txt"
        secureFileSystem.writeFile("file2.txt", "Hello, World!"); // "Writing file file2.txt with content: Hello, World!"

        // 승인되지 않은 파일 읽기 및 쓰기
        secureFileSystem.readFile("file3.txt"); // "Access denied to file file3.txt"
        secureFileSystem.writeFile("file3.txt", "Goodbye, World!"); // "Access denied to file file3.txt"
    }
}
```
