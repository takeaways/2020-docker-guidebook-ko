# docker-guidebook-ko

### 1. 도커 설치하기

- docker를 설치하는 방법은 두 가지가 있다.

  - Docker에서 제공하는 자동 설치 스크립트를 이용하는 방법
  - 리눅스 배포판의 패키징 시스템을 이용하여 직접 설치하는 방법

- CentOS7에서 설치하기

```bash
$ sudo yum install docker
```

### 2. 도커 사용해보기

- 도커의 명령은 <b>docker run, docker push</b>와 같이 <b>docker &lt;명령&gt; </b> 형식이며, 항상 "root"권한으로 실행해야합니다.
- 도커의 기본적인 사용 방법을 알아보기 위해서 Docker Hub에서 제공하는 이미지를 받아서 실행
  - 도커는 "Docker Hub"를 통해 이미지를 공유하는 생태계가 구축되어 있다.<br/><br/><br/>
- docker search : 명령으로 Docker Hub에서 이미지를 검색할 수 있습니다.
  ```bash
  $ docker search [ubuntu, centos]
  ```
- docker pull <이미지 이름> : <태그> 를 이용하여 이미지 다운 받기
  ```bash
  $ docker pull centos
  ```
- docker images 명령으로 이미지 목록 축력하기
  ```bash
  $ docker images
  ```
- docker run <옵션> <이미지 이름> <실행할 파일> 컨테이너 생성하기
  - 이미지를 컨테이너로 생성한 뒤 bash 쉘을 실행 시켜보기
  - -i(interactive), -t(Pseudo-tty) 옵션을 사용하면 실행된 bash 쉘에 입력 및 출력을 할 수 있습니다.
  - -name 옵션으로 켄테이너의 이름을 지정할 수 있습니다.
  ```bash
  $ docker run -i -t --name hello centos /bin/bash
  ```
- docker ps -a 컨테이너 목록 확인하기
  ```bash
  $ docker ps -a
  ```
- docker start <컨테이너 이름> 컨테이너 시작하기
  ```bash
  $ docker start hello
  ```
- docker start restart <컨테이너 이름> 컨테이너 재시작하기
  ```bash
  $ docker restart hello
  ```
- docker start attach <컨테이너 이름> 컨테이너에 접속하기
  - ctrl + d : 컨테이너 정지 및 빠져나오기
  - ctrl + p or ctrl + q : 컨테이너 동작상태에서 빠져나오기
  ```bash
  $ docker attach hello
  ```
- docker exec <컨테이너 이름> <명령> <매개 변수> 외부에서 컨테이너 안의 명령 실행하기
  ```bash
  $ docker exec hello echo "Hello World"
  ```
- docker stop <컨테이너 이름> 컨테이너 정지 시키기
  ```bash
    $ docker stop hello
  ```
- docker stop <컨테이너 이름> 컨테이너 정지 시키기
  ```bash
    $ docker stop hello
  ```
- docker rm <컨테이너 이름> 컨테이너 삭제하기
  ```bash
    $ docker rm hello
  ```
- docker rmi <이미지 이름>:<태그> 이미지 삭제하기
  ```bash
    $ docker rm hello
  ```

### 3. 도커 이미지 생성하기

- 3.1) Bash 익히기
  | 문법 | 설명 |
  | ------------- |:-------------:|
  | > | 출력 리다이렉션, 명령 실행의 표춘 출력(stdout)을 파일로 저장합니다. 유닉스계열 운영체제는 장치도 파일로 처리하기 때문에 명령 실행 결과를 특정 장치로 보낼 수도 있습니다. <br/> $ echo "hello" > ./hello.txt <br/> $ echo "hello" > /dev/null|
  | < | 입력 리다이렉션, 파일의 내용을 읽어 명령의 표준 입력(stdin)으로 사용합니다. |  
   | zebra stripes | are neat |
