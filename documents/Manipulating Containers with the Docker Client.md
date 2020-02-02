# Manipulating Containers with the Docker Client

### Overriding Default Commands

```powershell
$ docker run hello-world
```

docker는 명령어가 이렇게 구성됩니다. `docker run [이미지 이름]` 순으로 구성되고 그 다음에는 명령어를 실어서 보낼 수 있습니다.

```powershell
$ docker run busybox echo hi there
hi there
```

이렇게 busybox에는 `echo hi there` 명령어를 같이 전달하면 결과가 콘솔창에 뜨는 것을 확인할 수 있습니다.

```powershell
$ docker run hello-world echo hi there
docker: Error response from daemon: OCI runtime create failed: container_linux.go:346: starting container process caused "exec: \"echo\": executable file not found in $PATH": unknown.
```

그런데 hello-world에서는 작동하지 않고 에러가 발생하는 것을 확인할 수 있습니다. 그 이유는 무엇일까요?

```powershell
$ docker run busybox ls
bin
dev
etc
home
proc
root
sys
tmp
usr
var
```

busybox image에는 여러개의 폴더들이 file system상 설치되어 있고 container가 만들어 질 때도 위 폴더들이 그대로 적용됩니다. 이 폴더 안에 해당 명령어를 읽을 수 있는 파일들이 들어있을테고 `ls, echo` 와 같은 명령어들을 실행할 수 있게 되는 것입니다. 하지만 hello-world image에는 정해진 text만을 보여주는 동작만 설정되어 있기 때문에 실행했을 때 해당 명령어를 읽어내지 못하고 에러가 발생한 것입니다.

```powershell
$ docker run hello-world ls
docker: Error response from daemon: OCI runtime create failed: container_linux.go:346: starting container process caused "exec: \"ls\": executable file not found in $PATH": unknown.
```

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

### Listing Running Containers

현재 돌아가고 있는 container를 보려면 

```powershell
$ docker ps
```

이와 같은 명령어를 입력하면 됩니다.

```powershell
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

하지만 아무런 container도 없는 것을 확인할 수 있습니다. 왜냐하면 지금까지 작성한 container는 모두 한번 동작하고 종료되는 container이었기 때문입니다. 따라서 어느 정도 시간동안 동작하는 명령을 내리도록 하겠습니다.

```powershell
$ docker run busybox ping google.com
```

위 명령어를 치고 다른 터미널을 열어 container를 확인해봅니다.

```powershell
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
211eca35b4e0        busybox             "ping google.com"   15 seconds ago      Up 12 seconds                           adoring_noyce
```

CONTAINER ID는 해당 container의 id를 의미합니다. IMAGE는 이 container가 어떤 image의 instance인지 알려주며 COMMAND는 어떤 명령어를 입력했는지 나타냅니다. CREATED는 언제 이 container가 생성됐는지, STATUS는 현재 상태, PORTS는 port, 번호 NAMES는 docker에서 부여하는 다른 container와 구분짓기 위한 고유한 이름입니다.

`docker ps --all` 명령어를 치면 지금까지 생성한 container를 모두 확인할 수 있습니다.

```powershell
$ docker ps --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
211eca35b4e0        busybox             "ping google.com"   4 minutes ago       Exited (0) 4 minutes ago                        adoring_noyce
2a234b85c6bb        hello-world         "/hello"            7 minutes ago       Exited (0) 7 minutes ago                        thirsty_faraday
9a5099360afc        hello-world         "ls"                8 minutes ago       Created                                         vibrant_goldstine
47d736b6557c        busybox             "ls"                11 minutes ago      Exited (0) 11 minutes ago                       tender_cerf
5af4e1eff1e9        hello-world         "echo hi there"     12 minutes ago      Created                                         wonderful_maxwell
484156be7870        busybox             "echo hi there"     16 minutes ago      Exited (0) 16 minutes ago                       charming_curran
c79f7e8f1ba6        busybox             "ls"                25 minutes ago      Exited (0) 25 minutes ago                       sleepy_blackwell
38952aff63b8        busybox             "ls"                26 minutes ago      Exited (0) 26 minutes ago                       inspiring_ishizaka
924da3db5792        busybox             "echo bye there"    27 minutes ago      Exited (0) 27 minutes ago                       trusting_ganguly
e83feb2b6f2c        busybox             "echo hi there"     27 minutes ago      Exited (0) 27 minutes ago                       hungry_burnell
a48a2a871eb3        hello-world         "/hello"            33 minutes ago      Exited (0) 33 minutes ago                       objective_meitner
c89d8d4bb866        hello-world         "/hello"            8 hours ago         Exited (0) 8 hours ago                          flamboyant_feynman
749a70e8c8c9        hello-world         "/hello"            9 hours ago         Exited (0) 9 hours ago                          kind_chaum
5d6bcb0bd0c4        hello-world         "/hello"            9 hours ago         Exited (0) 9 hours ago                          strange_cannon
9e378ea4b287        hello-world         "/hello"            9 hours ago         Exited (0) 9 hours ago                          hungry_ardinghelli
e7445dc569a4        hello-world         "/hello"            10 hours ago        Exited (0) 10 hours ago                         charming_neumann
dd3262f622cb        hello-world         "/hello"            10 hours ago        Exited (0) 10 hours ago                         infallible_swirles
```

### Container LifeCycle

`docker run`  `docker create` 명령어와 `docker start` 명령어를 모두 실행하는 명령어입니다. `docker create <image name>` `docker start <container id>` 이렇게 구성됩니다. create 명령어는 해당 image의 File System의 snapshot을 가져와 container에 setting하는 것까지를 의미합니다. start 명령어는 해당 startup command 또는 인자로 넘긴 command를 실행해 container를 돌리는 것을 의미합니다.

```powershell
$ docker create hello-world
e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
```

docker start 명령어는 start와 container id 사이에 -a를 입력해야 합니다.

```powershell
$ docker start -a e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4

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

입력을 하지 않으면 아래와 같이 container id만 콘솔창에 찍혀서 나타납니다.

```powershell
$ docker start e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
e794f746892482b3190d40a55d2248a96fb85be4ae813e04da10a0e510f476c4
```

`-a` 명령어는 docker가 container 실행시, 실제 ouput이 보여지게끔 하는 역할을 합니다.