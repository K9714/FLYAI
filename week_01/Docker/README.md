# 1주차 목록
* [Day1 교육내용](#day-1-교육내용) 
* [Day2 교육내용](#day-2-교육내용)

---

## 주요 내용
1. `Github` 또는 `Notion` 을 사용해서 교육내용을 정리하는 습관을 기르자. 되도록 `Github` 권장. ~~그래서 노력중~~
2. 우리가 지금까지 해온 ML(Machine Learning)은 `Research ML` 이다. 실제 서비스에서는 `Product ML` 을 해야 한다.
3. 개발환경이 모두 다르다. 다른 개발환경을 하나로 통일하기 위해 `Docker` 를 사용한다.
4. `Docker` 를 사용하는 서비스의 개수가 늘어날 경우, 관리를 위해 `쿠버네티스` 를 사용한다.
5. `VirtualBox` 를 사용하여 `Ubuntu 가상환경`을 설치하고, Ubuntu 환경에 `Docker` 설치
6. `CLI(Command-Line Interface)` 환경과 친해지기

## 간단 평가
오늘 수업은 DevOps / MLOps 의 기본 개념과 차이점에 대한 설명과 Docker 사용 이유에 대해 수업을 들었다.  
우분투 가상환경 및 Docker 설치에 수업시간의 대부분을 사용한 것 같다.

---

## Ubuntu 설치
### 우분투 이미지(ISO) 파일 받기
우분투 다운로드 URL : https://releases.ubuntu.com/  

다운로드 페이지 접속 후 LTS(Long-Term Support, 장기 지원) 버전으로 받기  
통상적으로, 우분투는 xx.04 버전이 LTS 버전이다.


## VirtualBox 설치
VirtualBox 다운로드 URL : https://www.virtualbox.org/wiki/Downloads  

가상머신에 리눅스를 설치하기 위해 사용하는 가상머신 프로그램이다.  
현재 작업하는 PC 내부에 또 다른 OS를 설치하여 사용할 수 있다.  

위에서 받은 ISO 파일을 이용하여 가상머신 환경 생성 및 설치를 진행한다.

> 사실 윈도우는 WSL(Windows Subsystem for Linux) 기능을 지원하여 쉽게 사용할 수 있지만, 교육과정에서는 편의를 위해 가상머신을 사용한 것 같다.

## Docker 설치
Ubuntu 패키지 매니저 `apt-get`  
관리자(`Su`peruser) 권한으로 해라(`do`)! : `sudo`  

Ubuntu 패키지를 최신 버전으로 유지하기 위해 아래의 명령어를 필수로 사용  
``` shell
$ sudo apt-get update
$ sudo apt-get upgrade
```
여기서 달러(`$`) 표시는 `사용자` 를 의미한다고 배웠다.  

제공해주신 Notion 페이지가 존재했지만, GPG 설정 중 오류가 발생하여...  
나는 Ubuntu 환경에 Docker 를 아래와 같은 방법으로 설치했다.

``` shell
# 최신버전 확인 및 필수 패키지 설치
$ sudo apt-get update
$ sudo apt-get install \
   ca-certificates \
   curl \
   gnupg \
   lsb-release
   
# GPG 관련 설명은 생략되었지만, GPG 키 설정을 위한 과정
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# ARM 아키텍처 CPU와 x86 아키텍처 CPU의 과정이 틀리다고 하셨다.
# 실습 랩톱은 x86(Intel) 기반이므로, 아래와 같이 진행
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
설치를 마치고, 정상 실행 확인 및 사용자에게 docker 명령어의 권한 추가 후 리부팅
``` shell
# Docker 테스트
$ sudo docker run hello-world

# docker 명령 권한 부여
$ sudo usermod -a -G docker $USER
$ sudo service docker restart
# 재부팅??(나는 재부팅을 하지 않아도 작동했었다.)
$ reboot
# 이후로는 'docker' 명령에 'sudo' 명령을 줄 필요가 없음
```

나는 기존 리눅스를 사용하는 환경과 비슷하게 구성하기 위해 `bash -> zsh 변경` 및 `oh-my-zsh 설치`까지 진행했다.  
추가로, `VirtualBox 포트포워딩`을 통해 `Windows Termial` ssh 연결을 통해 사용할 수 있도록 환경 구성을 마쳤다.  

수업 중 `VirtualBox - Windows 간 클립보드가 공유되지 않는 문제`가 발생했는데, 위 환경처럼 구성하면 Windows 의 클립보드를 바로 사용할 수 있다는 장점도 존재했다. (개인적으로 CLI 환경이라면 선호하는 방식)

![환경설정 사진](./pic01.png)

---

# Day 2 교육내용

## 주요내용
1. 기본적인 `Docker 사용법`
2. 로컬에서 이미지를 관리해주는 `registry` 이미지
3. 클라우드로 이미지를 관리하는 `docker hub` 사용하기
4. 더욱 방대한 이미지를 관리할 수 있는 `쿠버네티스` 설치하기

## 간단평가
오늘은 쿠버네티스 설정에 시간을 대부분 사용했다.  
Docker Hub 에서 약 1% 사용자가 전체 패킷량의 30% 이상을 사용한다는 이유로 IP 당 6시간 제한 할당량을 정해두었다.  
실습실에서 모두 같은 네트워크를 사용하다보니 금방 제한 할당량에 도달하여 Docker Image 를 다운로드 하지 못하는 상황이 발생했다. ~~나는 사실 VPN 으로 문제를 해결했다~~  
Docker 를 사용해본 경험이 없었기 때문에 아직 정확한 사용 의미는 깨닫지 못했지만, 점차 익숙해지려 노력해볼 예정이다.

---

## Docker

설치가 완료되었는지 확인하기 위해서 아래의 명령 실행
``` bash
$ docker run hello-world
```

누군가 만들어놓은 `hello-world` 라는 컨테이너를 받고 실행 후 종료 (지속 실행되지 않는 컨테이너)  

어떤 특정한 프로그램이 우분투 래거시(과거) 버전을 사용하는 경우 `컨테이너에서 버전 변경 가능`  

```bash
# Docker 이미지를 가져오는 명령 'pull'
# 우분투 18.04 Docker 이미지를 가져옴
$ docker pull ubuntu:18.04

# 다운로드 받은 이미지 확인
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        18.04     ad080923604a   3 weeks ago    63.1MB
hello-world   latest    feb5d9fea6a5   9 months ago   13.3kB

# 실행중인 docker 프로세스 확인 (-a 옵션으로 Exited 기록까지 확인)
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
9a3a340adbf1   hello-world   "/hello"   10 minutes ago   Exited (0) 10 minutes ago             upbeat_ritchie
d3155679476c   hello-world   "/hello"   10 hours ago     Exited (0) 10 hours ago               epic_matsumoto
e6b372e7e0c6   hello-world   "/hello"   16 hours ago     Exited (0) 16 hours ago               confident_williams
c91846635398   hello-world   "/hello"   16 hours ago     Exited (0) 16 hours ago               loving_brown
b062aac74813   hello-world   "/hello"   16 hours ago     Exited (0) 16 hours ago               zen_euler

# docker 실행, -it(interective mode, 명령 입력 가능 모드), --name 별칭
# 최종적으로 아까 다운받은 ubuntu 18.04 이미지 실행
# 최초 실행 시 어느 프로세스를 실행시킬 것인지 경로 지정 -> bash
# /bin 경로에는 자주 사용하는 프로세스가 존재하는 경로
$ docker run -it --name demo1 ubuntu:18.04 /bin/bash
root@a2442a22985a:/#
# 달러($) 표시에서 샵(#) 표시로 바뀜으로서
# 새로운 Docker 환경의 관리자 권한으로 들어왔다는 것을 확인

# 새로운 Docker 환경에서 update 및 upgrade 진행
# superuser 권한이므로 'sudo' 명령이 필요 없음
> apt-get update
> apt-get upgrade

# docker 환경에서 빠져나가기
> exit
~$
# 다시 달러($) 표시로 바뀌며 원래의 Ubuntu 환경으로 돌아온것을 확인

# 어떤 작업을 했는 지 확인하기 위해 log 확인
# 로그 검사할 대상 컨테이너가 인수로 들어감
$ docker logs demo1
root@a2442a22985a:/# apt-get update
Get:1 http://archive.ubuntu.com/ubuntu bionic InRelease [242 kB]
...
Reading package lists... Done
root@a2442a22985a:/# apt-get upgrade
Reading package lists... Done
...
Processing triggers for libc-bin (2.27-3ubuntu1.6) ...
root@a2442a22985a:/# exit
exit
# 전에 입력한 명령어들과 출력들을 확인할 수 있음
```

동시에 여러 컨테이너를 실행시키기 위해서는 아래와 같이 실행

```bash
# ubuntu 18.04 이미지의 별칭을 demo2 로도 생성
# -d : 메모리에 적재(demon)
$ docker run -it -d --name demo2 ubuntu:18.04
e19baf7c370b83a0c3430d3ef343d20ca336a5722ee3fdbafdf44645456b050d

# docker 프로세스 확인
$ docker ps
CONTAINER ID   IMAGE          COMMAND   CREATED         STATUS         PORTS     NAMES
e19baf7c370b   ubuntu:18.04   "bash"    3 seconds ago   Up 2 seconds             demo2

# 실행중인 docker 프로세스에 접속
$ docker exec -it demo2 /bin/bash
root@e19baf7c370b:/#
> exit
# -d 모드로 실행시킨 경우 exit 로 빠져나와도 프로세스가 종료되지 않음

# busybox 이미지 -d 모드로 실행
# 루프를 돌며 날짜를 1초마다 찍음
$ docker run -d --name demo3 busybox sh -c "while true; do $(echo date); sleep 1; done"
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
19d511225f94: Pull complete
Digest: sha256:3614ca5eacf0a3a1bcc361c939202a974b4902b9334ff36eb29ffe9011aaad83
Status: Downloaded newer image for busybox:latest
662cb30d7c7f2a893fbdb497c90c18bbfb72b23f0eacb01683d468a743e47e9a

# -d 모드의 실행 결과를 확인하기 위해 로그를 확인
$ docker logs demo3
Tue Jun 28 01:17:13 UTC 2022
Tue Jun 28 01:17:14 UTC 2022
Tue Jun 28 01:17:15 UTC 2022
Tue Jun 28 01:17:16 UTC 2022
Tue Jun 28 01:17:17 UTC 2022
Tue Jun 28 01:17:18 UTC 2022
Tue Jun 28 01:17:19 UTC 2022
Tue Jun 28 01:17:20 UTC 2022

# 실시간 로그를 확인하기 위해 -f 옵션 사용
# 1초마다 실행되는 것을 확인할 수 있음
$ docker logs demo3 -f
Tue Jun 28 01:17:13 UTC 2022
Tue Jun 28 01:17:14 UTC 2022
Tue Jun 28 01:17:15 UTC 2022
...
Tue Jun 28 01:20:09 UTC 2022
# Break 명령(Control + C, ^C)

# 실행중인 컨테이너를 멈추고 싶은 경우
$ docker stop demo3
demo3
$ docker stop demo2
demo2

# 모든 컨테이너가 종료된것을 확인
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 실행중인 컨테이너가 없어도 메모리에 존재
# rm(remove) 명령을 사용하여 컨테이너 삭제
$ docker rm demo3
demo3
$ docker rm demo2
demo2
$ docker rm demo1
demo1

# 삭제 후 docker 프로세스 확인
# 삭제한 demo1~3 에 대한 정보가 모두 사라진것을 확인
CONTAINER ID   IMAGE         COMMAND    CREATED             STATUS                         PORTS     NAMES
9a3a340adbf1   hello-world   "/hello"   About an hour ago   Exited (0) About an hour ago             upbeat_ritchie
d3155679476c   hello-world   "/hello"   11 hours ago        Exited (0) 11 hours ago                  epic_matsumoto
e6b372e7e0c6   hello-world   "/hello"   17 hours ago        Exited (0) 17 hours ago                  confident_williams
c91846635398   hello-world   "/hello"   17 hours ago        Exited (0) 17 hours ago                  loving_brown
b062aac74813   hello-world   "/hello"   17 hours ago        Exited (0) 17 hours ago                  zen_euler

# 다운받은 이미지를 확인하고, 필요 없는 이미지는 삭제
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
busybox       latest    62aedd01bd85   2 weeks ago    1.24MB
ubuntu        18.04     ad080923604a   3 weeks ago    63.1MB
hello-world   latest    feb5d9fea6a5   9 months ago   13.3kB

# 이미지 삭제는 rmi(remove image) 사용
$ docker rmi ubuntu:18.04
Untagged: ubuntu:18.04
Untagged: ubuntu@sha256:478caf1bec1afd54a58435ec681c8755883b7eb843a8630091890130b15a79af
Deleted: sha256:ad080923604aa54962e903125cd9a860605c111bc45afc7d491cd8c77dccc13b
Deleted: sha256:95129a5fe07e89c1898dc40a027b291d5fe33a67b35a88f0f0eaf51ea691f0b5

$ docker rmi busybox
Untagged: busybox:latest
Untagged: busybox@sha256:3614ca5eacf0a3a1bcc361c939202a974b4902b9334ff36eb29ffe9011aaad83
Deleted: sha256:62aedd01bd8520c43d06b09f7a0f67ba9720bdc04631a8242c65ea995f3ecac8
Deleted: sha256:7ad00cd55506625f2afad262de6002c8cef20d214b353e51d1025e40e8646e18

# 이미지 삭제 확인
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   9 months ago   13.3kB
```

## Linux 명령어

* `cd(Change Directory)` : 경로 변경  
* `pwd(Print Working Directory)` : 현재 위치한 경로 출력  
* `mkdir(Make Directory)` : 새로운 폴더 생성  
* `ls(List Segments)` : 현재 경로의 파일/경로 목록 출력  

```bash
# 홈으로 이동
$ cd $HOME
# 현재 경로 출력
$ pwd
/home/ubuntu
# 새로운 폴더 생성
$ mkdir docker-practice
# 새로 만든 폴더 경로로 이동
$ cd docker-practice
# 빈 내용의 파일 'Dockerfile' 생성
$ touch Dockerfile
# 파일 목록을 확인
$ ls:q:
Dockerfile

# 파일 내용을 편집하기 위해서 'vi' 편집기 사용
$ vi Dockerfile
```

## Vi 편집기
`ESC` : 명령 모드로 전환  
`i(insert)` : 편집 모드  
`:w` : 저장  
`:q` : 나가기  
`:q!` : 강제 종료  

Dockerfile 은 아래와 같이 작성

```file
# Dockerfile 
FROM ubuntu:18.04

RUN apt-get update

CMD ["echo", "hello docker!"]
```

## Docker Build

'Dockerfile' 을 이용하여 Docker 이미지를 만드는 것을 빌드 또는 '만다' 라고 표현함

```bash
# 도커 이미지를 생성하는데, 이름을 'my-image' 로 생성
# 버전 태그를 1.0.0 으로 설정, Dockerfile 이 존재하는 경로를 입력
$ docker build -t my-image:v1.0.0 .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM ubuntu:18.04
18.04: Pulling from library/ubuntu
...
Successfully built a636966ff83e
Successfully tagged my-image:v1.0.0
# 자동으로 apt-get update 명령이 실행됨

# 생성된 docker 이미지 확인
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
my-image     v1.0.0    a636966ff83e   16 seconds ago   104MB
ubuntu       18.04     ad080923604a   3 weeks ago      63.1MB

# docker 이미지 실행결과 중, grep 명령어를 통해 'my' 단어가 들어간 목록만 출력
$ docker images | grep my
my-image     v1.0.0    a636966ff83e   About a minute ago   104MB

# 새로 생성한 'my-image' 이미지를 실행
$ docker run my-image:v1.0.0
Hello Docker!!
# 정상적으로 echo 명령이 실행되는 것을 확인

# -d 모드로 실행
# -p 옵션으로 포트넘버 지정 (내부:외부)
# registry 라는 이름의 이미지를 registry 이름으로 지정
$ docker run -d -p 5000:5000 --name registry registry

# docker pull rate limit 발생 시 로그인 처리
$ docker login
UserName: 
Password: 
...
Login Succeeded

# 명령 재실행
$ docker run -d -p 5000:5000 --name registry registry
Unable to find image 'registry:latest' locally
latest: Pulling from library/registry
2408cc74d12b: Pull complete
ea60b727a1ce: Pull complete
c87369050336: Pull complete
e69d20d3dd20: Pull complete
fc30d7061437: Pull complete
Digest: sha256:bedef0f1d248508fe0a16d2cacea1d2e68e899b2220e2258f1b604e1f327d475
Status: Downloaded newer image for registry:latest
0535dfd1ec34ed596ef7e0297e6256e97e67136bcf831a971abc54ccb3679fc9

# Registry docker 는 로컬에서 docker 이미지를 관리해주는 역할
# 실행 여부 확인
$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                                       NAMES
0535dfd1ec34   registry   "/entrypoint.sh /etc…"   41 seconds ago   Up 40 seconds   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   registry

# registry 컨테이너에 이미지를 전송
$ docker tag my-image:v1.0.0 localhost:5000/my-image:v1.0.0

# docker 이미지 확인
$ docker images
REPOSITORY                TAG       IMAGE ID       CREATED          SIZE
my-image                  v1.0.0    a636966ff83e   31 minutes ago   104MB
localhost:5000/my-image   v1.0.0    a636966ff83e   31 minutes ago   104MB
ubuntu                    18.04     ad080923604a   3 weeks ago      63.1MB
registry                  latest    773dbf02e42e   4 weeks ago      24.1MB

# docker 이미지 registry 에 push
$ docker push localhost:5000/my-image:v1.0.0
The push refers to repository [localhost:5000/my-image]
9beb6b9d6d83: Pushed
95129a5fe07e: Pushed
v1.0.0: digest: sha256:b60db61f2ba1b4c4ee817204bb9e4780dcd440b984a97a86893d8fe4246bf976 size: 741

# registry 에 데이터 상태 질의
$ curl -X GET http://localhost:5000/v2/_catalog
{"repositories":["my-image"]}
# 특정 이미지의 Tag 정보를 자세하게 확인하려고 하는 경우
$ curl -X GET http://localhost:5000/v2/my-image/tags/list
{"name":"my-image","tags":["v1.0.0"]}
```

## Docker Hub
Github 와 비슷하게 Docker Hub 사이트에 이미지를 업로드 하는 방법

```bash
# Docker Hub 에 이미지 업로드 (내 계정으로)
$ docker tag my-image:v1.0.0 k9714/my-image:v1.0.0

# Docker Hub 에 push (내 계정으로)
$ docker push k9714/my-image:v1.0.0
The push refers to repository [docker.io/k9714/my-image]
9beb6b9d6d83: Pushed
95129a5fe07e: Mounted from library/ubuntu
v1.0.0: digest: sha256:b60db61f2ba1b4c4ee817204bb9e4780dcd440b984a97a86893d8fe4246bf976 size: 741
```

Docker Hub Repository 에 업로드가 완료된 모습

![dockerhub-repos](pic02.png)

## 쿠버네티스 설치

```bash
# Minikube 설치를 위한 파일 받기
$ curl -LO https://storage.googleapis.com/minikube/releases/v1.22.0/minikube-linux-amd64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 66.5M  100 66.5M    0     0  4961k      0  0:00:13  0:00:13 --:--:-- 3959k

# 다운받은 minikube 설치
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube

# minikube 설치 확인
$ minikube
minikube는 개발 워크플로우에 최적화된 로컬 쿠버네티스를 제공하고                                     관리합니다.
...
Use "minikube <command> --help" for more information about a given command.

# minikube 버전 확인
$ minikube version
minikube version: v1.22.0
commit: a03fbcf166e6f74ef224d4a63be4277d017bb62e

# 명령어로 컨트롤하기 위해 CLI 도구가 필요
# kubectl 도구 다운로드
$ curl -LO https://dl.k8s.io/release/v1.22.1/bin/linux/amd64/kubectl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   154  100   154    0     0    594      0 --:--:-- --:--:-- --:--:--   592
100   220  100   220    0     0    367      0 --:--:-- --:--:-- --:--:--   367

# 유저 모드로 'sudo' 키워드 없이 명령을 사용하기 위해서
# /usr/local/bin/kubectl 경로 권한 0755 로 변경
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# kubectl 설치 확인
$ kubectl
kubectl controls the Kubernetes cluster manager.
...
Use "kubectl options" for a list of global command-line options (applies to all commands).

# Minikube 시작
# !! 중요, VirtualBox CPU 개수가 2코어 이상이여야 힘!
$ minikube start --driver=docker
😄  Ubuntu 20.04 (vbox/amd64) 의 minikube v1.22.0
🎉  minikube 1.26.0 이 사용가능합니다! 다음 경로에서 다운받으세요: https://github.com/kubernetes/minikube/releases/tag/v1.26.0
...
🌟  애드온 활성화 : storage-provisioner, default-storageclass
🏄  끝났습니다! kubectl이 "minikube" 클러스터와 "default" 네임스페이스를 기본적으로 사용하도록 구성되었습니다.
# CPU 코어 수정 후 설치가 정상적으로 진행됨

# minikube 실행중인지 확인
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
# minikube Running 확인

# docker 에서도 확인
$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS         PORTS                                                                                                                                  NAMES
4aff53531b89   gcr.io/k8s-minikube/kicbase:v0.0.25   "/usr/local/bin/entr…"   2 minutes ago   Up 2 minutes   127.0.0.1:49157->22/tcp, 127.0.0.1:49156->2376/tcp, 127.0.0.1:49155->5000/tcp, 127.0.0.1:49154->8443/tcp, 127.0.0.1:49153->32443/tcp   minikube
# minikube 컨테이너가 실행중인 것을 확인 가능


# 쿠버네티스의 실행중인 시스템 확인
$ kubectl get pod -n kube-system
NAME                               READY   STATUS    RESTARTS   AGE
coredns-558bd4d5db-np97g           1/1     Running   0          15m
etcd-minikube                      1/1     Running   0          15m
kube-apiserver-minikube            1/1     Running   0          15m
kube-controller-manager-minikube   1/1     Running   0          15m
kube-proxy-wmdzq                   1/1     Running   0          15m
kube-scheduler-minikube            1/1     Running   0          15m
storage-provisioner                1/1     Running   0          15m

# minikube 서비스 끄기
$ minikube delete
🔥  docker 의 "minikube" 를 삭제하는 중 ...
🔥  Deleting container "minikube" ...
🔥  /home/ubuntu/.minikube/machines/minikube 제거 중 ...
💀  "minikube" 클러스터 관련 정보가 모두 삭제되었습니다
```

## pod.yaml 생성
확장자는 `yaml`, `pod.yaml` 파일을 생성
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: counter
spec:
    containers:
        - name: count
          image: busybox
          args: [/bin/sh, -c, 'i = 0; while true; do echo "$i: &(date) "; i=$((i + 1)); sleep 1; done']
```

## pod.yaml 파일 적용
```bash
# 쿠버네티스 실행
$ minikube start --driver=docker
😄  Ubuntu 20.04 (vbox/amd64) 의 minikube v1.22.0
✨  유저 환경 설정 정보에 기반하여 docker 드라이버를 사용하는 중
...
🌟  애드온 활성화 : default-storageclass, storage-provisioner
🏄  끝났습니다! kubectl이 "minikube" 클러스터와 "default" 네임스페이스를 기본적으로 사용하도록 구성되었습니다.

# pod.yaml 파일 적용
$ kubectl apply -f pod.yaml
pod/counter created

# pod 상태 확인
$ kubectl get pod
NAME      READY   STATUS    RESTARTS   AGE
counter   1/1     Running   0          37s
# 만약, Running 이 안된다면 대부분 docker hub rate limit 오류
# docker login 또는 IP 변경으로 해결 가능

# System pod 까지 모두 확인하기
$ kubectl get pod -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
default       counter                            1/1     Running   0          25m
kube-system   coredns-558bd4d5db-p8nfx           1/1     Running   0          25m
kube-system   etcd-minikube                      1/1     Running   0          25m
kube-system   kube-apiserver-minikube            1/1     Running   0          25m
kube-system   kube-controller-manager-minikube   1/1     Running   0          25m
kube-system   kube-proxy-kw59m                   1/1     Running   0          25m
kube-system   kube-scheduler-minikube            1/1     Running   0          25m
kube-system   storage-provisioner                1/1     Running   0          25m

# 지속적으로 확인하기 위해 모니터링 옵션 -w 사용
$ kubectl get pod -w
NAME      READY   STATUS    RESTARTS   AGE
counter   1/1     Running   0          26m
# 상태 변화를 주기적으로 확인할 수 있음

# Pod 내부에 접속하기
$ - # kubectl exec -it counter /bin/bash

# Pod 삭제하기
$ kubectl delete pod counter
pod "counter" deleted

# 삭제 확인하기
$ kubectl get pod
No resources found in default namespace.
```

## 번외 - Google Colab.
`Jupyter Notebook` 과 같이 사용할 수 있는 구글 제공 서비스.  
구글의 방대한 컴퓨팅 자원을 일부 사용할 수 있도록 해주는 서비스다.  
`Jupyter Notebook` 과 거의 사용 방법이 유사해서 만약 경험자라면 손쉽게 적응할 수 있다.  

