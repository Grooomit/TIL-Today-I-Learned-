# Docker
<!-- TOC -->

- [Docker](#docker)
    - [Docker 란?](#docker-%EB%9E%80)
        - [Docker 를 사용하는 이유](#docker-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
        - [What is a Container?](#what-is-a-container)
            - [특징](#%ED%8A%B9%EC%A7%95)
        - [What is a container image?](#what-is-a-container-image)
            - [특징](#%ED%8A%B9%EC%A7%95)

<!-- /TOC -->

<br>

## Docker 란?

> 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는데 소프트웨어 플랫폼

- 소프트웨어를 컨테이너라는 표준화된 유닛으로 패키징하며, 이 컨테이너에는 라이브러리, 시스템 도구, 코드 등 소프트웨어를 실행하는 데 필요한 모든 것이 포함

<img src="https://user-images.githubusercontent.com/107091097/225026853-a6f63943-13ba-4335-a648-a44bd44e8e35.png" width="80%">

<br>

### Docker 를 사용하는 이유

> docker 를 사용하면 어디서나 안정적으로 실행할 수 있는 단일 객체를 확보

- Docker 를 사용하면 환경에 구애받지 않고 애플리케이션을 신속하게 배포 및 확장할 수 있다.

<br>

### What is a Container?

> "a container is a sandboxed process on your machine that is isolated from all other processes on the host machine" - docker document -

> 컨테이너는 애플리케이션이 한 컴퓨팅 환경에서 다른 컴퓨팅 환경으로 빠르고 안정적으로 실행될 수 있도록 코드와 모든 종속성을 패키징하는 표준 소프트웨어 단위

- 컨테이너 안에서 실행되는 애플리케이션은 host machine 에서 실행되는 다른 애플리케이션과 완전히 분리되어 있으며, 서로 영향을 미치지 않는다.
- 컨테이너는 host machine 의 리소스를 격리된 환경에서 사용하고, host machine 과의 상호작용은 제한적으로 허용
    - `a sandboxed process` 라고 부르는 이유
    - 더욱 안전하고 보안적으로 격리된 환경에서 실행할 수 있도록 한다.

#### 특징

- 실행 가능한 이미지 인스턴스
- 로컬 머신, 가상 머신에서 실행하거나 클라우드에 배포 가능
- 이식성이 뛰어남

<br>

### What is a container image?

> 컨테이너를 실행할 때는 격리된 파일 시스템을 허용한다. 이 사용자 정의된 파일 시스템은 컨테이너 이미지로 제공된다.

- 컨테이너 이미지는 런타임에 컨테이너가 되고, Docker 컨테이너의 경우 이미지가 Docker 엔진에서 실행될 때 컨테이너가 됩니다. Linux 및 Windows 기반 애플리케이션 모두에서 사용할 수 있는 컨테이너화된 소프트웨어는 인프라에 관계없이 항상 동일하게 실행

#### 특징 

- 이미지에는 컨테이너의 파일 시스템이 포함되어 있으므로 애플리케이션을 실행하는 데 필요한 모든 종속성, 구성, 스크립트, 바이너리 등 모든 것이 포함되어야 한다.
- 환경 변수, 실행할 명령어, 메타데이터 등 컨테이너에 대한 다른 구성도 포함되어야 한다.
