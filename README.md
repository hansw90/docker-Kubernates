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
1. What are Containers?
2. What is Docker?
3. Why do you need it?
4. What can it do?

5. Run Docker Containers
6. Create a Docker Image
7. Networks in Docker
8. Docker Compose

9. Docker Concepts in Depth

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

위의 그림처럼 가상화 기술은 Host OS, Guest OS 두개의 커널을 지니며, Container는 한개의 커널을 지난다. 이점은 애플리 케이션의 오버헤드와 큰 연광성이 있다.

- [VM과 Container의 비교](https://medium.com/@lhs6395/container%EC%99%80-vm%EC%9D%98-%EB%B9%84%EA%B5%90-84f6a8b7cd4c)





