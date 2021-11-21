# Docker

기존에는 서버를 관리하는데 있어서 매우 복잡하고, 계속해서 서버 환경과 개발 환경이 변경되었다. 예를 들면 서버 환경이 AWS에서 Azure로 바뀌거나, 또는 기존에는 프로그램이 python 기반이였는데 java 또는 Node.js로 바뀌는 예가 있다.

그러나 **Docker가 등장**하면서 **서버 관리/개발 방식이 완전히 바뀌게** 되었다.



기존의 서버관리 방식

Add user -> System Env -> Firewall -> Network -> Dependencies -> Python

 -> Git clone -> Package -> Configuration -> Migration -> Proxy -> Run



**Docker**가 등장하고 나서 모든 프로그램은 **컨테이너로 추상화** 된다.

~~~ 
Container   Container   Container 
 --------    --------    --------
| MYSQL  |  |JENKINS |  |LINUX   |
|				 |  |        |  |				 |
 --------    --------	   --------   
~~~



### 도커와 가상머신

도커를 가상머신과 비슷하다고 생각할 수 있지만 엄연히 말하면 반은 맞고 반은 틀리다.

##### 비슷한 점

- 가상 머신처럼 독립적으로 실행된다.
  - 각 Container들은 독립적으로 실행된다.

##### 도커를 가상머신과 비교했을때 장점

- 가상머신보다 빠르다.
  - 가상머신은 리눅스 부팅부터 시작해서 느리지만, docker는 명령어 하나만 쳐도 컨테이너가 올라가진다.
- 가상머신보다 쉽다.
  - 명령어 하나만으로 컨테이너를 올릴 수 있다.
- 가상머신보다 효율적이다.
  - 도커는 HostOs위에서 자원만 공유하기 때문에 상대적으로 매우 가벼우나, 가상머신은 HostOs 위에 각각의 Guest Os를 설치하기 때문에 OS를 각각 포함하고 있어서 매우 무겁고, 각각마다 자원이 할당되어서 도커보다 더 비효율적이다.

![DockerContainer_VM_Architecture](https://github.com/twkim5235/Docker/blob/main/picture/DockerContainer_VM_Architecture.png)



## 도커의 등장

##### 컨테이너: 격리된 환경에서 작동하는 프로세스 

##### 리눅스 커널의 여러기술을 활용

##### 하드웨어 가상화 기술보다 가벼움

##### 이미지 단위로 프로세스 실행 환경을 구성



### 도커란?

#### 도커의 특징 

#### 확장성/이식성

- 도커가 설치되어 있다면 어디서든 컨테이너를 실행할 수 있음
  - 다른 서버에서 만든 도커이미지만 가지고 컨테이너를 실행할 수 있다.
- 특정 회사나 서비스에 종속되지 않음
- 쉽게 개발서버를 만들 수 있고 테스트서버 생성도 간편함



#### 표준성

- 도커를 사용하지 않는 경우 ruby, node.js, go 등으로 만든 서비스들의 배포 방식은 제각각 다르다.
- 컨테이너라는 표준으로 서버를 배포하므로 모든 서비스들의 배포과정이 동일해진다.



#### 이미지

- 이미지에서 컨테이너를 생성하기 때문에 반드시 이미지를 만드는 과정이 필요하다.
- Dockerfile을 이용하여 이미지를 만들고 처음부터 재현이 가능하다.
- 빌드 서버에서 이미지를 만들면 해당 이미지를 이미지 저장소에 저장하고 운영서버에서 이미지를 불러온다.



#### 설정관리

- 설정은 보통 환경변수로 관리한다.
- MYSQL_PASS=password와 같이 컨테이너를 띄울때 환경변수를 같이 지정한다.
- 하나의 이미지가 환경변수에 따라 동적으로 설정파일을 생성하도록 만들어져야 한다.



#### 자원관리

- 컨테이너는 삭제 후 새로 만들면 모든 데이터가 초기화 된다.
- 업로드 파일을 외부 스토리지와 링크하여 사용하거나 S3같은 별도의 저장소가 필요하다.
- 세션이나 캐시를 memcached나 redis와 같은 외부로 분리한다.

### 컨테이너의 미래

### 쿠버네티스(kubernetes)

- **여러대의 서버**와 **여러개의 서비스**를 **관리**하기 쉽게 해주는 프로그램



### 쿠버네티스 특징

#### 스케줄링

- 컨테이너를 적당한 서버에 배포해주는 작업

- 여러대의 서버중 가장 여유로운 서버에 컨테이너를 배포하거나 그냥 차례대로 배포 또는 랜덤하게 배포해준다.
- 컨테이너 개수를 여러개로 늘리면 적당히 나눠서 배포하고 서버가 죽으면 실행중이던 컨테이너를 다른 서버에 띄워준다.



#### 클러스터링

- 여러 개의 서버를 하나의 서버처럼 사용한다.
- 작게는 몇 개 안 되는 서버부터 많게는 수천 대의 서버를 하나의 클러스터로 관린한다.
- 여기저기 서버에 흩어져 있는 컨테이너도 가상 네트워크를 이용하여 마치 같은 서버에 있는 것처럼 쉽게 통신이 가능하다.



#### 서비스 디스커버리

- 서비스를 찾아주는 기능
  - MySQL 과 Java Was를 다른 서버에 컨테이너로 실행되었는데, 하지만 두 컨테이너를 연동해야 되기 때문에 서비스를 찾아주는 기능이 필요하다.
- 클러스터 환경에서 컨테이너는 어느 서버에 생성될지 알 수 없고 다른 서버로 이동할 수도 있다.
- 따라서 컨테이너와 통신을 하기 위해서 어느 서버에서 실행중인지 알아야 하고 컨테이너가 생성되고 중지될 때 어딘가에 IP와 Port같은 정보를 업데이트해줘야 한다.
- 키 - value 스토리지에 정보를 저장할 수도 있고 내부 DNS 서버를 이용한다.

### 도커 기본 명령어

#### run 

**컨테이너 실행 명령어** 

~~~dockerfile
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
~~~



 [OPTIONS] 주요 예약어

|   이름    |                             기능                             |
| :-------: | :----------------------------------------------------------: |
|    -d     |                detached mode(백그라운드 모드)                |
|    -p     | 호스트와 컨테이너의 포스트를 연결 <br>ex) docker run -p 8080(호스트 포트):80(컨테이너 포트) <br>호스트의 8080포트로 접속을 시도하면 컨테이너의 80포트로 접속된다. |
|    -v     |             호스트와 컨테이너의 디렉토리를 연결              |
|    -e     |             컨테이너 내에서 사용할 환경변수 설정             |
|  --name   |                      컨테이너 이름 설정                      |
|   --rm    |              프로세스 종료시 컨테이너 자동 제거              |
|    -it    |    -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션    |
| --network |                        네트워크 연결                         |



#### M1환경에서 docker로 Mysql을 실행할때 주의점

~~~
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql
~~~

상단과 같이 명령어를 입력하면

docker: no matching manifest for linux/arm64/v8 in the manifest list entries

같은 에러가 발생한다.



그러므로

~~~
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  --platform linux/amd64  mysql:5.7
~~~

  --platform linux/amd64 을 넣어서 실행해 주면 문제없이 실행된다.



#### exec

**run은 실행명령어라고 하면, exec는 컨테이너에 접속하게 해주는 명령어 이다.**
~~~ 
docker exec <CONTAINER_ID> <COMMAND>
~~~

#### ps

**실행중인 컨테이너 목록을 확인 하는 명령어이다.**

~~~dockerfile
docker ps
~~~

**중지된 모든 컨테이너까지 다 볼  수 있는 명령어이다.**
-a 추가

~~~
docker ps -a
~~~



#### stop

**실행중인 컨테이너를 중지하는 명령어이다.**

**실해중인 컨테이너를 하나 또는 여러개(띄어쓰기)를 중지할 수 있다.**

~~~dockerfile
docker stop [OPTIONS] CONTAINER(단일) CONTAINER CONTAINER...(띄워쓰기로 여러개)
~~~

ID나 컨테이너 이름으로 중지를 한다.



#### logs

**컨테이너의 log를 확인할 수 있는 명령어이다.**

~~~
docker logs [OPTIONS] CONTAINER
~~~

**-f** 옵션을 사용하면 실시간으로 log를 확인할 수 있다.



#### images

**도커가 다운로드한 이미지 목록을 확인할 수 있는 명령어이다.**

~~~
docker images [OPTIONS] [REPOSITORY[:TAG]]
~~~



#### pull

**도커의 이미지를 다운로드하는 명령어이다.**

~~~
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
~~~



#### rmi

**도커의 이미지를 삭제하는 명령어이다.**

~~~
docker rmi [OPTIONS] IMAGE(단일) IMAGE IMAGE...(띄워쓰기로 여러개)
~~~



#### network create 

**도커 컨테이너끼리 이름으로 통신할 수 있는 가상 네트워크를 생성하는 명령어이다.**

~~~
docker network create [OPTIONS] NETWORK
~~~

ex)

~~~
docker network create app-network
~~~



#### network connect

**기존에 생성된 컨테이너에 네트워크를 추가한다.**

~~~
docker network connect [OPTIONS] NETWORK CONTAINER
~~~

ex)

~~~
docker network connect app-network mysql
~~~



#### network option 

**컨테이너를 연결할 network를 옵션에 추가한다.**

ex)

~~~
docker run -d 0p 8080:80 \
 --network=app-network \ 연결할 네트워크
 --WORDPRESS_DB_HOST=mysql\ wordpress에 연결할 DB - (네트워크에 속해있는 mysql을 연동함)
 ...
 wordpress
~~~
