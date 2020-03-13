# docker-guidebook-ko

# 1. 도커 설치하기

- docker를 설치하는 방법은 두 가지가 있다.

  - Docker에서 제공하는 자동 설치 스크립트를 이용하는 방법
  - 리눅스 배포판의 패키징 시스템을 이용하여 직접 설치하는 방법

- CentOS7에서 설치하기

```bash
$ sudo yum install docker
```

# 2. 도커 사용해보기

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

# 3. 도커 이미지 생성하기

- ### 3.1) Bash 익히기

  <table>
  <thead>
  <th>문법</th><th>설명</th>
  </thead>
  <tbody>
  <tr>
  <td>></td><td>출력 리다이렉션, 명령 실행의 표춘 출력(stdout)을 파일로 저장합니다. 유닉스계열 운영체제는 장치도 파일로 처리하기 때문에 명령 실행 결과를 특정 장치로 보낼 수도있습니다<pre><code>$ echo "hello" > ./hello.txt
  $ echo "hello" > /dev/null</code></pre>
  </td>
  </tr>
  <tr>
  <td><</td><td>입력 리다이렉션, 파일의 내용을 읽어 명령의 표준 입력(stdin)으로 사용합니다.<pre><code>$ cat < ./hello.txt</code></pre></td>
  </tr>
  <tr>
  <td>>></td><td>명령 실행의 표준 출력(stdout)을 파일에 추가합니다. >는 이미 있는 파일에 내용을 덮어 쓰지만 >>는 파일 뒷 부분에 내용을 추가 합니다.<pre><code>$ echo "world" >> ./hello.txt</code></pre></td>
  </tr>
  <tr>
  <td>2></td><td>명령 실행의 표준 에러(stderr)를 파일로 저장합니다.</td>
  </tr>
  <tr>
  <td>2>></td><td>명령 실행의 표준 에러(stderr)를 파일에 추가합니다.</td>
  </tr>
  <tr>
  <td>&></td><td>표준 출려과 표준 에러를 모두 파일로 저장합니다.</td>
  </tr>
  <tr>
  <td>1>$2</td><td>표준 출력을 표준 에러로 보냅니다. echo 명령으로 문자열을 표준 출력으로 출력했지만 표준 에러로 보냈기 때문에 변수에는 문자열이 들어가지 않습니다.<pre><code>$ hello=$(echo "Hello WOrld" 1>&2)
  $ echo \$hello</code></pre></td>
  </tr>
  <tr>  
  <td>2>&1</td><td>표준 에러를 표준 출력으로 보냅니다. abcd라는 명령은 없음으로 에러가 발생하지만 에러를 표준출력으로 보낸 뒤 다시 /dev/null로 보냈기 떄문에 아무것도 출력 되지 않습니다.<pre><code>$ abcd > /dev/null 2>&1</code></pre></td>
  </tr>
  <tr>
  <td>|</td><td>파이프 명령 실행의 표준 출력을 다른 명령의 표준 입력으로 보냅니다. 즉 첫 번쨰 명령의 출력 같을 두 번 쨰 명령에서 처리합니다.<pre><code>$ ls -al | grep.txt </code></pre></td>
  </tr>
  <tr>
  <td>\$</td><td>bash의 변수 입니다. 값을 저장할 떄는 $를 붙이지 않고, 변수를 가져다 쓸 떄만 \$를 붙입니다. <br/> <pre><code>$ hello= "Hello World"
  $ echo $hello <br/> Hello World</code></pre></td>
  </tr>
  <tr>
  <td>\$()</td><td>명령 실행결과를 변수화합니다. 명령 실행 결과를 변수에 저장하거나 다른 명령의 매개 변수로 넘겨줄 떄 사용합니다. 또는 문자열 안에 명령의 실행 결과를 넣을 때 사용합니다.<pre><code>$ docker rm $(docker ps -aq)
  $ echo $(date) </code></pre></td>
  </tr>
  <tr>
  <td>``</td><td>$()꽈 마찬가지로 명령 실행 결과를 변수화합니다.<pre><code>$ docker rm `docker ps -aq`
  $ echo \`date\` </code></pre></td>
  </tr>
  <tr>
  <td>&& </td><td>한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 멸령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다.<pre><code>$ make && make install </code></pre></td>
  </tr>
  <tr>
  <td>;</td><td>한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 명령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다.<pre><code>$ false; echo "Hello" </code></pre></td>
  </tr>
  <tr>
  <td>''</td><td>문자열입니다. ' '안에 들어있는 변수는 처리되지 않고 변수명 그대로 사용됩니다. 또한 "와 $()도 처리되지 않고 그대로 사용됩니다.<pre><code>$ echo '$USER'
  \$USER</code></pre>그대로 출력됩니다.</td>
  </tr>
  <tr>
  <td>""</td><td>문자열입니다. 명령에 문자열 매개 변수를 입력하거나 변수에 저장할 때 주로 사용합니다. '' 와는 달리 " "안에 변수가 들어 있으면 변수의 내용으로 바뀝니다. 또한 "와 $()도 실행 결과 값이 사용됩니다.<pre><code>$ echo "Hello world"
  $ echo "$USER" </code></pre></td>
  </tr>
  <tr>
  <td>"''"</td><td>" "안에 ''가 들어갈 수 있습니다. 명령 안에서 다시 명령을 실행하고 매개 변수를 지정할 떄 사용합니다.<pre><code>$ bash -c "/bin/echo Hello 'World" </code></pre></td>
  </tr>
  <tr>
  <td>\"<br/>\$hello</td><td>''안에서 "를 사용할 때 \"처럼 앞에 \를 붙여줍니다. <br/><pre><code>$ bash -c "/bin/echo '{\"user\":\"$USER\"}'" </code></pre></td>
  </tr>
  <tr>
  <td>${}<br/>\$hello</td><td>변수 치환입니다. " "문자열 안에서 변수를 출력할 때 주로 사용합니다. ${} 대신 $만 사용해도 됩니다.<pre><code>$ strt="World"
  $ echo "Hello ${str}"</code></pre>스크립트에서 변수의 기본 값을 설정할 떄도 사용합니다. 다음은 HELLO변수가 있으면 그대로 사용하고 변수가 없으면 기본 값으로 설정한 abcd를 대입합니다.<pre><code>$ HELLO=
  $ HELLO=${HELLO-"abcd"}
  $ echo $HELLO</code></pre>값이 NULL인 HELLO 변수가 이미 있기 때문에 기본 값을대입하지 않습니다. 다음은 변수의 값이 있으면 그대로 사용하고, 값이 NULL이면 기본 값으로설정한 abcd를 대입합니다.<br/><pre><code>$ WORLD=
  $ WORLD=${WORLD:-"abcd"}
  $ echo \$WORLD </code></pre></td>
  </tr>
  <tr>
  <td>\</td><td>한 줄로된 명령을 여러 줄로 표현할 때 사용합니다.<pre><code>$ dockerrun -d -name hello busybox:latest
  $ docker run \
  -d \
  --name hello \
  busybox:latest</code></pre></td>
  </tr>
  <tr>
  <td>{1..10}</td><td>연속된 숫자를 표현합니다. {시작숫자..끝 숫자} 형식</td>
  </tr>
  <tr>
  <td>{문자열1,문자열2}</td><td>{}안에 문자열을 여러 개 지정하여 명령 실행 횟수를 줄입니다. 다음은 hello.txt, world.txt 두 파일을 한 번에 hello-dir디렉 터리에 복사합니다.<pre><code>$ cp ./{hello.txt, world.txt} hello-dir/ </code></pre></td>
  </tr>
  <tr>
  <td>if</td><td>if 조건문입니다. 변수와 변수끼리 또는 문자열과 비교할 때 사용합니다.<pre><code>if [$a -eq $b]; then echo \$a
  fi
  </code></pre><b>숫자 비교</b><ul><li> -eq : 같다</li><li>-ne : 같지 않다.</li><li>-gt : 초과</li><li>-ge : 이상</li><li>-lt : 미만</li><li>-le : 이하</li></ul><br><b>문자열 비교</b><ul><li> =, == : 같다</li><li>!= : 같지 않다</li><li>-z : 문자열이 NULL 일 때</li><li>-n : 문자열이 NULL이 아닐 때</li></ul></td>
  </tr>
  <tr>
  <td>for</td><td>for 반복문입니다. 변수 안에 있는 값을 반복하거나 범위를 지정하여 반복할 수 있습니다.<pre><code>for i in ${ls}<br/>do
  &nbsp;&nbsp;echo $i
  done</code></pre></td>
  </tr>
  <tr>
  <td>while</td><td>while 반복문입니다.<pre><code>while :
  &nbsp;&nbsp;&nbsp;echo "Hello World";
  sleep 1;
  done</code></pre></td>
  </tr>
  <tr>
  <td><<<</td><td>문자열을 명령(프로세스)의 표준 입력으로 보냅니다.<pre><code> $ cat <<< "User name is $USER"
  &nbsp;&nbsp;&nbsp;User name is myname </code></pre>
  </td>
  </tr>
  <tr>
  <td> &lt;&lt;EOF EOF </td><td>여러 줄의 문자열을 명령의 표준 입력으로 보냅니다.<pre><code>$ cat > ./hello.txt &lt;&lt;EOF
  &nbsp;&nbsp;&nbsp;&nbsp;Hello World
  &nbsp;&nbsp;&nbsp;&nbsp;Host name is \$(hostname)
  &nbsp;&nbsp;&nbsp;&nbsp;User name is \$(USER)
  &nbsp;&nbsp;&nbsp;EOF</code></pre>cat은 파일이나 표준 입력의 내용을 출력하는 명령입니다. cat의 표준 출력을 ./hello.txt로 저장하고, <<EOF로 문자열을 cat의 표준 입력으로 보냅니다. 이렇게 하면 문자열 3줄이 ./hello.txt 파일에 저장됩니다.</pre>
  </td>
  </tr>
  <tr>
  <td>export</td><td>설정한 값을 환경 변수로 만듭니다. export <변수>=<값> 형식<pre><code>$ export HELLO=world</code></pre></td>
  </tr>
  <tr>
  <td>printf</td><td>지정한 형식대로 값을 출력합니다. 파이프와 연동하여 명령(프로세스)에 값을 입력하는 효과를 낼 수 있습니다.
  <pre><code>$ print 80\\neampleuser\\ny | example-config
  &nbsp;&nbsp;&nbsp;PORT: 80
  &nbsp;&nbsp;&nbsp;User: exampleuser
  &nbsp;&nbsp;&nbsp;Save Configuration (y/n): y</code></pre>
  예를 들어 example-config는 Port, User, Save Configuration을 사용자에게 입력을 받습니다. printf로 미리 값을 설정하여 파이프로 example-config에 넘겨주면 사용자가 입력하지 않아도 자동으로 값이 입력되니다. 줄바꿈(개행)은\\n 으로 표현합니다.</td>
  </tr>
  <tr>
  <td>sed</td><td>텍스트 파일에서 문자열을 변경합니다. hello.txt파일의 내용 중에서 hello하는 문자열을 찾아서 world 문자열로 바꾸려면 다음과 같이 실행합니다.<pre><code>$ sed -i "s/hello/world/g" hello.txt<code/></pre></td>
  </tr>
  </tbody>
  </table>

- ### 3.2) Dockerfile 작성하기

  1. dockerfile은 Docker 이미지 설정 파일 입니다. Dockerfile에 설정된 내용대로 이미지를 생성합니다.
     ```bash
     ~$ mkdir example
     ~$ cd example
     ```
  2. dockerfile 작성

     - FROM : 어떤 이미지를 기반ㅂ으로 할지 설정합니다. Docker 이미지는 기존에 만들어진 이미지를 기반으로 생성합니다. <이미지 이름>:<태그> 형식으로 설정합니다.
     - maintainer : 메인테이너 정보입니다.
     - RUN : 셸 스크립트 혹은 명령을 실행합니다.
       > 이미지 생성 중에는 사용자 입력을 박을 수 없으므로 apt-get install 명령세서 -y 옵션을 사용합니다. (yum install도 동일)
       > 나마지는 nginx 설정
     - VOLUME : 호스트와 공유할 디렉터리 목록입니다. docker run 명령에서 -v 옵션으로 설정할 수 있습니다.
       > -v /root/data:/data는 호스트의 /root/data/ 디렉터리를 Docker 컨테이너의 /data 디렉터리에 연결
     - CMD : 컨테이너가 시작되었을 때 실행할 실행 파일 또는 셰 스크립트 입니다.
     - WORKDIR : CMD 에서 설정한 실행파일이 실행괼 디렉터리 입니다.
     - EXPOST : 호스트와 연결할 포트 번호 입니다.

     ```bash
     FROM ubuntu:14.04
     LABEL maintainer="jgi92@naver.com"

     RUN apt-get update
     RUN apt-get install -y nginx
     RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
     RUN chown -R www-data:www-data /var/lib/nginx

     VOLUME [ "/data", "/etc/nginx/site-enabled", "/var/log/nginx" ]

     WORKDIR /etc/nginx

     CMD ["nginx"]

     EXPOSE 80
     EXPOSE 433
     ```

- ### 3.2) build 명령으로 이미지 생성하기
  1. Dockerfile을 작성하였으면, 이미지를 생성합니다. Dockerfile이 저장된 example 디렉터리에서 다음 명령을 실행합니다.
     ```bash
     ~$ docker build --tag hello:0.1 .
     ```
     docker build <옵션> <Dockerfile 경로> 형식입니다. --tag 옵션으로 이미지 이름과 태그를 설정할 수 있입니다. 이미지 이르만 설정하면 태그는 lastest로 설정됩니다.
  2. 실행하기
     ```bash
     $ docker run --name <실행시키고 싶은 이름> -d -p 80:80 -v /root/data:/data <이지미 경로>
     ```
     - -d 옵션은 컨테이너를 백그라운드로 실행합니다.
     - -p 80:80 옵션으로 호스트의 80번 포스트와 컨테이터의 80번 포트를 연결하고 외부에 노출합니다.
       이렇게 설정한뒤 80번 포트로 접근하면 접속이 됩니다.
     - -v /root/data:/data 옵션으로 호스트의 /root/data 디렉터리를 컨테이너의 ./data 디렉터리에 연결 합니다. /root/data 디렉터리에 파일을 넣으면 컨테이너에서 해당 파일을 읽을 수 있습니다.

# 4. 도커 살펴보기

- 이미지와 컨테이너의 정보를 조회하는 방법.
- 컨테이너에서 파일을 꺼내는 방법.
- 변경된 파일을 확인하는 방법.
- 변경 사항을 이미지로 저장하는 방법.

### 4.1) history 명령으로 이미지 히스토리 살펴보기

1. 이미지 히스토리 조회 해보기
   ```bash
   $ docker history <이미지 이름> :<태그>
   ```

### 4.2) cp 명령으로 파일 꺼내기

1. hello-nginx 컨테이너에서 파일 꺼내보기

- docker cp <컨테이너 이름>:<경로> <호스트 경로>
  ```bash
  $ docker cp hello-nginx:/etc/nginx/nginx.conf ./
  ```

### 4.3) commit 명령으로 컨테이너의 변경사항을 이미지로 생성하기

1. docker commit 명령은 컨테이너의 변경 사항을 이미지 파일로 생성한다.

- docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그> 형식입니다. 컨테이너 이름 대신 컨테이너 ID를 사용해도 됩니다.
- -a "jgi92@naver.com"과 -m "add hello.txt" 옵션으로 커밋한 사용자와 로그 메시지를 설정합니다. hello-nginx 컨테이너를 hello:0.2 이미지로 생성
  ```bash
  $ docker commit -a <작성 maintainer> -m "<로그파일.txt>" <컨테이너 이름> <뉴 이미지 이름>:<태그>
  ```

### 4.4) diff 명령으로 컨테이너에서 변경된 파일 확인하기

1. docker diff 명령은 컨테이너가 실행되면서 변경된 파일 목록을 출력

- A(추가된 파일), C(변경된 파일), D(삭제된 파일)
- docker diff <컨테이너 이름>
  ```bash
  $ docker diff happygi
  ```

### 4.5) inspect 명령으로 세부 정보 확인하기

1. docker inspect 명령은 이미지와 컨테이너의 세부 정보를 출력합니다.

- docker inspect <컨테이너 이름>
  ```bash
  docker inspect happygi
  ```
