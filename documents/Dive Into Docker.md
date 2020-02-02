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

### What's a Container?

Container를 이해하기 위해서는 운영체제가 어떻게 돌아가는지에 대해 간략하게 이해해야합니다. 현재 컴퓨터에 Chrome, Terminal, NodeJS등등 여러개의 Process가 돌아가고 있다고 가정해봅시다. 그렇다면 이 프로세스들은 어떻게 CPU, Memory, Hard Disk와 의사소통을 할까요? 바로 Kernel이란 것을 통해 합니다. Kernel에서 해당 프로세스에는 얼마의 Memory를 할당하고 Hard Disk를 할당하고 하는 등의 작업을 진행합니다. 

이런 것을 크게 Name Spacing과 Control Groups 로 나눌 수 있습니다. (Linux에만 해당합니다.) Name Spacing은 쉽게 말해 프로세스들마다 사용하는 자원을 분리시키는 것이고 Control Groups는 해당 프로세스마다 사용할 자원의 양을 결정하고 제한합니다. 이러한 작업을 Kernel가 해주고 해당 컴퓨터 자원을 배분받음 프로세스는 그 자원위에서 돌아가게 됩니다. 이러한 일련의 프로그램이 자원을 할당받아 돌아가는 이 일련의 과정을 Container라고 합니다.