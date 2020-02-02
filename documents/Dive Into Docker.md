# Dive Into Docker

### Using the Docker Client

```powershell
$ docker run hello-world
```

linux에 도커 설치를 완료하고 위와 같은 명령어를 치면 docker client가 docker server에 요청을 보냅니다. docker server는 우선 해당 컴퓨터 안에서 hello-world image가 있는지 찾습니다. 물론 방금 막 설치한 docker에 hello-world image는 존재하지 않기에 docker server는 docker hub에서 image를 찾습니다. docker hub에서 hello world image를 찾은 docker server는 이를 컴퓨터에 다운로드 받고 해당 image로 부터 container를 생성하고 실행합니다.

```powershell
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

그러면 해당 console를 위와 같이 확인할 수 있습니다. 