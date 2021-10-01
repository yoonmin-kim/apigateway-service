## 컨테이너 가상화
### 물리적인 컴퓨터 리소스를 다른 시스템이나 애플리케이션에서 사용할 수 있도록 제공
* 플랫폼 가상화
* 리소스 가상화
### 하이퍼바이저
* 다수의 운영체제를 동시에 실행하기 위한 논리적 플랫폼
* Type1: 하드웨어 위에 하이퍼바이저 그위에 다수의 운영체제를 실행(native 또는 bare metal 라고도 함)
* Type2: 하드웨어 위에 운영체제가 깔려있고 그운영체제에서 하이퍼바이저를 설치하여 다수의 운영체제를 실행(hosted 라고도 함)

### OS Virtualization
* Host OS 위에 Guest OS 전체를 가상화
* VMWare, VirtualBox
* 자유도가 높으나, 시스템에 부하가 많고 느려짐

### Container Virtualization
* Host OS가 가진 리소스를 적게 사용하며, 필요한 프로세스 실행
* 최소한의 라이브러리와 도구만 포함
* Container의 생성속도 빠름

```
Docker컨테이너를 사용하면 별도의 Guest OS 를 사용할 필요없이 Host OS의 자원을 컨테이너라는 
가상의 공간으로 완전 독립적으로 분리해줌으로써 리소스의 낭비를 최소화함
```

### Container
* 상태값x, Immutable
* image를 가지고 실체화 -> Container
