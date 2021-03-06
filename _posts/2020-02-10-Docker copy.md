---
layout: post
title: "Docker"
description: 
headline: 
modified: 2020-02-10
category: webdevelopment
imagefeature: cover3.jpg
tags: [Docker]
mathjax: 
chart: 
share: true
comments: true
featured: true
disqus:
---


# Docker


## Introduction
-  가장 짧게 요약을 하자면 가상의 환경에서 미리 정해져있는 필요한 프로그램들의 리스트를 통째로 설치하고 실행한 뒤 가상의 환경을 종료시킴으로써 내 컴퓨터는 깔끔하게 아무 영향을 받지 않도록 해주는 녀석입니다.

## Concept
- Image, Docker engine, Container
- docker engine는 image를 통해 container라는 실행 객체를 생성하고 실행

## 구성요소
- Docker Client : 도커를 구성하고 운영 관리하는 패키지로 주로 관리자가 가장 많이 사용한다. docker cli 가 여기에 해당
- Docker Server : 도커를 실행하는 데몬이 동작하는 서버
    - 내부에는 이미지를 담아둘수 있는 볼륨이 필요하고, 해당 이미지는 내부 또는 외부의 repository 사용
    - 서버는 운영체제가 동작하고, 운영체제에서는 컨테이너 서비스를 제공하고 있어야 한다.
    - 도커 데몬이 동작해야 한다.
- 이미지 : 이미지는 로컬 또는 Docker Hb등과 같은 퍼블릭 Repository를 사용할수 있다.


## CentOS 설치
- yum-config-manager 유틸리티를 제공하는 yum-utils를 설치하십시오
    - yum install -y yum-utils
- 안정적인 저장소를 설정
    - yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
- docker-ce를 설치
    - yum install -y docker-ce repo

- 옵션 :  에지 저장소 (최신버전)으로 설정(활성화/비활성화)
    - sudo yum-config-manager --enable docker-ce-edge
    - sudo yum-config-manager --enable docker-ce-testing
    - sudo yum-config-manager --disable docker-ce-edge

- 옵션 : docker-ce 특정 버전 설치
    - yum install docker-ce-17.03.0.ce 

- systemctl 을 통해 서비스 구동 및 등록
    - sudo systemctl start docker 
    - sudo systemctl enable docker 
- 이미지 시작
    - docker run hello-world
    - 이미지가 없으면 Repo에서 Pull하고 실행까지 수행

## 도커 이미지와 컨테이너 삭제 방법
- docker ps : 동작중인 컨테이너 확인
- docker ps -a : 정지된 컨테이너 확인
- dockeer rm [컨테이너id] : 컨테이너 삭제
- docker images : 현재 이미지 확인
- docker rmi [이미지id] : 이미지 삭제
- docker rmi -f [이미지id] : 컨테이너, 이미지 동시 강제삭제


## 도커 명령어
- docker --version
- docker info
- docker search ngnix
- docker images
- docker create
- docker start
- docker stop
- docker restart
- docker ps
- docker kill
- docker attach
- docker exec
- docker run
- docker rm
- docker rmi
- docker save
- docker load
- docker export
- docker import
- docker history
- docker logs
- docker stats
- docker top





### docker ubuntu 예시
- docker pull ubuntu:latest
    - docker pull <이미지 이름>:<태그>
- docker images : 이미지의 목록을 출력
- docker run -i -t --name hello ubuntu /bin/bash : 이미지를 컨테이너로 생성한 뒤 Bash Shell을 실행
    - docker run <옵션> <이미지 이름> <실행할 파일> 형식입니다. 여기서는 ubunbu 이미지를 컨테이너로 생성한 뒤 ubuntu 이미지 안의 /bin/bash를 실행합니다. 이미지 이름 대신 이미지 ID를 사용해도 됩니다. 
    - -i(interactive), -t(Pseudo-tty) 옵션을 사용하면 실행된 Bash Shell에 입력 및 출력을 할 수 있습니다. 그리고 --name 옵션으로 컨테이너의 이름을 지정할 수 있습니다. 이름을 지정하지 않으면 Docker가 자동으로 이름을 생성하여 지정합니다.
    - 참고 : CentOS에서 아래와 같은 에러가 발생하면
    ```
        unable to remount sys readonly: unable to mount sys as readonly max retries reached
        /etc/sysconfig/docker 파일에서 아래와 같이 --exec-driver=lxc를 추가합니다.

        /etc/sysconfig/docker
        # /etc/sysconfig/docker
        #
        # Other arguments to pass to the docker daemon process
        # These will be parsed by the sysv initscript and appended
        # to the arguments list passed to docker -d

        other_args="--selinux-enabled --exec-driver=lxc"
        Docker 서비스를 재시작합니다.

        $ sudo service docker restart
    ```
- docker ps -a : 모든 컨테이너 목록을 출력
- docker stop hello
    - docker stop <컨테이너 이름>
- docker start hello
    - docker start <컨테이너 이름> 
- docker restart hello
    - docker restart <컨테이너 이름>
- docker attach hello : 컨테이너에 접속
    - docker attach <컨테이너 이름>
    - exit, Ctrl+D 를 컨테이너가 정지됨
    - Ctrl+P, Ctrl+Q 를 차례로 입력하여 컨테이너 정지없이 빠져나옴
- docker exec hello echo "Hello World" : 외부에서 컨테이너 안의 명령을 실행
    - docker exec <컨테이너 이름> <명령> <매개 변수>


### docker nginx 예시
- docker search --filter "is-official=true" nginx
- docker pull nginx
- docker run --name ngnix-test -p 8080:80 nginx
- curl http://127.0.0.1:8080
- docker ps
- docker network ls
- docker network inspect bridge


### docker 이미지 생성하기
- makir example
- cd example
- example/Dockerfile
```
    FROM ubuntu:14.04
    MAINTAINER Foo Bar <foo@bar.com>

    RUN apt-get update
    RUN apt-get install -y nginx
    RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
    RUN chown -R www-data:www-data /var/lib/nginx

    VOLUME ["/data", "/etc/nginx/site-enabled", "/var/log/nginx"]

    WORKDIR /etc/nginx

    CMD ["nginx"]

    EXPOSE 80
    EXPOSE 443
```
- docker build --tag hello:0.1 .
    - docker build <옵션> <Dockerfile 경로>
- docker images
- docker run --name hello-nginx -d -p 80:80 -v /root/data:/data hello:0.1
```
    -d 옵션은 컨테이너를 백그라운드로 실행합니다.

    -p 80:80 옵션으로 호스트의 80번 포트와 컨테이너의 80번 포트를 연결하고 외부에 노출합니다. 이렇게 설정한 뒤 http://<호스트 IP>:80에 접속하면 컨테이너의 80번 포트로 접속됩니다.

    -v /root/data:/data 옵션으로 호스트의 /root/data 디렉터리를 컨테이너의 /data 디렉터리에 연결합니다. /root/data 디렉터리에 파일을 넣으면 컨테이너에서 해당 파일을 읽을 수 있습니다.
```
- 웹 브라우저를 실행하고, http://<호스트 IP>:80으로 접속합니다. Welcome to nginx! 페이지가 표시될 것입니다.


### docker 기타 명령
- docker history hello:0.1 : 이미지의 히스토리를 조회
    - docker history <이미지 이름>:<태그>
- docker cp hello-nginx:/etc/nginx/nginx.conf ./ : hello-nginx 컨테이너에서 파일을 꺼내보겠습니다.
    - docker cp <컨테이너 이름>:<경로> <호스트 경로>
    - 현재 디렉터리에 nginx.conf 파일이 복사됨
- docker commit -a "Foo Bar <foo@bar.com>" -m "add hello.txt" hello-nginx hello:0.2 : 컨테이너의 변경 사항을 이미지 파일로 생성합니다.
    - docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그>
- docker diff hello-nginx : 컨테이너가 실행되면서 변경된 파일 목록을 출력
- docker inspect hello-nginx :  이미지와 컨테이너의 세부 정보를 출력합니다.

#### 컨테이너 연결하기
- 웹 서버 컨테이너와 DB 컨테이너가 있을 때 웹 서버 컨테이너에서 DB 컨테이너에 접근할 수 있어야 합니다. 이 때에는 docker run 명령에서 --link 옵션을 사용하여 컨테이너를 연결합니다.
    - docker run --name db -d mongo
    - docker run --name web -d -p 80:80 --link db:db nginx


## Docker NetWork
### Bridge
- ifconfig
- `iptables -t nat -L | more`
- docker ps
- brctl show
- docker network ls
- docker network inspect bridge
    ```JavaScript
    "IPAM": .... "Subnet":"172.17.0.0/16"
    "Containers": .... "IPV4Address":"172.17.0.2/16"
    ```
- ip a
- route -n


## Docker 이미지 관리
- docker login
- docker images
- docker tag nginx:latest mamma1234/nginx-test:v0.1
- docker images
- docker push mamma1234/nginx-test:v0.1
- docker pull mamma1234/nginx-test:v0.1


## Docker Dockerfile
##### FROM
- 기본 이미지를 지정한다. 최근에는 경량화된 운영체제인 Alpine Linux를 많이 사용함.
```JavaScript
    FROM "사용할 이미지 이름"
    $ FROM centos:latest'
```

##### MAINTAINER/LABEL
- 이미지를 생성한 저자에 대한 정보 및 라벨링을 할 수 있다.
최근에는 LABEL을 많이 사용한다.
```JavaScript
    MAINTAINER "이미지 생성한 사람 관련 정보"
    $ MAINTAINER PARK DAE KYU mamma1234@gmail.com
```

##### RUN
- 패키지 인스톨 및 Shell 명령을 기본이미지를 실행 할때, 함께 수행할 수 있다.
```JavaScript
    RUN "기본 이미지에서 스크립트 또는 명령 실행시키는 명령어"
    $ RUN yum -y update
    $ RUN yum -y install net-tools
```

##### CMD
- 이미지를 통해 컨테이너가 만들어지고 나서 가장 처음 실행될 명령어를 선언한다.
단 한번만 적용되기 때문에, 필요에 따라 지정해서 사용하면 된다.
```JavaScript
    CMD "컨테이너에서 실행할 명령어를 실행시키는 명령"
    $ CMD touch /home/test.txt
```

##### EXPOSE
- EXPOSE는 docker run --expose 옵션과 동일하여 호스트와 연결될 포트번호를 설정하는 명령이다.
```JavaScript
    EXPOSE 호스트와 연결할 포트번호를 설정하는 명령
    $ EXPOSE 8888 80
```

##### ENV
- 환경변수를 설정할 수 있다. Docker run -e와 유사한 결과를 가질수 있다.
```JavaScript
    ENV "환경변수"
    $ ENV nginx_vhost /etc/nginx/sites-available/default
```

##### ADD
- 현재 폴더 내부의 파일을 이미지에 추가할 수 있으며, 반드시 절대경로를 지정한다.
```JavaScript
    ADD http://test.com/test.txt /home/test/
    # Image 내부 /home/test 폴더에 test.txt를 저장
```

##### COPY
- ADD와 유사하지만 URL, tar에 대한 압축을 풀 수는 없다.

##### ENTRYPOINT
- 컨테이너가 실행될 때 사용할 명령어를 지정할 수 있다.

##### VOLUME
- 지정된 호스트 보륨에 특정 디렉토리를 저장할 수 있도록 마운트 시킬 수 있다.
```JavaScript
    VOLUME ["호스트 디렉토리", "이미지 내부 디렉토리"]
    $ VOLUME ["/home/parkdk", "/home/guest"]
```

##### USER
- RUN, CMD, ENTRYPOINT에서 실행될 계정을 선언한다.
```JavaScript
    USER "User Name"
    $ USER admin
```

##### ONBUILD
- ONBUILD는 최초 이미지를 생성할 때는 동작하지 않으며, 최초에 생성된 이미지에 바인딩되어 이후 해당 이미지를 부모 이미지로 기반으로 자식 이미지들을 만들 때 자동으로 바인딩 된다.
```JavaScript
    ONBUILD RUN touch /thisisremade.txt
```

##### WORKDIR
- 명령을 실행하게 되는 디렉토리를 설정하는 명령이다. 주로 RUN, CMD, ENTRYPOINT에서 설정한 실행 파일이 실행될 디렉토리가 된다.
```JavaScript
    WORKDIR "작업 디렉토리"
    $ WORKDIR "/home/parkdk"
```

##### Dockerfile 예제
```JavaScript
    # VERSION 1.0
    FROM alpine
    # alpine linux 이미지를 기본으로 사용

    MAINTAINER PARK DAE KYU, mamma1234@gmail.com 
    # 생성자 표시

    # basic package install, pip install

    RUN apk update \
    && apk upgrade \
    && apk add vim git python python-dev py-pip gcc g++ make bash \
    && pip install flask flask-admin flask-bootstrap flask-cors flask-httpauth flask-sqlalchemy flask-wtf gitpython \
    && pip install graphviz ipaddress jsonschema py-radix pymysql requests tabulate websocker-client deepdiff

    # Apline linux 생성 이후 설치 패키지

    # acitoolkit setup

    RUN git clone https://github.com/datacenter/acitoolkit.git \
    && cd /acitoolkit \
    && python ./setup.py install \
    && cd /acitoolkit \
    && python ./setup.py develop

    # nxtoolkit setup

    RUN git clone https://github.com/datacenter/nxtoolkit.git \
    && cd /nxtoolkit \
    && python ./setup.py install \
    && cd /nxtoolkit \
    && python ./setup.py develop

    # CiscoUCSM SDK setup

    RUN git clone https://github.com/CiscoUcs/ucsmsdk \
    && pip install ucsmsdk \
    && cd /ucsmsdk \
    && make install

    # vmware SDK pyvmomi setup

    RUN pip install pyvmomi \
    && git clone https://github.com/mamma1234/mamma1234_pyvmomi-community-samples.git

    WORKDIR /

    # 실행 디렉토리

    CMD ["bin/bash"]

    # docker run에서 실행될 때 가장 먼저 실행.

```

### docker Node.js 예시
- package.json 
```JavaScript
    {
        "name": "docker_web_app",
        "version": "1.0.0",
        "description": "Node.js on Docker",
        "author": "First Last <first.last@example.com>",
        "main": "server.js",
        "scripts": {
            "start": "node server.js"
        },
        "dependencies": {
            "express": "^4.16.1"
        }
    }
```

- server.js
```JavaScript
    'use strict';

    const express = require('express');

    // 상수
    const PORT = 8080;
    const HOST = '0.0.0.0';

    // 앱
    const app = express();
    app.get('/', (req, res) => {
    res.send('Hello World');
    });

    app.listen(PORT, HOST);
    console.log(`Running on http://${HOST}:${PORT}`);
```

- Dockerfile
```JavaScript
    FROM node:latest

    # 앱 디렉터리 생성
    WORKDIR /usr/src/app

    # 앱 의존성 설치
    # 가능한 경우(npm@5+) package.json과 package-lock.json을 모두 복사하기 위해
    # 와일드카드를 사용
    COPY package*.json ./

    RUN npm install
    # 프로덕션을 위한 코드를 빌드하는 경우
    # RUN npm ci --only=production

    # 앱 소스 추가
    COPY . .

    EXPOSE 8080
    CMD [ "node", "server.js" ]
```

- .dockerignore
```JavaScript
    node_modules
    npm-debug.log
```

- 이미지 빌드
    - docker build -t klnet.owner/klnet.owner.web .

- 이미지 실행
    - docker run -p 49160:8080 -d klnet.owner/klnet.owner.web
    - docker run -p 80:8080 -d klnet.owner/klnet.owner.web