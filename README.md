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
  | :------------------------: |:-------------|
  | > | 출력 리다이렉션, 명령 실행의 표춘 출력(stdout)을 파일로 저장합니다. 유닉스계열 운영체제는 장치도 파일로 처리하기 때문에 명령 실행 결과를 특정 장치로 보낼 수도 있습니다. <br/><pre><code> $ echo "hello" > ./hello.txt </code></pre><pre><code> $ echo "hello" > /dev/null|</code></pre>
  | < | 입력 리다이렉션, 파일의 내용을 읽어 명령의 표준 입력(stdin)으로 사용합니다. <br/> <pre><code>$ cat < ./hello.txt </code></pre>|  
   | >> | 명령 실행의 표준 출력(stdout)을 파일에 추가합니다. >는 이미 있는 파일에 내용을 덮어 쓰지만 >>는 파일 뒷 부분에 내용을 추가 합니다. <br/> <pre><code>$ echo "world" >> ./hello.txt</code></pre> |
  |2>| 명령 실행의 표준 에러(stderr)를 파일로 저장합니다.|
  |2>>| 명령 실행의 표준 에러(stderr)를 파일에 추가합니다.|
  |&>| 표준 출려과 표준 에러를 모두 파일로 저장합니다.|
  |1>$2| 표준 출력을 표준 에러로 보냅니다. echo 명령으로 문자열을 표준 출력으로 출력했지만 표준 에러로 보냈기 때문에 변수에는 문자열이 들어가지 않습니다. <br/> <pre><code> $ hello=$(echo "Hello WOrld" 1>&2)</code></pre><pre><code> $ echo \$hello</code></pre>|
  |2>&1|표준 에러를 표준 출력으로 보냅니다. abcd라는 명령은 없음으로 에러가 발생하지만 에러를 표준출력으로 보낸 뒤 다시 /dev/null로 보냈기 떄문에 아무것도 출력 되지 않습니다. <br/> <pre><code>$ abcd > /dev/null 2>&1</code></pre>|
  | \| | 파이프 명령 실행의 표준 출력을 다른 명령의 표준 입력으로 보냅니다. 즉 첫 번쨰 명령의 출력 같을 두 번 쨰 명령에서 처리합니다. <br/><pre><code> $ ls -al \| grep.txt </code></pre>|
  |\$|bash의 변수 입니다. 값을 저장할 떄는 $를 붙이지 않고, 변수를 가져다 쓸 떄만 \$를 붙입니다. <br/> <pre><code> $ hello= "Hello World" <br/> $ echo $hello <br/> Hello World</code></pre>|
  |$()|명령 실행결과를 변수화합니다. 명령 실행 결과를 변수에 저장하거나 다른 명령의 매개 변수로 넘겨줄 떄 사용합니다. 또는 문자열 안에 명령의 실행 결과를 넣을 때 사용합니다. <br/><pre><code> $ docker rm $(docker ps -aq) <br/> $ echo \$(date) </code></pre>|
  |``|$()꽈 마찬가지로 명령 실행 결과를 변수화합니다. <br/><pre><code> $ docker rm `docker ps -aq` <br/> $ echo \`date\` </code></pre>|
  |&&|한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 멸령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다. <br/><pre><code> $ make && make install </code></pre>|
  |;|한 줄에서 명령을 여러 개 실행합니다. 단 앞에 있는 명령이 에러 없이 실행되어야 뒤에 오는 명령이 실행됩니다. <br/><pre><code> $ false; echo "Hello" </code></pre>|
  |''|문자열입니다. ' '안에 들어있는 변수는 처리되지 않고 변수명 그대로 사용됩니다. 또한 "와 $()도 처리되지 않고 그대로 사용됩니다. <br/><pre><code> $ echo '$USER'<br/> \$USER<br/></code></pre>그대로 출력됩니다.|
  |" "|문자열입니다. 명령에 문자열 매개 변수를 입력하거나 변수에 저장할 때 주로 사용합니다. '' 와는 달리 " "안에 변수가 들어 있으면 변수의 내용으로 바뀝니다. 또한 "와 $()도 실행 결과 값이 사용됩니다. <br/><pre><code> $ echo "Hello world" <br/> $ echo "$USER" </code></pre>|
  |"''"|""안에 ''가 들어갈 수 있습니다. 명령 안에서 다시 명령을 실행하고 매개 변수를 지정할 떄 사용합니다.<pre><code> $ bash -c "/bin/echo Hello 'World" </code></pre>|
  |\"<br/>\$hello|''안에서 "를 사용할 때 \"처럼 앞에 \를 붙여줍니다. <br/><pre><code> $ bash -c "/bin/echo '{\"user\":\"$USER\"}'" </code></pre>|
  |${}|변수 치환입니다. " "문자열 안에서 변수를 출력할 때 주로 사용합니다. ${} 대신 $만 사용해도 됩니다.<br/><pre><code> $ strt="World"<br/> $ echo "Hello ${str}" </code></pre>스크립트에서 변수의 기본 값을 설정할 떄도 사용합니다. 다음은 HELLO변수가 있으면 그대로 사용하고 변수가 없으면 기본 값으로 설정한 abcd를 대입합니다.<br/><pre><code> $ HELLO=<br/> $ HELLO=${HELLO-"abcd"}<br/> $ echo $HELLO</code></pre>값이 NULL인 HELLO 변수가 이미 있기 때문에 기본 값을대입하지 않습니다. 다음은 변수의 값이 있으면 그대로 사용하고, 값이 NULL이면 기본 값으로설정한 abcd를 대입합니다.<br/><pre><code> $ WORLD=<br/> $ WORLD=${WORLD:-"abcd"}<br/> $ echo \$WORLD </code></pre>|
  | \\ |한 줄로된 명령을 여러 줄로 표현할 때 사용합니다. <br/><pre><code> $ dockerrun -d -name hello busybox:latest <br/> $ docker run \\<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-d \\<br/>&nbsp;--name hello \\<br/>busybox:latest </code></pre>|
  |{1..10}|연속된 숫자를 표현합니다. {시작숫자..끝 숫자} 형식|
  |{문자열1, 문자열2}|{}안에 문자열을 여러 개 지정하여 명령 실행 횟수를 줄입니다. 다음은 hello.txt, world.txt 두 파일을 한 번에 hello-dir디렉 터리에 복사합니다. <br/><pre><code> $ cp ./{hello.txt, world.txt} hello-dir/ </code></pre>|
  |if| if 조건문입니다. 변수와 변수끼리 또는 문자열과 비교할 때 사용합니다. <br/><pre><code> if [$a -eq $b]; then <br/>   echo \$a<br/> fi<br/></code></pre><b>숫자 비교</b><ul><li> -eq : 같다</li><li>-ne : 같지 않다.</li><li>-gt : 초과</li><li>-ge : 이상</li><li>-lt : 미만</li><li>-le : 이하</li></ul><br><b>문자열 비교</b><ul><li> =, == : 같다</li><li>!= : 같지 않다</li><li>-z : 문자열이 NULL 일 때</li><li>-n : 문자열이 NULL이 아닐 때</li></ul>|
  |for|for 반복문입니다. 변수 안에 있는 값을 반복하거나 범위를 지정하여 반복할 수 있습니다.<br/><pre><code>for i in ${ls}<br/>do<br>&nbsp;&nbsp;echo $i<br/>done</code></pre>|
  |while|while 반복문입니다. <br/><pre><code> while : <br/>&nbsp;&nbsp;&nbsp;echo "Hello World";<br/> sleep 1;<br> done </code></pre>|
  |<<<|문자열을 명령(프로세스)의 표준 입력으로 보냅니다.<br/><pre><code> $ cat <<< "User name is $USER"<br/>&nbsp;&nbsp;&nbsp;User name is myname </code></pre>|
  |<<EOF<br/>EOF|여러 줄의 문자열을 명령의 표준 입력으로 보냅니다.<br/><br/><pre><code> $ cat > ./hello.txt <<EOF<br/>&nbsp;&nbsp;&nbsp;&nbsp;Hello World<br>&nbsp;&nbsp;&nbsp;&nbsp;Host name is \$(hostname)<br/>&nbsp;&nbsp;&nbsp;&nbsp;User name is \$(USER)<br/>&nbsp;&nbsp;&nbsp;EOF<br/><br/></code></pre>cat은 파일이나 표준 입력의 내용을 출력하는 명령입니다. cat의 표준 출력을 ./hello.txt로 저장하고, <<EOF로 문자열을 cat의 표준 입력으로 보냅니다. 이렇게 하면 문자열 3줄이 ./hello.txt 파일에 저장됩니다.|
  |export|설정한 값을 환경 변수로 만듭니다. export <변수>=<값> 형식<br/>\$ export HELLO=world|
  |printf|지정한 형식대로 값을 출력합니다. 파이프와 연동하여 명령(프로세스)에 값을 입력하는 효과를 낼 수 있습니다.<br/><pre><code> $ print 80\\neampleuser\\ny \| example-config<br/>&nbsp;&nbsp;&nbsp;PORT: 80<br/>&nbsp;&nbsp;&nbsp;User: exampleuser<br/>&nbsp;&nbsp;&nbsp;Save Configuration (y/n): y </code></pre>예를 들어 example-config는 Port, User, Save Configuration을 사용자에게 입력을 받습니다. printf로 미리 값을 설정하여 파이프로 example-config에 넘겨주면 사용자가 입력하지 않아도 자동으로 값이 입력되니다. 줄바꿈(개행)은\\n 으로 표현합니다.|
  |sed|텍스트 파일에서 문자열을 변경합니다. hello.txt파일의 내용 중에서 hello하는 문자열을 찾아서 world 문자열로 바꾸려면 다음과 같이 실행합니다.<br/><pre><code>\$ sed -i "s/hello/world/g" hello.txt<code/></pre>|
