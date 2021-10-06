## 컨테이너 가상화
### **물리적인 컴퓨터 리소스를 다른 시스템이나 애플리케이션에서 사용할 수 있도록 제공**
* 플랫폼 가상화
* 리소스 가상화
### **하이퍼바이저**
* 다수의 운영체제를 동시에 실행하기 위한 논리적 플랫폼
* Type1: 하드웨어 위에 하이퍼바이저 그위에 다수의 운영체제를 실행(native 또는 bare metal 라고도 함)
* Type2: 하드웨어 위에 운영체제가 깔려있고 그운영체제에서 하이퍼바이저를 설치하여 다수의 운영체제를 실행(hosted 라고도 함)

### **OS Virtualization**
* Host OS 위에 Guest OS 전체를 가상화
* VMWare, VirtualBox
* 자유도가 높으나, 시스템에 부하가 많고 느려짐

### **Container Virtualization**
* Host OS가 가진 리소스를 적게 사용하며, 필요한 프로세스 실행
* 최소한의 라이브러리와 도구만 포함
* Container의 생성속도 빠름

```
Docker컨테이너를 사용하면 별도의 Guest OS 를 사용할 필요없이 Host OS의 자원을 컨테이너라는 
가상의 공간으로 완전 독립적으로 분리해줌으로써 리소스의 낭비를 최소화함
```

### **Container**
* 상태값x, Immutable
* image를 가지고 실체화 -> Container

### **명령어**
\$ docker info // 도커 설치정보 <br>
\$ docker image ls // 도커 이미지 리스트<br>
\$ docker container ls // 도커 컨테이너 리스트<br>
\$ docker container rm \${컨테이너명} // 컨테이너 삭제<br>
\$ docker system prune // 실행 종료된 컨테이너를 캐시포함 전부 삭제<br>
\$ docker logs \${컨테이너명} // 컨테이너 실행시 생성된 로그확인<br>
\$ docker run[OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND][ARG...]<br>
```
-d 'detached mode' 흔히말하는 백그라운드 모드
-p 호스트와 컨테이너의 포트를 연결(포워딩)
-v 호스트와 컨테이너의 디렉토리를 연결(마운트)
-e 컨테이너 내에서 사용할 환경변수 설정
--name 컨테이너 이름 설정
--rm 프로세스 종료시 컨테이너 자동 제거
-it -i와-t를 동시에 사용한 것으로 터미널 입력을 위한 옵션
--link 컨테이너 연결[컨테이너명:별칭]
```
<hr>

### **mysql 예시**
\$ docker pull mysql:5.7

\$ docker run -d -p 13306:3306 -e MYSQL_EMPTY_PASSWORD=true --name mysql mysql:5.7<br>
`
-d 백그라운드로 실행하며 -p 호스트의 3306포트와 컨테이너의 3306포트를 포워딩한다.
-e 환경변수로 MYSQL_EMPTY_PASSWORD=true 라는 옵션을 주고 --name으로 컨테이너 명을 지정한다.
컨테이너명을 지정하지 않을 시 랜덤한 값이 부여된다. 마지막으로 컨테이너를 실행할때 사용될 이미지명을 명시한다.
`

\$ docker exec -it mysql bash<br>
`
이미 실행된 컨테이너에 추가적인 명령을 전달할때 exec를 사용한다.
`

<img src="https://user-images.githubusercontent.com/46228593/135862100-68f0e5a0-ba78-4783-a093-93ddd2a2a873.png" width="300"/>

<img src="https://user-images.githubusercontent.com/46228593/135862892-eaacdcd7-6540-4226-93cf-31840b153d4d.png" width="500" heigh="400"/>

<img src="https://user-images.githubusercontent.com/46228593/135863042-30a81ae3-4a1a-4983-94d7-2b461171368c.png" width="500" heigh="400"/>
<hr>

### **Dockerfile, 도커 이미지 업로드**
```
FROM openjdk:17-ea-11-jdk-slim 
VOLUME /tmp 
COPY target/user-service-1.0.jar UserService.jar 
ENTRYPOINT ["java","-jar","UserService.jar"] 
```


\$ docker build --tag rladbsals23/user-service:1.0 . <br>
`도커파일 이용해서 빌드`<br>
\$ docker push rladbsals23/user-service:1.0 <br>
`도커 허브에 업로드`<br>
![image](https://user-images.githubusercontent.com/46228593/136013836-caffa4f8-d6ca-4f32-a9f9-bcedd9eee191.png)
<hr>

### **Bridge network**
\$ docker network create --driver bridge [브릿지이름] <br>
`기본적으로 설정되는 네트워크이다.
호스트PC의 네트워크와 분리된 가상의 네트워크를 만들고 가상의 네트워크에서 컨테이너를 배치하여 사용
`
### **Host network**
- 네트워크를 호스트로 설정하면 호스트의 네트워크 환경을 그대로 사용
- 포트포워딩 없이 내부 어플리케이션 사용
### **None network**
- 네트워크를 사용하지 않음
- IO 네트워크만 사용, 외부와 단절

\$ docker network create ecommerce-network <br>
\$ docker network ls

![image](https://user-images.githubusercontent.com/46228593/136195907-9a656294-b5bc-4f17-b5d6-1cd68e4448d8.png)<br>
컨테이너의 ip-address를 수동으로 지정할 걸 대비해서 gateway, subnetMask를 지정한다

![image](https://user-images.githubusercontent.com/46228593/136196071-4467dded-4544-4eee-a5e2-49cdcc930c07.png)<br>
docker network inspect 명령을 통해 지정한 네트워크의 상세정보를 확인한다

```
여러개의 마이크로 서비스를 각각의 컨테이너로 만들어서 운영을 하게 되면 다음과 같은 문제가 발생할 수 있다.

처음 생성된 컨테이너의 ip가 172.0.0.1 이라고 하면 12번째 컨테이너의 ip는 172.0.0.12가 된다.(순차적으로 증가) 하지만
만약 컨테이너를 구성하는 서버가 변경되어 컨테이너를 다시 생성할 일이 발생하게 될 경우 처음 생성하는 컨테이너의 
ip가 172.0.0.1이 아닐 수 있다. 그러면 ip로 통신하던 마이크로 서비스의 설정정보를 변경해야되는 일이 발생하게 된다. 

하지만, 같은 네트워크로 구성된 컨테이너들 끼리는 컨테이너ID, 혹은 컨테이너NAME으로 통신이 가능하게 된다. 
마치 유레카 서버를 이용하여 서비스명으로 통신하는 것과 비슷한 방식으로 운영이 가능해진다.
```
