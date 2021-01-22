# docker-Kubernates
#도커와 쿠버네티스를 이해해보자

----
## 0. 소개
이제는 안쓰는 사람이 없는 docker 그리고 이걸 관리해주는 Kubernates 
구글 Cloud jam 과정에서 배우긴 했는데,, 안쓰다 보니 사실상 다 까먹은것 같다,,
이곳에서는 docer의 기본 개념과 사용법 명령어등을 다루고 최종적으로는 지금 하고 있는 프로젝트와 유사한 환경을 만들고 배포 해보도록 하려한다.
순전히 내가 쓸 목적으로,, 
차례는 아래와 같으며 코드는 커맨드 명령어외에 shell script 정도가 나올거 같은데 쥐뿔도 모른다,,
이곳에서 사용해보고 익혀야 겠다..

## 0-1. 참고자료 
- [Docker Tutorial for Beginners - A Full DevOps Course on How to Run Applications in Containers (Youtube)](https://www.youtube.com/watch?v=fqMOX6JJhGo)
- [초보자를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
- [kodekloud (무료 도커 예제)](https://kodekloud.com/p/docker-labs)

## 0-2. 차례
 
개념
1. What are Containers?
2. What is Docker?
3. Why do you need it?
4. What can it do?

실습
5. Run Docker Containers
6. Create a Docker Image
7. Networks in Docker
8. Docker Compose

9. Docker Concepts in Depth

기타
10. Docker for Windows/Mac

11. Docker Swarm
12. Docker vs Kubernates

---

## 1. What are Container?
컨테이이란 무엇일까? __컨테이너__ 란 호스 OS상에 논리적인 구획(컨테이너)를 만들고, 어플리케이션을 작동시키기 위해 필요한 라이브러리나 어플리케이션 등을 모아, 별도의 서버인 것처럼 사용할 수 있게 만든 것이라고 한다. 아 도커 컨테이너가 최초의 컨테이너 기술은 아니다. Docker는 이전 Linux의chroot에서 부터 시작된 리눅스 커널 격리 기술의 'LXC'에 근간이 있다. 

컨테이너 역사를 알면 도커의 개념을 잡는데 조금더 쉬울수 있으니 리눅스 컨테이너를 잠시 알아보자,

### 1-1. Linux Container
리눅스 컨테이너는 운영체제 수준의 가상화 기술로 리눅스 커널을 공유하면서 프로세스를 격리된 환경에서 실행하는 기술이다. 하드웨어를 가상화하는 가상 머신과 달리 커널을 공유하는 방식이기 때문에 실행 속도가 빠르고, 성능의 손실이 거의 없다, 컨테이너로 실행된 프로세는 말한것 처럼 커널을 공유하지만, CGROUP, Namespaces, Root DIR격리등의 커널 기술을 활용해 격리되어 실행된다.   

커널을 격리함으로써 기존의 서로 다른 플랫폼에 대한 제약을 해소할 수 있으며, 리소스 관리르 보다 더 효율적으로 사용할 수 있게 되었다.

![](https://blog.netapp.com/wp-content/uploads/2016/03/Screen-Shot-2018-03-20-at-9.24.09-AM-935x500.png)
(https://blog.netapp.com/)

위의 그림처럼 가상화 기술은 Host OS, Guest OS 두개의 커널을 지니며, Container는 한개의 커널을 지난다. 이점은 애플리 케이션의 오버헤드와 큰 연광성이 있다. (hypervisor위에 guestOs가 올라가는데 이때 Binary와 라이브러리 드응ㄹ 모두 구성해야 하기에 무겁고 성능저하가 발생한다 = 오버헤드 발생)

[VM과 Container의 비교](https://medium.com/@lhs6395/container%EC%99%80-vm%EC%9D%98-%EB%B9%84%EA%B5%90-84f6a8b7cd4c)  
위의 medium에선 이러한 내용을 자세히 다루고 있으니 확인해도 좋을것 같다.  

이에 반해 컨테이너 기술을 사용하면 OS나 디렉토리 IP 주소 등과 같은 시스템 자원을 마치 각 어플리케이션이 점유하고 있는것 처럼 보이게 하는 것이 가능하다. 컨테이너는 어플레케이션의 실행에 필요한 모듈을 컨테너로 모을수 있기 때문에 여러개의 컨테이너를 조합하여 하나의 어플리케이션을 구축하는 마이크로형 어플리케이션과 친화성 또한 높다.

### 1-2 Why we have to use Container?
1. 컨테이너를 사용하면 개발자와 IT운영팀이 훨씬 작은 단위로 업무를 수행할 수 있으므로 그에 따른 이점이 많다고 한다.
2. 위에서 말한것 처럼 가상머신은 하드웨어 스택을 가상화하고, 컨테이너는 이와 달리 운영체제 수준에서 가상화를 실시하여 다수 컨테이너를 OS 커널에서 직접 구동한다. 컨테이너는 이러한점에서 훨씬 가볍고 운영체제 커널을 공유하며, 시작이 훨씬 빠르고 운영체제 전체 부팅보다 메모리를 훨씩 적게 차지한다.

---

## 2. What is Docekr?

그렇다면 Docker 는 무엇일까? 음, 사실 컨테이너를 이해 하였다면 도커를 이해한 것과 다름없다. 
docker는 리눅스 컨테이너를 플랫폼화 시킨 것이다.

- 리눅스 컨테이너에 여러기능 추가
- 어플리케이션을 컨테이너로 좀더 쉽게 사용할 수 있게 만든 오픈소스 프로젝트
- 가상 머신과 달리 성능 손실이 거의 없는 차세대 클라우드 솔루션으로 주목

### Docer image  
- __도커이미지__
  - 여러 개의 층으로 된 바이너리 파일로 존재
  - 컨테이너 생성시 읽기 전용으로 사용된다.
  ![](img)
  
  - 저장소 이름 : 이미지의 역할을 나타낼 이름, 생랼 불가
  - 이미지 이름 : 이미지의 역할을 나타낼 이름, 생략 불가
  - 이미지 버젼 : 이미지 버전정보, 생략하면 latest로 인식
  
이미지를 생성하는것은 아래 실습과정에서 다루도록 하겠습니다. 
  
  
## 3. Why do you need it?
[[참조 : 왜 굳이 도커를 써야 하는가?]](https://www.44bits.io/ko/post/why-should-i-use-docker-container)
사실 나도 굉장히 궁금한점이다. 아직 애송이 주니어인 내 입장에선 도커 없이도 배포/운영에 있어 큰 불편함을 못느껴봤다. 위의 링크의 문서에선 이를 예시까지 들어가며 설명해주고 있다. 


