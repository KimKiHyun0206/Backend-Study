# dockerfile 이란?
* 도커의 이미지를 작성할 수 있는 기능을 가진 파일이다
* 이미지는 스크립트를 기반으로 생성된다
* 도커는 그 이미지가 dockerfile 문법으로 이루어져 있다.
* dockerfile을 활용하여 자신만의 이미지를 만들 수 있고, 보통 배포를 위해 많이 사용한다.


<br>

# dockerfile 기본 문법

## FROM
> 베이스 이미지 지정 명령이며, Dockerfile에 반드시 있어야 하는 명령어이다

```dockerfile
FROM httpd:myapp
LABEL maintaioner="rlarlgus0206@naver.com"
LABEL version="1.0.0"
LABEL description="Sample DockerFile"
```
* 도커는 계속해서 층으로 여러 겹 이미지를 쌓는 개념이다.
* 베이스 이미지한 그 층 중 가장 밑바닥의 기본이 되는 이미지이고, FROM으로 이미지를 지정해준다

## LABEL
> 메타 데이터를 넣을 수 있는 기능이다

보통 저자, 버전, 설명, 작성일자 등을 각각 key로 지정하고, 오른쪽에 설명 값을 넣는다

```dockerfile
FROM httpd:myapp
LABEL maintaioner="rlarlgus0206@naver.com"
LABEL version="1.0.0"
LABEL description="Sample DockerFile"
```

## CMD
> 해당 도커파일로 만든 이미지를 기반으로 컨테이너를 만들었을 때, 해당 컨테이너가 실행될 때 가장 먼저 실행될 프로그램을 기술한다

* CMD는 하나의 도커파일에서 한 가지만 설정되며 만약 CMD가 여러 개일 경우 마지막에 설정된 CMD만 설정이 적용된다

기술하는 방식은 크게 세 가지가 있다
### 명령어, 인자를 리스트처럼 작성하는 형태
```dockerfile
CMD ["bin/sh", "-c", "echo", "Hello"]
```
1. bin/sh : sh 프로그램에서 명령을 실행한다 
2. -c : 쉘 명령을 터미널 상에서 받지 않고 인자로 받는다
3. echo Hello : 라는 명령어를

만약 위와 같이 쓰지 않고 단순히 CMD["echo"]라고 쓸 경우 쉘에서 echo 명령이 실행되는 것이 아니라 직접 해당 명령어가 실행된다.
따라서 정확하게 쉘(bin/sh)까지 지정해서 명령을 실행하는 것이 좋다.

### ENTRYPOINT 명령어에 인자를 리스트처럼 작성하여 넘겨주는 형태
작성하는 방식은 위와 동일하지만, 실행되는 방식이 다르다.
### 쉘 명령처럼 작성하는 형태
```dockerfile
CMD <command1> <param1> <param2>
```

## RUN
* 도커 명령어의 run과 다른 명령어이다
* 이 항목의 기능을 알기 위해서는 도커 이미지의 구성 방식을 알아야 한다

### 도커 이미지 구성 방식
1. 도커는 이미지를 생성할 때 여러 단계의 layer를 층층이 쌓아 이미지를 작성한다
2. RUN명령은 이미지 실행 시, layer를 만들 수 있는 단계로 보통 베이스 이미지에 패키지(프로그램)을 추가로 설치하여 새로운 이미지를 만들 때 사용한다
    * 이렇게 하면 특정 단계를 변경할 때, 전체 이미지를 다시 다운로드 받는 번거로움을 해결할 수 있다
```dockerfile
RUN apt-get update #패키지(프로그램) 정보 업데이트
RUN apt-get install -y apache2
```

## ENTRYPOINT
만약 도커파일로 이미지를 만든 다음 컨테이너를 `docker run`으로 생성하고 실행하면서 명령문을 같이 적으며 실행한다면, 기존 이미지에 있던 CMD 명령이 터미널 명령으로 덮어띄워진다.

하지만 이미지에 ENTRYPOINT항목이 있다면, `docker run`으로 실행하더라고 ENTRYPOINT의 명령에 영향을 미치지 못하고, ENTRYPOINT명령 뒤에 쭉 인자로 나열을 하게 된다

```dockerfile
FROM httpd:myapp

LABEL maintaioner="rlarlgus0206@naver.com"
LABEL version="1.0.0"
LABEL description="Sample DockerFile

ENTRYPOINT "bin/sh"
```
다음과 같이 bin/sh가 있는 도커파일을 만들었다고 가정하고 mydockerapp 이미지를 생성해본다
```
docker build --tag mydockerapp
```

생성한 이미지로 http1컨테이너를 /bin/sh -c httpd-foreground라는 명령어와 함께 생성한다

```
docker run -dit -p 9999:80 --name http1 mydockerapp /bin/sh -c httpd-fpreground
```
이렇게 해서 새로 생성한 컨테이너를 보면

```
"CMD: [
    "/bin/sh",
    "-c",
    "httpd-foreground"
],
...
"ENTRYPOINT": [
    "/bin/bash"
],
...
```

ENTRTPOINT의 명령이 그대로 뒤에 남아있다.
CMD가 ENTRYPOINT의 뒤에 인자처럼 붙게 되므로 이 컨테이너는
1. /bin/bash
2. /bin/sh
3. -c
4. httpd-foreground

위 순서로 실행될 것이다

## EXPOSE
> 도커 컨테이너의 특정 포트를 외부에 오픈하는 설정

`docker run -p`설정과 유사하다. 차이점은 `-p`옵션은 포트를 외부에 오픈하고, 해당 포트를 호스트 PC의 특정 포트와 매핑시키는 것까지는 알아서 해주지만,
EXPOSE는 단순히 포트를 열어주기만 한다는 것이다.

따라서 도커파일에 EXPOSE로 포트를 열어주더라도 `docker run`을 할 때 `-p`옵션을 써야한다
```
docker run -P -d mydockerapp
```
위와 같이 -p를 대문자로 쓴다면 EXPOSE로 열어준 포트를 호스트 PC의 랜덤 포트가 매칭된다.
* 따라서 실행할 때마다 달라진다

## ENV
> 컨테이너 내 환경변수 설정

설정한 환경변수는 RUN, CMD, ENTRYPOINT 명령에도 적용된다

```dockerfile
ENV MYSQL_ROOT_PASSWORD=password # mysql 슈퍼관리자인 root ID에 대한 password란에 패스워드 설정하기
ENV MYSQL_DATABASE=dbname # DB 지정해주기
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=pw # mysql 추가 사용자 ID, 패스워드 설정
```

## WORKDIR
> 이미지 내 특정 폴더로 이동하기 위해서 쓰는 명령

```dockerfile
FROM httpd:myapp
WORKDIR /usr/local/apache2/htdocs
CMD /bin/cat index/html # 이처럼 WORKDIR로 이동하는 방식 외에도, 절대 경로 직접 모두 써주는 방법도 존재한다
```

## 작성한 도커파일로 이미지 만들기
```
docker build --tag <짓고싶은 이미지명> 
docker build --tag <짓고싶은 이미지명> -f <Dockerfile명> 
```
둘 다 도커파일이 있는 폴더 내에서 이미지를 빌드한다는 조건이 있다.
단 도커파일 명이 그냥 Dockerfile이라면 -f 옵션이 없어도 되지만, 그 이외의 이름일 경우에는 -f 옵션으로 파일명을 명시해줄 수 있다

































