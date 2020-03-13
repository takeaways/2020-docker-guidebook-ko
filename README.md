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
- docker images 명령으로 이미지 목록 출력하기
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
  | :-------------: |:-------------|
  | > | 출력 리다이렉션, 명령 실행의 표춘 출력(stdout)을 파일로 저장합니다. 유닉스계열 운영체제는 장치도 파일로 처리하기 때문에 명령 실행 결과를 특정 장치로 보낼 수도 있습니다. <br/> $ echo "hello" > ./hello.txt <br/> $ echo "hello" > /dev/null|
  | < | 입력 리다이렉션, 파일의 내용을 읽어 명령의 표준 입력(stdin)으로 사용합니다. <br/> $ cat < ./hello.txt|  
   | >> | 명령 실행의 표준 출력(stdout)을 파일에 추가합니다. >는 이미 있는 파일에 내용을 덮어 쓰지만 >>는 파일 뒷 부분에 내용을 추가 합니다. <br/> $ echo "world" >> ./hello.txt |
  |2>| 명령 실행의 표준 에러(stderr)를 파일로 저장합니다.|
  |2>>| 명령 실행의 표준 에러(stderr)를 파일에 추가합니다.|
  |&>| 표준 출려과 표준 에러를 모두 파일로 저장합니다.|
  |1>$2| 표준 출력을 표준 에러로 보냅니다. echo 명령으로 문자열을 표준 출력으로 출력했지만 표준 에러로 보냈기 때문에 변수에는 문자열이 들어가지 않습니다. <br/> $ hello=$(echo "Hello WOrld" 1>&2)<br/> $ echo \$hello|
  |2>&1|표준 에러를 표준 출력으로 보냅니다. abcd라는 명령은 없음으로 에러가 발생하지만 에러를 표준출력으로 보낸 뒤 다시 /dev/null로 보냈기 떄문에 아무것도 출력 되지 않습니다. <br/> \$ abcd > /dev/null 2>&1|
  | \| | 파이프 명령 실행의 표준 출력을 다른 명령의 표준 입력으로 보냅니다. 즉 첫 번쨰 명령의 출력 같을 두 번 쨰 명령에서 처리합니다. <br/> $ ls -al \| grep.txt |
  |\$|bash의 변수 입니다. 값을 저장할 떄는 $를 붙이지 않고, 변수를 가져다 쓸 떄만 \$를 붙입니다. <br/> $ hello= "Hello World" <br/> $ echo $hello <br/> Hello World|
  |$()|명령 실행결과를 변수화합니다. 명령 실행 결과를 변수에 저장하거나 다른 명령의 매개 변수로 넘겨줄 떄 사용합니다. 또는 문자열 안에 명령의 실행 결과를 넣을 때 사용합니다. <br/> $ docker rm $(docker ps -aq) <br/> $ echo $(date)|
  |``|$()꽈 마찬가지로 명령 실행 결과를 변수화합니다. <br/> $ docker rm `docker ps -aq` <br/> $ echo \`date\`|
  |&&|한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 멸령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다. <br/> $ make && make install|
  |;|한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 명령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다. <br/> $ false; echo "Hello"|
  |''|문자열입니다. ' '안에 들어있는 변수는 처리되지 않고 변수명 그대로 사용됩니다. 또한 "와 $()도 처리되지 않고 그대로 사용됩니다. <br/> $ echo '$USER'<br/>\$USER<br/>그대로 출력됩니다.|
  |" "|문자열입니다. 명령에 문자열 매개 변수를 입력하거나 변수에 저장할 때 주로 사용합니다. '' 와는 달리 " "안에 변수가 들어 있으면 변수의 내용으로 바뀝니다. 또한 "와 $()도 실행 결과 값이 사용됩니다. <br/> $ echo "Hello world" <br/> $ echo "$USER"|
  |"''"|""안에 ''가 들어갈 수 있습니다. 명령 안에서 다시 명령을 실행하고 매개 변수를 지정할 떄 사용합니다. $ bash -c "/bin/echo Hello 'World"|
  |\"<br/>\$hello|''안에서 "를 사용할 때 \"처럼 앞에 \를 붙여줍니다. <br/> $ bash -c "/bin/echo '{\"user\":\"$USER\"}'"|
  |${}|변수 치환입니다. " "문자열 안에서 변수를 출력할 때 주로 사용합니다. ${} 대신 $만 사용해도 됩니다.<br/>$ strt="World"<br/>$ echo "Hello ${str}"<br/><br/>스크립트에서 변수의 기본 값을 설정할 떄도 사용합니다. 다음은 HELLO변수가 있으면 그대로 사용하고 변수가 없으면 기본 값으로 설정한 abcd를 대입합니다.<br/>$ HELLO=<br/>$ HELLO=${HELLO-"abcd"}<br/>$ echo $HELLO<br/><br/>값이 NULL인 HELLO 변수가 이미 있기 때문에 기본 값을대입하지 않습니다. 다음은 변수의 값이 있으면 그대로 사용하고, 값이 NULL이면 기본 값으로설정한 abcd를 대입합ㄴ디ㅏ.<br/>$ WORLD=<br/> $ WORLD=${WORLD:-"abcd"}<br/>$ echo \$WORLD|
