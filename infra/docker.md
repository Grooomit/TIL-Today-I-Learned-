# Docker
<!-- TOC -->

- [Docker](#docker)
    - [Docker 란?](#docker-%EB%9E%80)
        - [Docker 를 사용하는 이유](#docker-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
        - [가상 머신과 도커 컨테이너](#%EA%B0%80%EC%83%81-%EB%A8%B8%EC%8B%A0%EA%B3%BC-%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88)
            - [가상머신](#%EA%B0%80%EC%83%81%EB%A8%B8%EC%8B%A0)
            - [도커 컨테이너](#%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88)
        - [What is a Container?](#what-is-a-container)
            - [특징](#%ED%8A%B9%EC%A7%95)
        - [What is a container image?](#what-is-a-container-image)
            - [특징](#%ED%8A%B9%EC%A7%95)
    - [도커 엔진](#%EB%8F%84%EC%BB%A4-%EC%97%94%EC%A7%84)
        - [도커 이미지와 컨테이너](#%EB%8F%84%EC%BB%A4-%EC%9D%B4%EB%AF%B8%EC%A7%80%EC%99%80-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88)
            - [도커 이미지](#%EB%8F%84%EC%BB%A4-%EC%9D%B4%EB%AF%B8%EC%A7%80)
            - [도커 컨테이너](#%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88)
        - [도커 컨테이너 다루기](#%EB%8F%84%EC%BB%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EB%8B%A4%EB%A3%A8%EA%B8%B0)
            - [run 과 create 명령어의 차이](#run-%EA%B3%BC-create-%EB%AA%85%EB%A0%B9%EC%96%B4%EC%9D%98-%EC%B0%A8%EC%9D%B4)
            - [도커 볼륨](#%EB%8F%84%EC%BB%A4-%EB%B3%BC%EB%A5%A8)
            - [도커 네트워크](#%EB%8F%84%EC%BB%A4-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
                - [브리지 네트워크](#%EB%B8%8C%EB%A6%AC%EC%A7%80-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
                - [호스트 네트워크](#%ED%98%B8%EC%8A%A4%ED%8A%B8-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
                - [None 네트워크](#none-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
                - [컨테이너 네트워크](#%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
            - [MacVLAN 네트워크](#macvlan-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
            - [컨테이너 로깅](#%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EB%A1%9C%EA%B9%85)
                - [syslog 로그](#syslog-%EB%A1%9C%EA%B7%B8)
            - [컨테이너 자원 할당 제한](#%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%9E%90%EC%9B%90-%ED%95%A0%EB%8B%B9-%EC%A0%9C%ED%95%9C)
        - [도커 이미지](#%EB%8F%84%EC%BB%A4-%EC%9D%B4%EB%AF%B8%EC%A7%80)
            - [도커 이미지 생성](#%EB%8F%84%EC%BB%A4-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%83%9D%EC%84%B1)
            - [이미지 구조 이해](#%EC%9D%B4%EB%AF%B8%EC%A7%80-%EA%B5%AC%EC%A1%B0-%EC%9D%B4%ED%95%B4)
            - [이미지 추출](#%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B6%94%EC%B6%9C)
            - [이미지 배포](#%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%B0%B0%ED%8F%AC)
        - [Dockerfile](#dockerfile)
        - [도커 데몬](#%EB%8F%84%EC%BB%A4-%EB%8D%B0%EB%AA%AC)
    - [도커 스웜](#%EB%8F%84%EC%BB%A4-%EC%8A%A4%EC%9B%9C)
    - [도커 컴포즈](#%EB%8F%84%EC%BB%A4-%EC%BB%B4%ED%8F%AC%EC%A6%88)

<!-- /TOC -->

<br>

## Docker 란?

> 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는데 소프트웨어 플랫폼

> docker 는 리눅스 컨테이너에 여러 기능을 추가함으로써 애플리케이션을 컨테이너로서 좀 더 쉽게 사용할 수 있게 만들어진 오픈소스 프로젝트

- 소프트웨어를 컨테이너라는 표준화된 유닛으로 패키징하며, 이 컨테이너에는 라이브러리, 시스템 도구, 코드 등 소프트웨어를 실행하는 데 필요한 모든 것이 포함

- 기존에 쓰이던 가상화 방법인 가상 머신과는 달리, 도커 컨테이너는 성능의 손실이 거의 없어서 차세대 클라우드 인프라 솔루션으로 사용 중

- 도커 관련 프로젝트
  - 도커 엔진 (Docker Engine)
  - 도커 컴포즈 (Docker Compose)
  - 레지스트리 (Private Registry)
  - 도커 허브 (Docker Hub)

<img src="https://user-images.githubusercontent.com/107091097/225026853-a6f63943-13ba-4335-a648-a44bd44e8e35.png" width="80%">

<br>

### Docker 를 사용하는 이유

> docker 를 사용하면 어디서나 안정적으로 실행할 수 있는 단일 객체를 확보

- Docker 를 사용하면 환경에 구애받지 않고 애플리케이션을 신속하게 배포 및 확장할 수 있다.

- 애플리케이션의 개발과 배포 편리
  - 호스트 OS 위에서 실행되는 독립된 컨테이너를 사용하여 호스트 OS 에 영향을 주지 않음
  - 도커 이미지라는 패키지로 만들어 전달함으로 배포 편리
- 여러 애플리케이션의 독립성과 확장성 상승
  - 도커를 통해 MSA 의 여러 모듈에게 독립된 환경을 동시에 제공할 수 있음
  - 이미지 버전을 독립적으로 관리하기 때문에 유지 보수 용이


### 가상 머신과 도커 컨테이너

<img src="https://user-images.githubusercontent.com/124122215/225486969-5d6aaf4e-fe29-4ee6-8fee-effade81caaf.png" width="60%">

<br>

#### 가상머신

- 기존의 가상화 기술은 Hypervisor 를 사용해 여러 개의 운영체제를 하나의 호스트에서 생성해 사용하는 방식
  - Hypervisor 를 반드시 거치기 때문에 일반 호스트에 비해 성능 손실
- 게스트 운영체제를 사용하기 위한 라이브러리, 커널 등을 전부 포함하기 때문에 가상 머신의 이미지 크기가 매우 커짐

> 완벽한 운영체제를 생성할 수 있지만, 성능손실과 매우 큰 이미지 사이즈

<br>

#### 도커 컨테이너

- 가상화된 공간을 생성하기 위해 리눅스의 자체 기능인 chroot, namespace, cgroup 을 사용함으로써 프로세스 단위의 격리 환경을 만들기 때문에 성능 손실이 거의 없음
- 컨테이너에 필요한 커널은 호스트의 커널을 공유하고, 컨테이너 안에는 애플리케이션 구동에 필요한 라이브러리 및 실행 파일만 존재
  - 이미지 사이즈 대폭 감소

> 작은 이미지 사이즈로 배포 시 가상 머신에 비해 빠르며, 가상화된 공간을 사용할 때의 성능 손실이 거의 없음

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

<br>

## 도커 엔진

> 도커 엔진에서 사용하는 기본 단위는 이미지와 컨테이너이며, 이 두가지가 도커 엔진의 핵심이다.

<br>

### 도커 이미지와 컨테이너

#### 도커 이미지

> 컨테이너를 생성할 때 필요한 요소이며, 가상 머신을 생성할 때 사용하는 iso 파일과 비슷한 개념

- 여러 개의 계층으로 된 바이너리 파일로 존재하고, 컨테이너를 생성하고 실행할 때 읽기 전용으로 사용
- 이미지의 이름은 `[저장소 이름]/[이미지 이름]:[태그]` 형태로 구성

  - 저장소 : 이미지 저장소인 도커 허브의 공식 이미지를 의미. 생략 가능
  - 이미지 이름 : 이미지의 역할을 의미
  - 태그 : 이미지의 버전 관리, 혹은 Revision 관리에 사용

- 이미 생성된 이미지는 어떠한 경우로도 변경되지 않는다.

<br>

#### 도커 컨테이너

> 이미지로 컨테이너를 생성하면 해당 이미지의 목적에 맞는 파일이 들어 있는 `파일시스템`과 격리된 `시스템 자원 및 네트워크`를 사용할 수 있는 독립된 공간이 생성되는데 이것을 `도커 컨테이너`라고 한다.

- 컨테이너는 이미지를 읽기 전용으로 사용하되 이미지에서 변경된 사항만 컨테이너 계층에 저장하므로 컨테이너에서 무엇을 하든지 원래 이미지는 영향을 받지 않는다.

<br>

### 도커 컨테이너 다루기

```sh
docker run -i -t \
--name myubuntu \
-e MYSQL_ROOT_PASSWORD=[password] \
-p 80:80 \
mysql:5.7
```

> \ 는 명령어의 길이가 길 때 가독성을 위해 설정옵션을 줄바꿈으로 구분한다.

- `docker run` : 컨테이너를 생성하고 실행하고 내부로 접속하는 역할
  - `-i -t` : 컨테이너와 상호 입출력을 가능하게 함
    - 컨테이너 내부로 진입하도록 attach 가능한 상태로 설정
  - `-d` : Detached 모드로 컨테이너를 실행
    - Detached 모드는 컨테이너를 백그라운드에서 동작하는 애플리케이션으로써 실행하도록 설정
  - `mysql:5.7` : 컨테이너를 생성하기 위한 이미지의 이름
  - `-e` : 컨테이너 내부의 환경변수 설정
    ```
    - 비밀번호처럼 민감한 정보를 컨테이너 내부의 환경 변수로 설정하는 것은 바람직하지 않다.
    - 이런 경우에는 도커 스웜 모드의 secret 이나 쿠버네티스의 secret 같은 기능을 활용해 안전하게 전달하는 것이 좋다.
    ```
  - `-p [호스트의 포트]:[컨테이너의 포트]` : 컨테이너의 포트를 호스트의 포트와 바인딩해 연결할 수 있게 설정

```sh
docker exec [컨테이너 이름] [shell 명령어]
docker exec -i -t [컨테이너 이름]
```

- `docker exec` : detach 모드로 실행 된 컨테이너 내부의 셸을 사용
  - `-i -t` 옵션을 사용하지 않으면, shell 명령어에 대한 결과만 반환

```sh
docker pull [이미지 이름]
docker images
```

- `docker pull` : 이미지를 내려받을 때 사용
- `docker images` : 도커 엔진에 존재하는 이미지 목록을 출력

```sh
docker create -i -t --name mycentos centos:7
```

- `docker create` : 컨테이너를 생성
  - `--name` : 컨테이너의 이름을 설정
  - run 명령어를 실행했을 때와 달리 컨테이너 내부로 들어가지 않는다.

```sh
docker start [컨테이너 이름]
docker attach [컨테이너 이름]
```

- `docker start` : 컨테이너 시작
- `docker attach` : 컨테이너 내부로 접속

<br>

#### run 과 create 명령어의 차이

- `docker run`
  - docker pull (이미지가 없을 때)
  - docker create
  - docker start
  - docker attach (-i -t 옵션을 사용했을 때 적용됨)
- `docker create`
  - docker pull (이미지가 없을 때)
  - docker create

<br>

```sh
docker ps
docker ps -a -q
```

- `docker ps` : 정지되지 않은 컨테이너 목록 출력
  - `-a` : 정지된 컨테이너를 포함한 모든 컨테이너 출력
  - `-q` : 컨테이너 ID만 출력

```sh
docker rm [컨테이너 이름]
```

- `docker rm` : 컨테이너를 삭제 (복구 불가)

<br>

#### 도커 볼륨

> 도커 이미지로 컨테이너를 생성하면 이미지는 읽기 전용이 되며, 컨테이너의 변경 사항만 별도로 저장해서 각 컨테이너의 정보를 보존한다.

- 컨테이너의 정보를 보존하는 방법 중 하나가 볼륨을 활용하는 것

1. 호스트와 볼륨을 공유하는 방법

   ```sh
   docker run -d \
   --name wordpressdb_hostvolume \
   -e MYSQL_ROOT_PASSWORD=password \
   -v /home/wordpress_db:/var/lib/mysql \
   mysql:5.7
   ```

   - `-v [호스트의 공유 directory]:[컨테이너의 공유 directory]`
     - 호스트의 `/home/wordpress_db` 디렉토리와 컨테이너의 `/var/lib/mysql` 디렉토리를 공유한다.
     - 두 개의 디렉토리는 동기화되는 것이 아니라 완전히 같은 디렉토리

   ```sh
   docker run -d \
   -e WORDPRESS_DB_PASSWORD=password \
   --name wordpress_hostvolume \
   --link wordpressdb_hostvolume:mysql \
   -p 80 \
   wordpress
   ```

   - `--link` : 컨테이너 내부 IP를 알 필요 없이 항상 컨테이너에 별명으로 접근하도록 설정
     - **컨테이너 내부 IP 가 변경돼도 별명으로 컨테이너를 찾을 수 있게 도커의 DNS에 의해 자동으로 관리**
     - 주의 : `--link`에 입력된 컨테이너가 실행중이지 않거나 존재하지 않는다면 `--link`를 적용한 컨테이너 또한 실행할 수 없다

2. 볼륨 컨테이너를 활용하는 방법

   ```sh
   docker run -i -t \
   --name volumes_from_container \
   --volumes-from [볼륨 컨테이너 이름] \
   ubuntu:14.04
   ```

   <img src="https://user-images.githubusercontent.com/124122215/225529189-fc488fb5-c478-48bf-8f3e-7b2e7b0d6997.png" width="70%">

   - `--volumes-from` : `-v` 또는 `--volume` 옵션을 적용한 컨테이너의 볼륨 디렉토리를 공유
     - 직접 볼륨을 공유하는 것이 아닌 `-v` 옵션을 적용한 컨테이너를 통해 공유하는 것

   > 즉, 볼륨을 사용하려는 컨테이너에 -v 옵션 대신 --volumes-from 옵션을 사용함으로써 볼륨 컨테이너에 연결해 데이터를 간접적으로 공유받는 방식

3. 도커가 관리하는 볼륨을 생성하는 방법

   - docker volume 명령어를 사용
     - 도커 자체에서 제공하는 볼륨 기능을 활용해 데이터를 보존할 수 있음

   ```sh
   docker volume create --name myvolume
   docker volume ls
   ```

   - 볼륨을 생성하고 생성된 볼륨 확인

   ```sh
   docker run -i -t --name myvolume_1 \
   -v myvolume:/root/ \
   ubuntu:14.04
   ```

   - `-v [볼륨의 이름]:[컨테이너의 공유 디렉토리]` : 컨테이너 공유 디렉토리에 파일을 쓰면 해당 파일이 볼륨에 저장
     - `-v` 옵션 대신 `--monut` 옵션을 사용할 수도 있다.
       - 두 옵션은 기능은 같지만, 볼륨의 정보를 나타내는 방법이 다름
       ```sh
       docker run -i -t --name mount_option_1 \
       --monut type=volume,source=myvolume,target=/root \
       ubuntu:14.04
       ```

   > 도커 볼륨도 호스트 볼륨 공유와 마찬가지로 호스트에 저장함으로써 데이터를 보존하지만 파일이 실제로 어디에 저장되는지 사용자는 알 필요가 없다.
   >
   > > `docker inspcet --type volume [볼륨이름]` : 볼륨의 실제 경로를 확인 가능
   >
   > > `docker volume prune` : 사용되지 않는 도커 볼륨을 한번에 삭제

<br>

#### 도커 네트워크

> 도커는 컨테이너에 내부 IP 를 순차적으로 할당하며, 이 IP는 컨테이너를 재시작할 때마다 변경될 수 있다. 이 내부 IP는 도커가 설치된 호스트, 즉 내부 망에서만 쓸 수 있는 IP 이므로 외부와 연결될 필요가 있다.

- 컨테이널르 생성하면 기본적으로 docker0 브리지를 통해 외부와 통신할 수 있는 환경을 사용할 수 있지만 사용자의 선택에 따라 여러 네트워크 드라이버를 쓸 수도 있다.
- 도커 자체 네트워크 드라이버
  - Bridge
    - 컨테이너를 생성할 때 자동으로 연결되는 docker0 브리지를 활용하도록 설정
  - Host
  - Container
  - Overlay
  - None

##### 브리지 네트워크

- docker0 이 아닌 사용자 정의 브리지를 새로 생성해 각 컨테이너에 연결하는 네트워크 구조
  - 컨테이너는 연결된 브리지를 통해 외부와 통신가능

```sh
// bridge 타입의 네트워크 생성
docker network create --driver bridge mybridge

docker run -i -t --name mynetwork_container \
--net mybridge \
ubuntu:14.04
```

- `--net` : 컨테이너가 해당 네트워크를 사용하도록 설정
- 생성된 사용자 정의 네트워크는 `disconnet, connect` 를 통해 컨테이너에 유동적으로 연결 가능

<br>

<img src="https://user-images.githubusercontent.com/124122215/225543972-3235e7d4-1d20-487b-8ae8-7893a1adf19f.png" width="70%">

`--net-alias`

```sh
docker run -i -t -d --name network_alias_container1 \
--net mybridge \
--net-alias alicek106 ubuntu:14.04

docker run -i -t -d --name network_alias_container2 \
--net mybridge \
--net-alias alicek107 ubuntu:14.04
```

- 브리지 타입의 네트워크와 run 명령어의 `--net-alias` 옵션을 함께 사용하면 컨테이너 이름이나 IP 주소 대신, 특정 호스트 이름으로 컨테이너 여러개를 접근할 수 있다.
- 특정 호스트 이름으로 접근할 때 DNS 서버는 라운드 로빈 방식을 이용해 컨테이너의 IP 리스트를 반환
  - 도커 엔진에 내장된 DNS 가 호스트 이름을 `-net--alias` 로 설정한 컨테이너로 변환하기 때문

<br>

##### 호스트 네트워크

- 네트워크를 호스트로 설정하면 호스트의 네트워크 환경을 그대로 쓸 수 있다.
- host 드라이버의 네트워크는 별도로 생성할 필요 없이 host 라는 이름의 네트워크 사용

```sh
docker run -i -t --name network_host \
--net host \
ubuntu:14.04
```

##### None 네트워크

- 말그대로 아무런 네트워크를 쓰지 않는 것
- 로컬호스트를 나타내는 lo 외네는 존재하지 않는다.

##### 컨테이너 네트워크

- `--net` 옵션으로 container 를 입력하면 다른 컨테이너의 네트워크 네임스페이스 환경을 공유할 수 있다.
  - `--net container:[다른 컨테이너의 ID 또는 이름]`
- 공유되는 속성은 내부 IP, 네트워크 인터페이스의 MAC 주소

#### MacVLAN 네트워크

- 호스트의 네트워크 인터페이스 카드를 가상화해 물리 네트워크 환경을 컨테이너에게 동일하게 제공
- MacVLAN 을 사용하면 컨테이너는 물리 네트워크상에서 가상의 Mac 주소를 가지며, 해당 네트워크에 연결된 다른 장치와의 통신이 가능해진다.

<br>

#### 컨테이너 로깅

```sh
docker logs [컨테이너 이름]
docker logs --tail 2 mysql
docker logs --since 1474765979 mysql
docker logs -f -t mysql
```

- `docker logs` : 컨테이너의 로그를 확인
  - `--tail N` : 마지막 줄부터 N 개 줄의 로그를 출력
  - `--since N` : N(유닉스 시간) 이후의 로그를 확인
  - `-t` : 타임스탬프 표시
  - `-f` : 로그를 스트림으로 확인

```sh
docker run -it \
--log-opt max-size=10k --log-opt max-file=3 \
--name log-test ubuntu:14.04
```

- `--log-opt` : 컨테이너 json 로그 파일의 최대 크기를 지정
- `--log-driver` : 로깅 드라이버를 사용하여 컨테이너 로그를 수집
  - 로그 설정을 하지않으면 기본적으로 JSON 형태로 저장
  - `syslog`, `journald`, `fluentd` ...

##### syslog 로그

- 유닉스 계열 운영체제에서 로그를 수집하는 오래된 표준

```sh
docker run -d --name syslog_container \
--log-driver=syslog \
ubuntu:14.04 \
echo syslogtest
```

- 기본적으로 로컬호스트의 syslog 에 저장하므로 운영체제 및 배포판에 따라 syslog 파일의 위치를 알아야 확인 가능

<br>

#### 컨테이너 자원 할당 제한

> 정리필요

<br>

### 도커 이미지

> 모든 컨테이너는 이미지를 기반으로 생성된다. 도커 이미지는 레이어로 구성된다.

- 도커는 기본적으로 도커 허브라는 중앙 이미지 저장소에서 이미지를 내려받는다.
  - `docker search [이미지 이름]` : 도커 허브에 해당 이미지가 있는지 확인

#### 도커 이미지 생성

- 도커 허브에서 pull 로 내려받을 수 있지만 특정 개발 환경을 직접 구축한 뒤 사용자만의 이미지를 직접 생성해야 한다.

```sh
// 컨테이너를 생성 한 후 변경사항을 만든 후
docker commit \
-a "alicek106" -m "my first commit" \
commit_test \
commit_test:first
```

- commit_test 라는 컨테이너를 commit_test:first 라는 이름의 이미지로 생성
- `-a` : 이미지의 작성자를 나타내는 메타데이터를 이미지에 포함
- `-m` : 커밋 메시지를 의미

<br>

#### 이미지 구조 이해

- 이미지를 커밋할 때 컨테이너에 변경된 사항만 새로운 레이어로 저장하고, 그 레이어를 포함해 새로운 이미지를 생성하기 때문에 전체 이미지의 실제 크기는 `기존 이미지의 크기 + commit 된 사항의 크기` 가 된다.

<br>

#### 이미지 추출

> save 나 export 와 같은 방법은 이미지를 단일 파일로 추출해서 이미지 파일의 크기가 너무 크거나, 도커 엔진의 수가 많다면 이미지를 파일로 배포하기 어렵다.
>
> > 도커의 이미지 구조인 레이어 형태를 이용하지 않으므로 매우 비효율적

- `docker save` : 도커 이미지를 별도로 저장하거나 옮기는 등 필요에 따라 이미지를 단일 binary 파일로 저장해야 하는 경우, 이미지의 모든 데이터를 하나의 파일로 추출할 수 있다.

  - `-o`: 추출될 파일명 입력
  - **추출된 이미지는 레이어 구조의 파일이 아닌 단일 파일이기 때문에, 이미지 용량을 각기 차지하게 된다.**

- `docker load -i [이미지 이름]` : 추출된 이미지는 load 명령어로 도커에 다시 로드 가능

```sh
docker export -o rootFS.tar mycontainer
docker import rootFS.tar myimage:0.0
```

<br>

#### 이미지 배포

> save 나 export 같은 방법은 이미지를 단일 파일로 추출하기 때문에 비효율적

이를 해결하기 위한 방법은

1. 도커 허브 이미지 저장소 사용

- `docker push` 로 이미지 올리기
- `docker pull` 로 이미지 가져오기

2. 도커 사설 레지스트리를 사용

- 사용자가 직접 이미지 저장소 및 서버, 저장공간 등을 관리해야 하므로 도커 허브보다 사용법이 까다롭다.

<br>

### Dockerfile

### 도커 데몬

## 도커 스웜

## 도커 컴포즈