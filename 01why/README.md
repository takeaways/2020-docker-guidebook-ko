# Doker is...

### 도커를 사용하는 이유는?

1. 어떠한 환경에서도 구동할 수 있는 애플리케이션을 만들고자 한다!
2. 실행환경은 어디에서 실행 시키느냐 따라서 달라 질 수 있다.
   <br/>ex) 회사에서 mac으로는 개발을 잘 되었는데?? 내집 윈도우에서는 왜!! 안되는 거야!!

### 도커의 길

- use docker for apps in production (58%)

### install docker in Mac

```bash
$ brew cask install docker
```

### how to make image

1. 이미지를 만들기위한 작업 해보기 (기본)
   ```bash
   $mkdir hello-docker
   $cd hello-docker
   $code .
   //hello.txt 만들고 내용을 작성해 봅니다.
   ```
2. Dockerfile(작업지시서) 만들기

- 어떤 OS : [FROM 도커의 이미지]
- 어떤 명령어
- 어떤 파일들을 컨테이너 안으로 가져갈거야. : 컨테이너 안이 비워져 있기 때문에 호스트 pc에 있는 내용을 도커 컨테이너 안으로 가져 들어가서 작업을 실행할 수 있도록 처리 해줘야 한다.

  ```Dokerfile
  FROM centos:7 [layer]
  WORKDIR /app #root(/)를 기준으로 [/app] 경로가 .이된다.
  COPY name.txt .
  CMD "cat name.txt"
  ```

> 1. docker build -t [내 이름/이미지 이름:버전]
> 2. docker images

### 컨테이너 만들어 보기

1. docker run 이미지:버전

   - 파일이 실행된 다음에 할일이 없으면 종료된다.
   - docker ps 명령을 통하여 동작하고 있는 컨테이너를 확인가능하다.

2. docker exec -it containerID bash

   - 컨테이너 안으로 들어가보기

### Docker Hub 다른 사람이 만들어둔 이미지로 컨테이너 실행

1. docker pull busybox

```bash
$docker pull busybox
$docker run -it busybox
$
```

### Docker Port Binding

1. nginx : webserver
2. 요청하는 자원을 반환하는 역할을한다.
3. config
4. docker run 이미지이름 //없으면 허브에서 자동으로 받아온다.

```bash
        //내부 포트:호스트 포트
$docker run -p 80:80 nginx
```

### Docker Volume Mount

1. 컨테이너 안에있는 영역을 -> 호스트 특정영역에

   - 컨테이너에 있는 영역에서 쌓은 정보가 호스트 영역에도 같이 쌓인다.

```bash
              //호스트영역:컨테이너영역
$docker run -v ${PWD}/data:/data/db mongo
```

### 컨테이너 삭제

    1. redis [key:value]
    2. docker run -p 6379:6379 redis
    3. redis-cli
    4. set key value
    5. get key

### 지속적 통합(CI)

1. Continuous Integration
2. code ---- 테스트 ---- git
3. circle-ci travis-ci

### 지속적 배포(CD)
