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
  CMD ["cat name.txt"]
  ```

1. docker build -t [내 이름/이미지 이름:버전]
2. docker images
